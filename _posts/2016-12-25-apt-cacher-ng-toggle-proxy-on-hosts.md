---
layout: post
title: Apt-cacher-ng Toggle Proxy on Hosts
date: 2016-12-25 21:20:19.000000000 -05:00
author: Michael J. Ford
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- CLI Fu
- Debian
- Lazy Scripts
- Linux
- Linux Mint
- Raspberry PI
- Ubuntu
tags:
- apt
- apt-get
- bash
- cache
- CLI
- Lazy Scripts
- Linux
- proxy
meta:
  _edit_last: '1'
  _responsive_layout: default
  _wpas_done_all: '1'
---
<p>Hi everyone and happy <a href="https://en.wikipedia.org/wiki/Festivus">Festivus</a>!!</p>
<p>So I was at my parents this morning for one of those non-secular holidays for gifts, and of course I bring my Linux laptop. After the festivities of gift giving, I pop open my lappy and start killing some time - and while doing that also start an apt update (patch your '<em><strong>S</strong></em>'). I then start getting a bunch of errors that apt couldn't reach any of the repos. I remember I have an <a href="https://help.ubuntu.com/community/Apt-Cacher-Server">apt-cacher-ng</a> proxy running at home and my lappy is configured to use it, so it's not reachable from where I was. I could have simply VPN'd in - but what's the fun in that!</p>
<p>On Ubuntu's page for apt-cacher there's a few scripts that will automatically enable/disable the proxies (mostly based off ping), either by running the script manually, in cron, or by events such as network. Well none of those were good enough for me; all of them required something that was not in my mobile environment or simply did not like cron/manual running. I ended up taking the ideas (and maybe a line or two) from one of the examples and wrote my own. You can find it here on my <a href="https://app.slashetc.us/gitlab/mford/general-scripts/blob/master/LazyScripts/toggle_apt-cacher.sh">gitlab</a> server....</p>
<p>Heres the initial comments:</p>
<pre>#!/bin/bash
#------------------------------------------------------------------------------
#
# Name   : toggle_apt-cache.sh
#
# Purpose: Toggles usage of proxies listed in ${aptProxyConf}
#
# Usage  : toggle_apt-cache.sh [start|on|off]
#
#     start       Same as calling script with no arguments. Run normal test
#                 and enable/disable routines
#
#     on          Toggle ALL proxies listed in ${aptProxyConf} ON
#
#     off         Toggle ALL proxies listed in ${aptProxyConf} OFF
#
#
# Installation: Make a symlink from where ever this script lives
#               to /etc/network/if-up.d, or simply put the script there:
#
#  ln -s /path/to/toggle_apt-cache.sh /etc/network/if-up.d/toggle_apt-cache.sh
#
#------------------------------------------------------------------------------
</pre>
<p>What's great about my version, is it uses the file where you have configured your apt proxies and parses out the necessary information. Which is a major plus for me - it means I don't need to update more than one file when I add/remove/change apt-proxy servers. The script also supports multiple servers listed in a single file and will dynamically enable/disable based on server availability when the script runs. I currently have it symlinked to /etc/network/if-up.d/toggle_apt-cache.sh.</p>
<p>I'm also add at some point support for turning on/off all proxies at once, or individually via <code>toggle_apt-cache.sh [on|off] host.example.com</code>. But that's an addition for another day.</p>
<p>Hope you all enjoy this...</p>
