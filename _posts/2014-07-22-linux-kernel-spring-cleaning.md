---
layout: post
title: Linux Kernel Spring Cleaning
date: 2014-07-22 21:40:58.000000000 -04:00
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
---
<p>So I've gone ahead and finished up my nice little 7 line script to clean out old kernel versions on your Linux desktop/laptop/workstation/server what have you. What this script does is finds and lists in a temp file all installed kernel versions (excluding the newest kernel) then for each line in the temp file uninstalls the old kernels. Finally finishing up by updating grub of the changes.</p>
<p>This script works great for me, but your mileage may vary. So the standard disclaimers apply: this script is run at your own risk. I have no responsibility for loss of data, unbootable systems, kittens on your keyboard, etc.</p>
<p><!--more--></p>
<p>That being said. This script was written and tested on Ubuntu 12.04.4 LTS current running kernel 3.2.0-67-generic (latest at the time of this writing) both Desktop and Server flavours. This script requires root privileges to do its magic. So I like to place mine in /sbin/ to require using 'sudo' to even start the script, and also it automagicly places script in my $PATH.</p>

    #!/bin/bash
    # File path: /sbin/cleanKernels
    # Version 1.0 STABLE
    # About:
    # This bash script will clean out the ubuntu grub boot menu
    # of old kernels, and remove them from the workstation/server
    # to run type 'sudo bash /path/to/cleanKernels' in a terminal
    ls /boot/ | grep vmlinuz | sed 's@vmlinuz-@linux-image-@g' | grep -v `uname -r` /tmp/kernelList
    for I in `cat /tmp/kernelList`
        do
        aptitude remove $I
    done
    rm -f /tmp/kernelList
    update-grub

<p>As said this script runs fantastically on Ubuntu 12.04 LTS, and will more than likely run on all previous and future releases as well. Great little piece of code if you are like me and (try to) keep systems running as long as possible between reinstalls and run SSDs. After awhile my /boot/ directory gets a bit on the full side. Best to run this after the latest kernel has been running and thoroughly tested. Because this script assumes the latest installed kernel is the one to keep. Again, this is a simple script, but any input to make better (or more cross compatibly with other linuxes) is always welcome.</p>
