---
layout: post
title: How ASLR protects Linux systems from buffer overflow attacks
date: 2019-01-08 11:44:00 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

<div class="section">

Address Space Layout Randomization (ASLR) is a memory-protection process for operating systems that guards against buffer-overflow attacks. It helps to ensure that the memory addresses associated with running processes on systems are not predictable, thus flaws or vulnerabilities associated with these processes will be more difficult to exploit.

ASLR is used today on Linux, Windows, and MacOS systems. It was first implemented on Linux in 2005. In 2007, the technique was deployed on Microsoft Windows and MacOS. While ASLR provides the same function on each of these operating systems, it is implemented differently on each one.

The effectiveness of ASLR is dependent on the entirety of the address space layout remaining unknown to the attacker. In addition, only executables that are compiled as Position Independent Executable (PIE) programs will be able to claim the maximum protection from ASLR technique because all sections of the code will be loaded at random locations. PIE machine code will execute properly regardless of its absolute address.

[To read this article in full, please click here](https://app.slashetc.us/article/3331199/linux/what-does-aslr-do-for-linux.html#jump)

Article Source [here](https://www.networkworld.com/article/3331199/linux/what-does-aslr-do-for-linux.html#tk.rss_security).
{: .center-text}

