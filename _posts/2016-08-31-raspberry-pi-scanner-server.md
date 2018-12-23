---
layout: post
title: Raspberry Pi Scanner Server
date: 2016-08-31 22:27:38.000000000 -04:00
author: Michael J. Ford
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- CLI Fu
- Linux
- Networking
- Raspberry PI
- Runs Linux
- Servers
tags:
- CLI
- Linux
- Raspberry Pi
- scanners
- Ubuntu
meta:
  _edit_last: '1'
  _responsive_layout: default
  _syntaxhighlighter_encoded: '1'
  _wpas_done_all: '1'
---
<p>It's come time that we (my fiancé and I) required the use of a color printer. My B/W Laser wasn't going to cut it this year for her class room printings. So we went out an picked up an Epson WP-3620 on sale at BestBuy. It's an All-In-One like most [read: ALL] consumer printers you'll find on the market today, this one even came with an Ethernet port (a requirement on my network :) ). The printer portion of the printer setup just fine as one would expect - but network scanning has been an issue. Not because I have it setup as a wired printer, but because my quest for the perfect network I have ALL network peripherals / IoT devices on a separate VLANs (if you question why, <a href="http://www.extremetech.com/computing/106945-tens-of-millions-of-hp-laserjet-printers-vulnerable-to-hacking">here's</a> an <a href="https://www.opswat.com/blog/have-printers-become-gateway-malware">example</a>, and <a href="https://www.theguardian.com/technology/2012/jul/23/hacking-attack-printers">another</a>, and <a href="https://security.stackexchange.com/questions/23691/attacking-an-office-printer">another</a>). However by doing this and making my network more secure, the secret sauce Epson uses to make network scanning ... well ... work ... is defeated. I assume it uses some form of NetBIOS as the port is listening and Wireshark shows lots of NB packets during the setup, but I cannot seem to get it to work properly without it being on the same subnet as my production rigs (maybe WiFi direct would work - but I disabled that). SANE straight will not connect to it via the network, however USB works fine and is officially <a href="http://www.sane-project.org/sane-mfgs.html">supported</a>.</p>
<p>So to make scanning work (and please my future wife) I did what I do best, engineer a solution.</p>
<p>Putting my RPi Model B to good use (It's not really up to much these days). Let's run <a href="https://www.raspberrypi.org/downloads/raspbian/">Raspbian Lite</a> and SANE, then wait for scanner requests on the network. This How-to should work on any Linux computer with a USB and Ethernet port.</p>
<p><!--more--></p>
<p><strong>Prerequisites</strong></p>
<p>I'll assume you know how to install an OS on a Raspberry Pi - that's been done to death already - so I won't go into detail here</p>
<p>But here's a few links to get you started if this is your first foray into Linux:</p>
<ul>
<li>Images: <a href="https://www.raspberrypi.org/downloads/raspbian/">https://www.raspberrypi.org/downloads/raspbian/</a></li>
<li>Installer: <a href="https://www.raspberrypi.org/documentation/installation/installing-images/README.md">https://www.raspberrypi.org/documentation/installation/installing-images/README.md</a></li>
<li>Compatible Scanners: <a href="http://www.sane-project.org/sane-mfgs.html">http://www.sane-project.org/sane-mfgs.html</a></li>
</ul>
<p>Also as a side note, I use vim as my preferred editor. Replace vim with which ever text editor of your choosing; e.g. nano.</p>
<p><strong>Installing Required Packages</strong></p>
<p>Other than the basics included in Raspbian Lite, we need sane-utils; that's it!! sane-utils contains the saned. (On Archlinux the package is called simply “sane”)
</p><pre>pi@scan:~ $ sudo apt-get install sane-utils</pre>
<p>This will take a bit to install if you're on an old Pi like mine - go grab a cup of coffee if that's your thing.</p>
<p><strong>Scanner Setup<br>
</strong></p>
<p>Configure /etc/default/saned so it will start automatically (RUN=yes):
</p><pre>pi@scan:~ $ sudo vim /etc/default/saned

# Defaults for the saned initscript, from sane-utils

# To enable under systemd please read README.Debian
# Set to yes to start saned under SysV
RUN=yes

# Set to the user saned should run as
RUN_AS_USER=saned
</pre>
<p>Now run <code>scanimage -L</code>, you should see your scanner. If not google your scanner model and linux sane for more information.</p>
<p>In my case for the Epson WP-3620 Scanner (Printer), add <code>usb 0x4b8 0x8b8</code> to /etc/sane.d/epson2.conf so it looks like below (use lsusb to obtain the USB productID and deviceID as yours could be different).
</p><pre># epson2.conf
#
# here are some examples for how to configure the EPSON2 backend

# SCSI
scsi EPSON
# for the GT-6500:
#scsi "EPSON SC"

# Parallel port
#pio 0x278
#pio 0x378
#pio 0x3BC

# USB
usb

# For libusb support for unknown scanners use the following command
# usb &lt;product ID&gt; &lt;device ID&gt;
# e.g.:
# usb 0x4b8 0x110

usb 0x4b8 0x8b8

# Network
#
# net 192.168.1.123
net autodiscovery
</pre>
<p>Now scanimage will have some output:
</p><pre>pi@scan:~ $ sudo su -s /bin/sh saned
$ scanimage -L
device `epson2:libusb:001:004' is a Epson PID 08B8 flatbed scanner
$ exit
</pre>
<p>Note: I did have a bit of trouble above, because this is an all-in-one printer/scanner, the device group is lp rather than saned or scanner. Simply run the following to add the group lp to saned.
</p><pre>usermod -a -G lp saned
groups saned</pre>
<p><strong>Scanner Service Setup</strong></p>
<p>Next we'll setup saned.socket to listen on port 6566 to accept scan requests over the network. Start by checking /etc/services to ensure
</p><pre>pi@scan:~ $ cat /etc/services | grep sane
sane-port	6566/tcp	sane saned	# SANE network scanner daemon
</pre>
<p>If you don't see sane-port listed append the output above to /etc/services.</p>
<p>Now edit /etc/sane.d/saned.conf to allow connections from your network; such as 192.168.1.0/24 - under scan-client.somedomain.firm.
</p><pre>pi@scan:~ $ sudo vim /etc/sane.d/saned.conf</pre>
<pre># saned.conf
# Configuration for the saned daemon

## Daemon options
# Port range for the data connection. Choose a range inside [1024 - 65535].
# Avoid specifying too large a range, for performance reasons.
#
# ONLY use this if your saned server is sitting behind a firewall. If your
# firewall is a Linux machine, we strongly recommend using the
# Netfilter nf_conntrack_sane connection tracking module instead.
#
# data_portrange = 10000 - 10100


## Access list
# A list of host names, IP addresses or IP subnets (CIDR notation) that
# are permitted to use local SANE devices. IPv6 addresses must be enclosed
# in brackets, and should always be specified in their compressed form.
#
# The hostname matching is not case-sensitive.

#scan-client.somedomain.firm
192.168.1.0/24
#192.168.0.1/29
#[2001:db8:185e::42:12]
#[2001:db8:185e::42:12]/64

# NOTE: /etc/inetd.conf (or /etc/xinetd.conf) and
# /etc/services must also be properly configured to start
# the saned daemon as documented in saned(8), services(4)
# and inetd.conf(4) (or xinetd.conf(5)).
</pre>
<p>Please adjust the subnet specification <em>192.168.1.0/24 </em>to your network architecture. You can also explicitly specify single IPs. If you do not edit this file, SaneTwain will hang when contacting your Raspberry Pi scanner server.</p>
<p>The latest Raspbian images now have systemd, enable and restart the saned.socket unit.
</p><pre>pi@scan:~ $ sudo systemctl enable saned.socket
pi@scan:~ $ sudo systemctl restart saned.socket</pre>
<p>Now you can reboot the Raspberry Pi (<em>sudo reboot</em>).</p>
<p>From a remote machine run the following to test the connection:
</p><pre>mford@reaper:~$ telnet scan 6566
Trying 10.13.37.94...
Connected to scan.
Escape character is '^]'.
</pre>
<p>If you receive "Connected to [Hostname|IpAddr]" then the configuration is a success!!!</p>
<p>You should now have a networked scan server. Next up is configuring clients. But I'll save that for the next blog post.</p>
<p>Cheers!!</p>
