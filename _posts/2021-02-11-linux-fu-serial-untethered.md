---
layout: post
title: Linux Fu: Serial Untethered
date: 2021-02-11 18:01:31 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

Serial ports used to be everywhere. In a way, they still are since many things that appear to plug in as a USB device actually look like a serial port. The problem is that today, the world runs on the network. Sure, you can buy a terminal server that converts a serial port to an Ethernet port, but what fun is that? In this article, I'm going to show you how to stream serial ports over the network using some available Linux tools. It isn't perfect, and it won't work for every case, but when it works it works well.

Everything is a File, Until it Isn't
------------------------------------

At some point in the past, Unix --- the progenitor of Linux --- treated virtually everything as a file, and all files were created more or less equal. Programs didn't care if a file was local, on the network, from a tape drive, or arriving over a named pipe.

But things started to change. Even though a serial port is just a file under Linux, it has some special attributes that let you set, for example, baud rates. Worse, some programs "know" too much about files and insist on certain naming conventions. So, in theory, you should be able to create a network socket, connect one end to a serial port and the other end to a program, and be done with it. In theory.

[![](https://hackaday.com/wp-content/uploads/2021/01/de9.png?w=250){sizes="(max-width: 250px) 100vw, 250px" srcset="https://hackaday.com/wp-content/uploads/2021/01/de9.png 499w, https://hackaday.com/wp-content/uploads/2021/01/de9.png?resize=250,129 250w, https://hackaday.com/wp-content/uploads/2021/01/de9.png?resize=400,206 400w"}](https://hackaday.com/wp-content/uploads/2021/01/de9.png)The practice is different, of course. That might work in some very simple cases. You can use a utility called `socat` (like cat for a socket) to set it up. However, if the program you are trying to fake tries to set a baud rate, for example, it is probably going to throw up its hands. Some programs won't even recognize your fake serial port.

About socat
-----------

The `socat` program is like a magic adapter cable that simply pipes everything from one place to another and also handles the reverse traffic. The program can read from files, pipes, devices, sockets, and a few other items. For example, you might want to build a simple TCP proxy to forward connections on the local host to another host. That's easy with `socat`:

    socat TCP4-LISTEN:88 TCP4:10.1.1.125:8000

That will only handle one connection, but you can even ask `socat` to fork off a new connection to handle multiple requests.

The Problem
-----------

If you try doing this with a serial port, though, it is unlikely to work without a lot of setup. This is such a common desire that there are several programs available to help. Most Linux distributions will support `ser2net`, a program made to handle the task of converting a serial port to a socket. If `ser2net` doesn't suit you, there's also some Python code that ships as an example with [the `pyserial` library](https://github.com/pyserial/pyserial/blob/master/examples/rfc2217_server.py), but your distribution version of `pyserial` maybe too old to support it. If that's the case, you'll need to install it outside of your package manager using `pip`, for example. I'm going to assume you are using `ser2net`.

The `ser2net` program normally installs as a system service and you can  edit `/etc/ser2net.conf` to configure it. I took out all the default lines and added the two serial ports I wanted to expose along with the baud rates I wanted:

    7777:telnet:0:/dev/ttyUSB0:115200 8DATABITS NONE 1STOPBIT remctl7778:telnet:0:/dev/ttyUSB1:115200 8DATABITS NONE 1STOPBIT remctl

Then you'll need to restart the service (probably `systemctl restart ser2net`; if it fails to restart the first time, try again as sometimes it tries to start before the old copy is totally shutdown).

For debugging, you might want to stop the service and run with some debugging messages:

`ser2net -d -C "7778:telnet:0:/dev/ttyUSB1:115200 8DATABITS NONE 1STOPBIT remctl"`

Either way, once the service is running, the ports you specify (7777 and 7778 in my configuration file) will act like the associated serial ports.

A few notes. If you are doing this with a 3D printer and you have something like Octoprint running, you'll need to shut it down or otherwise make it let go of the printer port before you set all this up. This would be true of any program that would hold the serial port you are interested in opening. For some standard ports, you need to make sure login is not holding the port open and waiting for a user. Exactly how all that works will depend on your setup. The same goes for your firewall. If you are listening on port 7777 and you use a firewall, you'll need to set it up to allow connections on that port. Don't forget you usually have to be root to open up low-numbered ports, too.

The Client Side
---------------

On the client side, you can use `socat`, but some programs will figure out it isn't a real serial port. You can still, try though:

    socat pty,link=$HOME/dev/ttyNET0,waitslave tcp:10.1.1.125:7777

Some programs are only going to look for devices in `/dev`, though. If that's the case for you, you are going to need to run `socat` as root (use `sudo`) and then also set `/dev/ttyNET0` so that you have read and write permissions (e.g., `sudo chmod 666 /dev/ttyNET0`).

Testing 1, 2, 3...
------------------

Instead of trying your program right away, though, I suggest you test using something like `picocom`. For example, on the client machine after you run `socat`, you should be able to run something like:

    picocom -b 115200 $HOME/dev/ttyNET0

![Picocom can talk across the network using a virtual serial device](https://hackaday.com/wp-content/uploads/2021/01/pico.png?w=250){sizes="(max-width: 250px) 100vw, 250px" srcset="https://hackaday.com/wp-content/uploads/2021/01/pico.png 750w, https://hackaday.com/wp-content/uploads/2021/01/pico.png?resize=250,184 250w, https://hackaday.com/wp-content/uploads/2021/01/pico.png?resize=400,294 400w"}

Obviously, you'll need to use the right name and baud rate. You should be able to talk to the remote device. If not, figure out why before you run the regular program. If your serial device isn't text-based, you might have more luck with [cutecom](https://gitlab.com/cutecom/cutecom/) but, alas, the program knows your fake serial port is fake and won't let you select it. On the other hand, the program is open source, so you could easily create a local version that used any device name you like. There are other choices, of course. For example, `gtkterm` will let you pick up the fake serial port.

If you run into a protocol error, you may find that the serial port has moved (e.g., it was `/dev/ttyUSB0` and now it is `/dev/ttyUSB1`) or you have an invalid baud rate. The error message isn't very helpful, but don't forget to use the -d option in `ser2net` while troubleshooting.

![The `ttynvt` program doesn't have great documentation graphics, but they are accurate.](https://hackaday.com/wp-content/uploads/2021/01/2021-01-31-19.41.27-gitlab.com-5bdaca0b7ca2.png?w=250){sizes="(max-width: 250px) 100vw, 250px" srcset="https://hackaday.com/wp-content/uploads/2021/01/2021-01-31-19.41.27-gitlab.com-5bdaca0b7ca2.png 596w, https://hackaday.com/wp-content/uploads/2021/01/2021-01-31-19.41.27-gitlab.com-5bdaca0b7ca2.png?resize=250,150 250w, https://hackaday.com/wp-content/uploads/2021/01/2021-01-31-19.41.27-gitlab.com-5bdaca0b7ca2.png?resize=400,240 400w"}

When you do run the regular program, it might work. However, it also might throw an error or, in some cases, refuse to let you select the file because it isn't really a serial port. In some cases, you are simply out of luck. However, a smarter client that understands the serial port might help, too. The `ttynvt` program is one example of that. You will probably have to build it from source, but it is very easy to do that. it does depend on `libfuse`, but otherwise doesn't need anything exotic.

Once you have it built, try something like this:

    sudo src/ttynvt -M 199 -m 6 -n ttyNET0 -S 10.1.1.125:7777

This will create the `/dev/ttyNET0` device (so you need to be root). On my system, the new device had read and write permissions set for root and the dialout group. You might need to check and fix the permissions, though, depending on your setup.

Again, test with `picocom`, and then try your target program. Cross your fingers!

Why, Oh Why?
------------

My original goal was to run Lightburn software for my laser cutter on a big machine using a remote desktop. I wanted the laser cutter plugged into the USB port on the local machine and have the software talk to a fake port on the bigger computer.

Alas, as of today, Lighburn is too smart for my naughty tricks and refuses to show my virtual serial ports. There's no way that I know of to force it to use a file name of my choice, so I can't even try to see if it would work. However, I was able to test the setup with some other G code software and it does work. I've mentioned this to Lightburn, so maybe it will be fixed by the time you read this.

The paradigm that "everything is a file" is very powerful. Unfortunately, every year it gets less true and that causes more hoops to jump through when you want to do something interesting like this. Still, in true Linux fashion, there's always a way to get there. I have no doubt that I could trace the calls Lighburn is making to open the port and find a way to fake them for the serial client. I'm hoping, though, that I don't have to.

![](http://feeds.feedburner.com/~r/hackaday/LgoM/~4/CjR4OaHBUdI)

[Article Source](https://hackaday.com/2021/02/11/linux-fu-serial-untethered/).
{: .center-text}

