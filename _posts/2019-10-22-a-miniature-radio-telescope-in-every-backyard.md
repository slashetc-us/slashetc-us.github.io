---
layout: post
title: A Miniature Radio Telescope in Every Backyard
date: 2019-10-22 20:01:47 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

You probably wouldn't expect to see somebody making astronomical observations during a cloudy day in the center of a dense urban area, but that's exactly what was happening at the recent 2019 Philadelphia Mini Maker Faire. Professor James Aguirre of the University of Pennsylvania was there demonstrating the particularly compact Mini Radio Telescope (MRT) project built around an old DirecTV satellite dish and a smattering of low-cost components, giving visitors a view of the sky in a way most had never seen before.

[![](https://hackaday.com/wp-content/uploads/2019/10/mrt_closeup.jpg?w=307){sizes="(max-width: 307px) 100vw, 307px" srcset="https://hackaday.com/wp-content/uploads/2019/10/mrt_closeup.jpg 2000w, https://hackaday.com/wp-content/uploads/2019/10/mrt_closeup.jpg?resize=192,250 192w, https://hackaday.com/wp-content/uploads/2019/10/mrt_closeup.jpg?resize=307,400 307w, https://hackaday.com/wp-content/uploads/2019/10/mrt_closeup.jpg?resize=479,625 479w"}](https://hackaday.com/wp-content/uploads/2019/10/mrt_closeup.jpg)Thanks to [the project's extensive online documentation](https://github.com/UPennEoR/MiniRadioTelescope), anyone with a spare satellite dish and a couple hundred dollars in support hardware can build their very own personal radio telescope that's capable of observing objects in the sky no matter what the time of day or weather conditions are. Even if you're not interested in peering into deep space from the comfort of your own home, the MRT offers a framework for building an automatic pan-and-tilt directional antenna platform that could be used for picking up signals from orbiting satellites.

With the [slow collapse of satellite television in the United States](https://hackaday.com/2018/12/17/welcome-to-the-slow-death-of-satellite-tv-in-america/) these dishes are often free for the taking, and a fairly common sight on the sidewalk come garbage day. Perhaps there's even one (or three) sitting on your own roof as you read this, waiting for a new lease on life in the Netflix Era.

Whether it's to satisfy your own curiosity or because you want to follow in Professor Aguirre's footsteps and use it as a tool for STEM outreach, projects like MRT make it easier than ever to build a functional DIY radio telescope.

Point and Shoot
---------------

The MRT, and really any radio telescope project like this, is essentially made up of two separate systems: one that provides the motorized aiming of the dish, and the receiver that actually captures the signals. Either system could work independently of the other, but when combined with the appropriate software "glue", they allow the user to map the sky in radio frequencies.

Obviously, the electronics and mechanical components required to pan an antenna across the sky aren't terribly complex. If you wanted to keep things really simple and were content with moving in a single axis, [you could even do it with a "barn door" tracker](https://hackaday.com/2019/01/12/diy-guided-telescope-mount-tracks-like-a-barn-door/). What's really kicked off the recent explosion of DIY radio telescopes is the RTL-SDR project and the era of low-cost Software Defined Radios (SDRs) it's inspired.

[![](https://hackaday.com/wp-content/uploads/2019/10/mrt_electronics.jpg?w=400){sizes="(max-width: 400px) 100vw, 400px" srcset="https://hackaday.com/wp-content/uploads/2019/10/mrt_electronics.jpg 1200w, https://hackaday.com/wp-content/uploads/2019/10/mrt_electronics.jpg?resize=250,177 250w, https://hackaday.com/wp-content/uploads/2019/10/mrt_electronics.jpg?resize=400,283 400w, https://hackaday.com/wp-content/uploads/2019/10/mrt_electronics.jpg?resize=800,566 800w"}](https://hackaday.com/wp-content/uploads/2019/10/mrt_electronics.jpg)Unsurprisingly, the MRT also uses an RTL-SDR receiver for processing signals from the Low-Noise Block (LNB) in the dish. Professor Aguirre says that since they are still using the stock DirecTV LNB, the telescope is fairly limited in what it can actually "see". But it's good enough to image the sun or [pick up satellites in orbit](https://hackaday.com/2017/03/23/see-satellites-with-a-simple-radio-telescope/), which is sufficient for the purposes of demonstrating the basic operating principles of a radio telescope.

To move the satellite dish, the MRT is using an Arduino connected to a trio of Big Easy Drivers from Sparkfun. These are in turn connected to the stepper motors in the antenna mount, which are sufficiently geared so they can move the dish around without the need for a counterweight. This makes it an excellent candidate for enclosure inside a dome, which would allow for all-weather observations.

Both the RTL-SDR receiver and the Arduino are connected to a Raspberry Pi, which runs the software for the telescope and provides the interface for the user. The MRT GitHub repository contains all of the various tools and programs created for the project, mostly written in Python, which should provide a useful reference even if you're not interested in duplicating the telescope's overall design.

Wandering Through the Sky
-------------------------

When we visited Professor Aguirre, he was attempting to use the MRT to find the Sun. You'd think that a simple enough task in the middle of the afternoon, but thanks to an unbroken layer of steel-gray clouds hanging low in the October sky, Sol was absolutely nowhere to be found with our meager human senses.

![Geostationary satellites as seen by the MRT](https://hackaday.com/wp-content/uploads/2019/10/mrt_sats.png?w=355){sizes="(max-width: 320px) 100vw, 320px" srcset="https://hackaday.com/wp-content/uploads/2019/10/mrt_sats.png 365w, https://hackaday.com/wp-content/uploads/2019/10/mrt_sats.png?resize=222,250 222w"}

As the dish made its slow robotic pans across the sky, we spoke with the Professor about the telescope and the various revisions it went through over the years. Eventually the display lit up, showing a representation of an unusually strong signal, clearly the MRT was hearing *something* out there. After brief scrutiny, the Professor announced that we hadn't found the sun; instead, the telescope most likely crossed paths with a geostationary satellite.

It was this raconteur style of discovery that kept visitors to the Mini Radio Telescope enthralled. Nobody expected this hacked together contraption of consumer-grade hardware to [discover a new exoplanet](https://hackaday.com/2019/09/28/chinese-radio-telescope-hopes-to-find-exoplanets-fast/) or help solve some long-pondered mystery of the cosmos while sitting in a Philadelphia parking lot.

But it was more than capable of pointing out objects tens of thousands of kilometers away while our own eyes couldn't even figure out where the Sun was. It reaffirmed in a very real way that something was out there, and students both young and old couldn't help but be fascinated by it.

![](http://feeds.feedburner.com/~r/hackaday/LgoM/~4/0nJB_YUIrHI)

[Article Source](https://hackaday.com/2019/10/22/a-miniature-radio-telescope-in-every-backyard/).
{: .center-text}

