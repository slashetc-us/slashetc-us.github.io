---
layout: post
title: Update Ubuntu Precise to 12.04.5 with Trusty HWE
date: 2014-08-11 20:02:29.000000000 -04:00
author: Michael J. Ford
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Linux
- Servers
- Ubuntu
tags: []
meta:
  _edit_last: '1'
  _responsive_layout: default
  _syntaxhighlighter_encoded: '1'
  _oembed_b1efa183620a436fc84c984b63c093cb: "{{unknown}}"
  _oembed_87640eca655acf6992d20d9b1e520a05: "{{unknown}}"
---
<p>Approximately two weeks ago I was met with the following message on one of my servers terminal after an upgrade and had no clue what to do:</p>

    Your current Hardware Enablement Stack (HWE) is going out of support
    on 2014-08-07. After this date security updates for critical parts (kernel
    and graphics stack) of your system will no longer be available.
    For more information, please see:
    http://wiki.ubuntu.com/1204_HWE_EOL
    To upgrade to a supported (or longer supported) configuration:
    * Upgrade from Ubuntu 12.04 LTS to Ubuntu 14.04 LTS by running:
    sudo do-release-upgrade
    OR
    * Install a newer HWE version by running:
    sudo apt-get install linux-generic-lts-trusty linux-image-generic-lts-trusty
    and reboot your system.

<p><em>DISLCAMER</em>: All normal warnings apply here. These commands and how tos are for edcuational use only. While commands work and are correct to the best of my knoledge: I am not responsible for your Linux crashing, loss of data, gnomes eating your sox in the dryer or your cat walking across the keyboard. With that said... Pleas read on...</p>
<p><!--more--><br>
What I found is simple. The Hardware Enablement Stack or HWE is ending support. What this means is what ever mainline kernel you happen to be using, such as 3.2.x or 3.5.x in Ubuntu 12.04 and point releases 3 and 4 will end support. Your system will still receive maintenance updates but kernel and graphics will be frozen. Point release 1 and 2 stay on mainline kernel 3.2 and do not have HWE but can be upgraded if needed/wanted. You can read more about HWE here and here.</p>
<p>Installing the latest HWE and mainline kernel 3.13.x is easy. What we need to do to get to Ubuntu 12.04.5 is manually update our systems. Canonical will not automaticlly push the updates, so we have to do it ourselfs or update to 14.04.1 to resolve the issue.</p>
<p>I chose the update to Trusty HWE and Ubuntu 12.04.5 as I have a few servers, and not enough time to roll out a new OS on them yet. Here is what I did to install the latest HWE:</p>
<p><strong><em>Updating SERVER:</em></strong></p>
<p>First you will want to install all updates pending for your installation:</p>

    sudo apt-get update ; sudo apt-get upgrade -y ; sudo apt-get dist-upgrade -y

<p>and reboot if a new kernel was installed. Then run exactly what the message states:</p>

    sudo apt-get install linux-generic-lts-trusty linux-image-generic-lts-trusty

<p>and reboot again. Once back up and running, you will be booted with a shiny new kernel and HWE. To test run.</p>

    mford@reaper:~$ hwe-support-status --verbose ; uname -srv ; lsb_release -a

<p>and your output should be the same or similar:</p>

    Your Hardware Enablement Stack (HWE) is supported until April 2017.
    Linux 3.13.0-32-generic #57~precise1-Ubuntu SMP Tue Jul 15 03:51:20 UTC 2014
    No LSB modules are available.
    Distributor ID: Ubuntu
    Description:    Ubuntu 12.04.5 LTS
    Release:        12.04
    Codename:       precise

<p><strong><em>Updating DESKTOP:</em></strong></p>
<p>Now updating a desktop system requires a bit more than a server. I got bit by this (but not too bad). What I did is run the same commands to update my desktop as my servers and.... this broke Unity.... lesson learned: Server != Desktop. I did however fix my issue by rebooting in to a previous kernel and ran my <a title="Linux Kernel Spring Cleaning" href="/2014/07/linux-kernel-spring-cleaning/" target="_blank">cleanKernels</a> script and fixed my system right up. To install properly, a kind fellow on Freenode.net IRC channel #ubuntu guided me in the right direction.</p>
<p>First to upgrade a desktop you will still want to have all updates installed by running the following or using the update manager:</p>

    sudo apt-get update ; sudo apt-get upgrade -y ; sudo apt-get dist-upgrade -y

<p>Reboot if needed, then run the following, (notice it's different): but be warned.... if you have WINE installed and need any of those applications, they are about to be wiped out. As this also removes WINE</p>

    sudo apt-get install --install-recommends linux-generic-lts-trusty xserver-xorg-lts-trusty libgl1-mesa-glx-lts-trusty

<p>This command will install the proper kernel HWE and graphics drivers. the xserver and mesa packages is what I missed when upgrading the first time. But you know what they say, "you live and learn." Now for some unknown reason, after updating apt-get says there is a bunch of pachages that are no longer needed it is safe to run an apt-get autoremove.... again... but be warned.... if you have WINE you will need to reinstall. No bigge though... for me any way. Just an apt-get install wine and off we go.</p>
<p>After the latest kernel HWE has been installed reboot and run the following to test:</p>

    mford@reaper:~$ hwe-support-status --verbose ; uname -srv ; lsb_release -a

<p>and your output should be the same or similar:</p>

    Your Hardware Enablement Stack (HWE) is supported until April 2017.
    Linux 3.13.0-32-generic #57~precise1-Ubuntu SMP Tue Jul 15 03:51:20 UTC 2014
    No LSB modules are available.
    Distributor ID: Ubuntu
    Description:    Ubuntu 12.04.5 LTS
    Release:        12.04
    Codename:       precise

<p>Just as before with the server, the system is now supported until April 2017 and is all set for the next 2.5 years.</p>
