---
layout: post
title: Writing Secure Shell Scripts
date: 2019-02-05 12:30:00 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---







![security](https://www.linuxjournal.com/sites/default/files/nodeimage/story/bigstock--209746780_0.jpg)





by [Dave Taylor](https://www.linuxjournal.com/users/dave-taylor "View user profile.")





*Don't expose your system with sloppy scripts!*

Although a Linux desktop or server is less susceptible to viruses and malwarethan a typical Windows device, there isn't a device on the internet thatisn't eventually attacked. The culprit might be the stereotypical nerd ina bedroom testing his or her hacker chops (think Matthew Broderick in *WarGames* or Angelina Jolie in *Hackers*). Then again, it might be anorganized military, criminal, terrorist or other funded entity creatingmassive botnets or stealing millions of credit cards via a dozen redirectedattack vectors.

In any case, modern systems face threats that were unimaginable in the earlydays of UNIX development and even in the first few years of Linux as a hobbyistreimplementation of UNIX. Ah, back in the day, the great worry was aboutcopyrighted code, and so useful tools constantly were being re-implemented fromscratch to get away from the AT&T Bell Labs licenses and so forth.

I have personal experience with this too. I rewrote the *Hunt theWumpus* game`wumpus` from scratch for BSD 4.2 when the Berkeley crowd was trying to getaway from AT&T UNIX legal hassles. I know, that's not the greatest claim to fame,but I also managed to cobble together a few other utilities in my time too.

Evolution worked backward with the internet, however. In real life, thelawless Wild West was gradually tamed, and law-abiding citizens replaced theoutlaws and thugs of the 1850s and the Gold Rush. Online, it seems that thereare more, smarter and better organized digital outlaws than ever.

Which is why one of the most important steps in learning how to write shellscripts is to learn how to ensure that your scripts are secure—even ifit's just your own home computer and an old PC you've converted intoa Linux-based media server with Plex or similar.

Let's have a look at some of the basics.

### Know the Utilities You Invoke

Here's a classic trojan horse attack: an attacker drops a script called`ls`into /tmp, and it simply checks to see the userid that invoked it, then handsoff its entire argument sequence to the real /bin/ls. If it recognizes userid= root, it makes a copy of /bin/sh into /tmp with an innocuous name, thenchanges its permission to setuid root.

This is super easy to write. Here's a version off the top of my head:

    #!/bin/shif [ "$USER" = "root" ] ; then  /bin/cp /bin/sh /tmp/.secretshell  /bin/chown root /tmp/.secretshell  /bin/chmod 4666 root /tmp/.secretshellfiexec /bin/ls $*

I hope you understand what just happened. This simple little script hascreated a shell that always grants its user root access to the Linux system.Yikes. Fancier versions would remove themselves once the root shell has beencreated, leaving no trace of how this transpired.





[Go to Full Article](https://www.linuxjournal.com/content/writing-secure-shell-scripts)







![](http://feeds.feedburner.com/~r/LinuxJournalHowtos/~4/6bCiZsFhvwg)

[Article Source](https://www.linuxjournal.com/content/writing-secure-shell-scripts).
{: .center-text}

