---
layout: post
title: "Tales From the Sysadmin: Impending Hard Drive Doom"
date: 2020-07-14 14:00:10 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

It should have been another fine day, but not all was well in paradise. Few things bring a creeping feeling of doom like a computer that hardlocks and then refuses to boot. The clicking sound coming from the tower probably isn't a good sign either. Those backups are up to date, right? Right?

There are some legends and old stories about hard drive repair. One of my favorites is the official solution to stiction for old drives: Smack it with a mallet. Another trick I've heard repeatedly is to freeze a hard drive before trying to read data off of it. This could actually be useful in a couple instances. The temperature change can help with stiction, and freezing the drive could potentially help an overheating drive last a bit longer. The downside is the potential for condensation inside the drive. Don't turn to one of these questionable fixes unless you've exhausted the safer options.

For the purpose of this article, we'll assume the problem is the hard drive, and not another component like a power supply or SATA cable causing problems. [A truly dead drive](https://hackaday.com/2019/12/24/hard-drive-data-recovery-why-not-diy/) is a topic for another time, but if the drive is alive enough to show up as a block device when plugged in, then there's hope for recovering the data. One of the USB to SATA cables available on your favorite online store is a great way to recover data. Another option is booting off a Linux DVD or flash drive, and accessing the drive in place. If you're lucky, you can just copy your files and call it a day. If the file transfer fails because of the dying drive, or you need a full disk image, it's time to pull out some tools and get to work.

As a hard drive degrades, individual sectors can become unreadable. This is an expected process, and modern drives are built with spare sectors to fend off the inevitable. As sectors begin to become unreliable, they are retired, and spare sectors are used instead. When the spare sectors are gone, the disk begins accumulating unreadable sectors. An unreadable sector in the middle of a file will kill a file transfer, or maybe even make the device unmountable. The ironic part is that it's usually only a tiny percentage of the disk that's unreadable. If only there was a way to manage those unreadable sectors.

Turning to DDRescue
-------------------

The amateur sysadmin has a potent tool in his toolkit: `ddrescue`. It's a descendant of sorts of the venerable `dd` disk copy tool, but with an important difference. When `dd` encounters a read error, it stops the transfer and displays the error. `ddrescue` makes a note of the error, leaves a blank spot in the output file, and continues transferring what data it can. Because there is record of the missing chunks, we can keep trying to read the missing parts, and maybe recover more data.

To get ddrescue running, we give it an input, an output, and a mapfile.\
`ddrescue /dev/sda diskimage.img mapfile.log`\
By default, ddrescue goes through three phases of rescue. First, it copies a sector at a time until it hits an error. For a drive that's working perfectly, this operation completes without issue and the whole drive is copied. If a sector can't be copied, or is even particularly slow in responding, `ddrescue` jumps ahead, hopefully beyond the problem.

The second phase is trimming. To put it simply, `ddrescue` starts at the end of each skipped section, and works backwards till it hits a bad sector. The purpose is to recover the largest amount of data as quickly as possible, and to establish exactly which sectors are the problematic ones. The last phase is scraping, where each unread sector is examined individually, attempting to read the data contained. Each time a sector is read, the mapfile is modified to keep track.

A sector might fail to read 15 times in a row, and on the 16th attempt, finally read successfully. Because of this, `ddrescue` supports making multiple scraping passes in alternating directions. Part of the theory is that the read head alignment might be slightly different when approaching the sector from a different location, and that difference might be enough to finally get a successful read.

When It's Not So Simple
-----------------------

While the ideal operation of ddrescue is straightforward enough, there are some potential problems to be aware of. The first is heat. The process of trying to recover data from an already dying drive can quickly overheat it, and make further reads impossible. The best and simplest solution is a fan blowing cool air over the drive. The other common problem I've encountered is a bit harder to explain, but it's identified by a specific error message: `ddrescue: Input file disappeared: No such file or directory.` When trying to read from the drive, something went wrong badly enough that the drive has disappeared from the system. My theory in this case is that the firmware on the drive itself has crashed and halted. Regardless, unpowering and repowering the drive is usually enough to get back to work.

![This could have worked better.](https://hackaday.com/wp-content/uploads/2020/07/IMG_20200712_154408-e1594586887750.jpg?w=400)

This means that for a particularly stubborn drive, the process of recovering bits feels a lot like babysitting. Power cycle the drive once it crashes, and restart `ddrescue` --- over and over and over again. Since the read fails as a result of the crash, that sector is marked as bad, and the rescue attempt jumps past it. Sectors in good shape might not trigger the crash, so some data gets read.

If you think that spending hours power cycling a hard drive doesn't sound like a fun task, and is something that should be automated, then you're right. It's easy enough to wrap our `ddrescue` command in a loop, ideally along with five seconds of sleep. That handles half the problem, but power cycling the drive isn't a software problem. I've used Adafruit's power switch tail in the past, connected to a Raspberry Pi GPIO pin, to kill the drive's power supply every 30 seconds. It's not ideal, but it works. Unfortunately that device is discontinued, and I'm not aware of a direct replacement.

The last time I ran into this problem, I used a WiFi power switch, pictured above. Whenever the device disappeared, the script triggered the plug to power cycle the drive. This worked, and on a 500 GB drive, I recovered all but the last 1.5 megs. The only downside is that the smart plug only works via the cloud, so every power cycle required a request sent to the IFTTT cloud. Leaving the drive running overnight resulted in too many requests, and my account was frozen. Next time, Ill have to use a device that supports one of the open source firmwares, like [Tasmota](https://github.com/arendst/Tasmota). Regardless, the script is simple:

    while true; do    sudo ddrescue /dev/sda diskimage.img mapfile.log    if [ -a /dev/sdc ]; then        sudo ddrescue /dev/sda diskimage.img mapfile.log -M    else        curl -X POST https://maker.ifttt.com/trigger/switch_off/with/key/REDACTED        sleep 10        curl -X POST https://maker.ifttt.com/trigger/switch_on/with/key/REDACTED        sleep 10    fidone

If the device disappears, use the switch to power cycle the drive. If ddrescue completes, and the device is still present, then use the `-M` switch to mark all the bad sectors as untried.

In many cases, this isn't a process that ever really finishes, but the rate of recovery eventually drops too low to be worth continuing. Once you've copied as much of the raw data off the drive as possible, it's a good idea to use `fsck`/`chkdsk` to repair the now-rescued filesystem. If it's a system drive, after you burn it to a new disk, you'll want to use your OS's tools to verify the system files. For Windows, I've had good success with `SFC` and `DISM`. On Linux, use your system's package manager to verify your installed packages. On a Fedora/Red Hat system, `rpm -Va` will show any installed binaries that have unexpected contents.

Over the years I've rescued a handful of drives with `ddrescue`, that other techniques just wouldn't touch. It's true that a good backup is the ideal solution, but if you find yourself in a situation where you really need to get data off a dying drive, `ddrescue` might just be your saving grace. Good luck!

Banner Image: "[Shiny](https://www.flickr.com/photos/35248676@N08/6527665133)" by Nick Perla, BY-ND

![](http://feeds.feedburner.com/~r/hackaday/LgoM/~4/-GckDZlVQUU)

[Article Source](https://hackaday.com/2020/07/14/tales-from-the-sysadmin-impending-hard-drive-doom/).
{: .center-text}

