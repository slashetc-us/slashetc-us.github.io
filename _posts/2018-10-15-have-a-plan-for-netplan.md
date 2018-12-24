---
layout: post
title: Have a Plan for Netplan
date: 2018-10-15 13:08:08.000000000 -04:00
author: TT-RSS Published Article
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- TT-RSS Published Articles
tags: []
meta:
  _wpas_done_all: '1'
---
<div>
<div>
<div>
<div>  <img src="{{ site.baseurl }}/assets/bigstock-Computer-Network-Background-1166097.jpg" alt="" /></div>
<div>by <a title="View user profile." href="https://www.linuxjournal.com/users/shawn-powers" rel="noopener noreferrer" target="_blank">Shawn Powers</a></div>
<div>
<p><em>Ubuntu changed networking. Embrace the YAML.</em></p>
<p>
If I'm being completely honest, I still dislike the switch from <code>eth0,<br />
eth1, eth2</code> to names like, <code>enp3s0, enp4s0, enp5s0</code>. I've learned to accept<br />
it and mutter to myself while I type in unfamiliar interface names. Then I<br />
installed the new LTS version of Ubuntu and typed <code>vi<br />
/etc/network/interfaces</code>. Yikes. After a technological lifetime of entering<br />
my server's IP information in a simple text file, that's no longer how<br />
things are done. Sigh. The good news is that while figuring out Netplan for<br />
both desktop and server environments, I fixed a nagging DNS issue I've had<br />
for years (more on that later).</p>
<h3>
The Basics of Netplan</h3>
<p>
The old way of configuring Debian-based network interfaces was based on the<br />
<code>ifupdown</code> package. The new default is called Netplan, and<br />
although it's not<br />
terribly difficult to use, it's drastically different. Netplan is sort of<br />
the interface used to configure the back-end d&aelig;mons that actually<br />
configure the interfaces. Right now, the back ends supported are<br />
NetworkManager and <code>networkd</code>.</p>
<p>
If you tell Netplan to use NetworkManager, all interface configuration<br />
control is handed off to the GUI interface on the desktop. The<br />
NetworkManager program itself hasn't changed; it's the same GUI-based<br />
interface configuration system you've likely used for years.</p>
<p>
If you tell Netplan to use <code>networkd</code>, systemd itself handles the interface<br />
configurations. Configuration is still done with Netplan files, but once<br />
"applied", Netplan creates the back-end configurations systemd requires. The<br />
Netplan files are vastly different from the old /etc/network/interfaces<br />
file, but it uses YAML syntax, and it's pretty easy to figure out.</p>
<h3>
The Desktop and DNS</h3>
<p>
If you install a GUI version of Ubuntu, Netplan is configured with<br />
NetworkManager as the back end by default. Your system should get IP<br />
information via DHCP or static entries you add via GUI. This is usually not<br />
an issue, but I've had a terrible time with my split-DNS setup and<br />
<code>systemd-resolved</code>. I'm sure there is a magical combination of configuration<br />
files that will make things work, but I've spent a lot of time, and it<br />
always behaves a little oddly. With my internal DNS server resolving domain<br />
names differently from external DNS servers (that is, split-DNS), I get random<br />
lookup failures. Sometimes <code>ping</code> will resolve, but<br />
<code>dig</code> will not. Sometimes<br />
the internal A record will resolve, but a <code>CNAME</code> will not. Sometimes I get<br />
resolution from an external DNS server (from the internet), even though I<br />
never configure anything other than the internal DNS!</p>
</div>
<div>  <a href="https://www.linuxjournal.com/content/have-plan-netplan" rel="noopener noreferrer" target="_blank">Go to Full Article</a>
</div>
</p></div>
</p></div>
<p><img src="{{ site.baseurl }}/assets/3kEVYB8Z5S0" alt="" /></div>
<p>Read More <a href="https://www.linuxjournal.com/content/have-plan-netplan" target="_blank">here</a>.</p>
