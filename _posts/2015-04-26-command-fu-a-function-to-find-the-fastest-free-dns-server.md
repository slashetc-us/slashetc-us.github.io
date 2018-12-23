---
layout: post
title: 'Command fu: a function to find the fastest free DNS server'
date: 2015-04-26 20:59:42.000000000 -04:00
author: Michael J. Ford
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- CLI Fu
- Linux
tags:
- CLI
- command line
- King fu
- Linux
meta:
  _wpas_done_all: '1'
---
<p>timeDNS() { parallel -j0 --tag dig @{} "$*" ::: 208.67.222.222 208.67.220.220 198.153.192.1 198.153.194.1 156.154.70.1 156.154.71.1 8.8.8.8 8.8.4.4 | grep Query | sort -nk5; }</p>
<p><a href="http://www.commandlinefu.com/commands/view/14188/a-function-to-find-the-fastest-free-dns-server">http://www.commandlinefu.com/commands/view/14188/a-function-to-find-the-fastest-free-dns-server</a></p>
