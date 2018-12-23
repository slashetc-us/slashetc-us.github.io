---
layout: post
title: "[one-liners] Find Reverse Depends of a Package"
date: 2017-01-04 19:44:35.000000000 -05:00
author: Michael J. Ford
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Bash tips
- CLI Fu
- Debian
- Linux
- Linux Mint
- Raspberry PI
- Ubuntu
tags:
- bash
- cli fu
- debian
- Linux
- one-liners
- Ubuntu
meta:
  _edit_last: '1'
  _responsive_layout: default
  _wpas_done_all: '1'
---
<p>Sometimes you have a package installed and you want to know what other packages depend on it (such as a lib package). This one-liner only works on Debian/Ubuntu (apt/dpkg) based systems, but I'm sure a simmilar method works for rpm based distros such as CentOS.</p>
<p><code>for depends in $( apt-cache rdepends ${package} ); do dpkg -l | grep ${depends}; done</code></p>
<p>Where ${package} is the name of the package you are looking to find which other packages are dependent on it. Lets see it in action:</p>
<p><a href="https://www.slashetc.us/wp-content/uploads/2017/01/Selection_005.png"><img class="aligncenter size-full wp-image-661" src="{{ site.baseurl }}/assets/Selection_005.png" alt="" width="1425" height="243" /></a></p>
<p>OK so what happened here...</p>
<p><code>apt-cache rdepends ${package}</code> provides us with a list of packages which depend on the package - which for most cases that would be the end of it. But I like pretty things... So we place that list into a loop, for each package in the list, we run <code>dpkg -l</code> which lists all packages, pipe that into <code>grep ${depends}</code> to list (and colorize*) the results in a nice ordered list.</p>
<p>I could have done this without grep and run dpkg -l ${depends} but then we would get some unnecessary output. This method also makes it easy for scripts looking for the reverse-depends of a given package, and what the install status, versions and architectures are.</p>
<h6><em>* Note: I have a grep alias which includes --color</em></h6>
