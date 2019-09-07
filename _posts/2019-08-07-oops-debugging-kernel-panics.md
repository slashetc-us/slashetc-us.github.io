---
layout: post
title: Oops! Debugging Kernel Panics
date: 2019-08-07 23:30:00 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---







![debugging kernel panics](https://www.linuxjournal.com/sites/default/files/nodeimage/story/bigstock--173363303_0.jpg)





by [Petros Koutoupis](https://www.linuxjournal.com/users/petros-koutoupis "View user profile.")





*A look into what causes kernel panics and some utilities to help gainmore information.*

Working in a Linux environment, how often have you seen a kernel panic?When it happens, your system is left in a crippled state untilyou reboot it completely. And, even after you get your system back into afunctional state, you\'re still left with the question: why? You may have noidea what happened or why it happened. Those questions can be answeredthough,and the following guide will help you root out the cause of some of the conditionsthat led to the original crash.

![](https://www.linuxjournal.com/sites/default/files/styles/max_650x650/public/u%5Buid%5D/panic.png)

*Figure 1. A Typical Kernel Panic*

Let\'s start by looking at a set of utilities known as`kexec` and `kdump`. `kexec` allows you to boot intoanother kernel from an existing (and running) kernel, and`kdump` is a`kexec`-based crash-dumping mechanism for Linux.

### Installing the Required Packages

First and foremost, your kernel should have the following components statically built in to its image:

    CONFIG_RELOCATABLE=yCONFIG_KEXEC=yCONFIG_CRASH_DUMP=yCONFIG_DEBUG_INFO=yCONFIG_MAGIC_SYSRQ=yCONFIG_PROC_VMCORE=y

You can find this in `` /boot/config-`uname -r` ``.

Make sure that your operating system is up to date with the latest-and-greatest package versions:

    $ sudo apt update && sudo apt upgrade

Install the following packages(I\'m currently using Debian, but thesame should and will apply to Ubuntu):

    $ sudo apt install gcc make binutils linux-headers-`uname -r` ↪kdump-tools crash `uname -r`-dbg

*Note: Package names may varyacross distributions.*

During the installation, you will be prompted with questions to enable`kexec` to handle reboots (answer whatever you\'d like, but I answered\"no\"; see Figure 2).

![](https://www.linuxjournal.com/sites/default/files/styles/max_650x650/public/u%5Buid%5D/kexec.png)

*Figure 2.kexec Configuration Menu*

And to enable `kdump` to run and load at system boot, answer\"yes\" (Figure 3).

![](https://www.linuxjournal.com/sites/default/files/styles/max_650x650/public/u%5Buid%5D/kdump.png)

*Figure 3.kdump Configuration Menu*

### Configuring kdump

Open the /etc/default/kdump-tools file, and at the very top,you should see the following:





[Go to Full Article](https://www.linuxjournal.com/content/oops-debugging-kernel-panics-0)







![](http://feeds.feedburner.com/~r/LinuxJournalHowtos/~4/QQQfCblW_es)

[Article Source](https://www.linuxjournal.com/content/oops-debugging-kernel-panics-0).
{: .center-text}

