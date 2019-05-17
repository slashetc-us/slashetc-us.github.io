---
layout: post
title: Raspberry Pi Becomes The Encrypted Password Keeper You Need
date: 2019-04-26 05:00:55 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

Unless you’re one of the cool people who uses the same password everywhere, you might be in need of a hardware device that keeps your usernames and passwords handy. The Passkeeper is a hardware password storage system built on a Raspberry Pi. [It encrypts your passwords](https://passkeeper.jdevelop.com/), and only through the magic of a special key fob will you ever get your passwords out of this device.

The hardware for this device is built around the Raspberry Pi Zero. You might be questioning the use of a Pi Zero, but given that it’s an entire Linux system for just a few bucks, it only makes sense. The rest of the hardware is a tiny OLED SPI display, an RFID card reader, a few LEDs, some wire, and some solder. A 3D printed case keeps everything together.

Of course, this build is all about the software, and for that, the Passkeeper device is built in Go, with a system that builds a web interface, builds the firmware, and writes everything to an SD card. Usage is simply plugging the Passkeeper into the USB port of your computer where it presents itself as a network interface. Everything is available by pinging an IP address, and after that the web UI will log your usernames and passwords. All this data is encrypted, and can only be unlocked if an RFID key fob is present. It’s an interesting idea and certinaly inexpensive. It’s not quite as polished [as something like the Mooltipass](https://www.tindie.com/products/stephanelec/mooltipass-mini-offline-password-keeper/), but if you have a Pi around and don’t have a password keeper, this is something to build this weekend.

![](http://feeds.feedburner.com/~r/hackaday/LgoM/~4/5-7gx0SX4Q8)

[Article Source](https://hackaday.com/2019/04/25/raspberry-pi-becomes-the-encrypted-password-keeper-you-need/).
{: .center-text}

