---
layout: post
title: Spy on your smart home with this open source research tool
date: 2019-04-13 10:16:30 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

Researchers at [Princeton University <span></span>](https://crunchbase.com/organization/princeton-university) have built a web app that lets you (and them) spy on your smart home devices to see what they’re up to.

The open source tool, called IoT Inspector, is available for download [here](https://iot-inspector.princeton.edu). (Currently it’s Mac OS only, with a wait list for Windows or Linux.)

In a [blog](https://iot-inspector.princeton.edu/blog/) about the effort the researchers write that their aim is to offer a simple tool for consumers to analyze the network traffic of their Internet connected gizmos. The basic idea is to help people see whether devices such as smart speakers or wi-fi enabled robot vacuum cleaners are sharing their data with third parties. (Or indeed how much snitching their gadgets are doing.)

Testing the IoT Inspector tool in their lab the researchers say they found a Chromecast device constantly contacting Google’s servers even when not in active use.

![](https://techcrunch.com/wp-content/uploads/2019/04/chromecast-activities.png?w=680){sizes="(max-width: 680px) 100vw, 680px" srcset="https://techcrunch.com/wp-content/uploads/2019/04/chromecast-activities.png 1349w, https://techcrunch.com/wp-content/uploads/2019/04/chromecast-activities.png?resize=150,119 150w, https://techcrunch.com/wp-content/uploads/2019/04/chromecast-activities.png?resize=300,237 300w, https://techcrunch.com/wp-content/uploads/2019/04/chromecast-activities.png?resize=768,607 768w, https://techcrunch.com/wp-content/uploads/2019/04/chromecast-activities.png?resize=680,538 680w, https://techcrunch.com/wp-content/uploads/2019/04/chromecast-activities.png?resize=50,40 50w"}

A Geeni smart bulb was also found to be constantly communicating with the cloud — sending/receiving traffic via a URL (tuyaus.com) that’s operated by a China-based company with a platform which controls IoT devices.

There are other ways to track devices like this — such as setting up a wireless hotspot to sniff IoT traffic using a packet analyzer like WireShark. But the level of technical expertise required makes them difficult for plenty of consumers.

Whereas the researchers say their web app doesn’t require any special hardware or complicated set-up so it sounds easier than trying to go packet sniffing your devices yourself. ([Gizmodo](https://gizmodo.com/this-simple-tool-will-reveal-the-secret-life-of-your-sm-1832264323), which got an early look at the tool, describes it as “incredibly easy to install and use”.)

One wrinkle: The web app doesn’t work with Safari; requiring either Firefox or Google Chrome (or a Chromium-based browser) to work.

The main caveat is that the team at Princeton do want to use the gathered data to feed IoT research — so users of the tool will be contributing to efforts to study smart home devices.

The title of their research project is *Identifying Privacy, Security, and Performance Risks of Consumer IoT Devices.* The listed principle investigators are professor Nick Feamster and postdoctoral researcher Danny Yuxing Huang at the university’s Computer Science department.

The Princeton team says it intends to study privacy and security risks and network performance risks of IoT devices. But they also note they may share the full dataset with other non-Princeton researchers after a standard research ethics approval process. So users of IoT Inspector will be participating in at least one research project. (Though the tool also lets you delete any collected data — per device or per account.)

“With IoT Inspector, we are the first in the research community to produce an open-source, anonymized dataset of actual IoT network traffic, where the identity of each device is labelled,” the researchers write. “We hope to invite any academic researchers to collaborate with us — e.g., to analyze the data or to improve the data collection — and advance our knowledge on IoT security, privacy, and other related fields (e.g., network performance).”

They have produced an extensive [FAQ](https://iot-inspector.princeton.edu/blog/post/faq/) which anyone thinking about running the tool should definitely read before getting involved with a piece of software that’s explicitly designed to spy on your network traffic. (tl;dr, they’re using ARP-spoofing to intercept traffic data — a technique they warn may slow your network, in addition to the risk of their software being buggy.)

The dataset that’s being harvesting by the traffic analyzer tool is anonymized and the researchers specify they’re not gathering any public-facing IP addresses or locations. But there are still some privacy risks — such as if you have smart home devices you’ve named using your real name. So, again, do read the FAQ carefully if you want to participate.

For each IoT device on a network the tool collects multiple data-points and sends them back to servers at Princeton University — including DNS requests and responses; destination IP addresses and ports; hashed MAC addresses; aggregated traffic statistics; TLS client handshakes; and device manufacturers.

The tool has been designed not to track computers, tablets and smartphones by default, given the study focus on smart home gizmos. Users can also manually exclude individual smart devices from being tracked if they’re able to power them down during set up or by specifying their MAC address.

Up to 50 smart devices can be tracked on the network where IoT Inspector is running. Anyone with more than 50 devices is asked to contact the researchers to ask for an increase to that limit.

The project team has produced a video showing how to install the app on Mac:



[![](http://feeds.feedburner.com/~ff/Techcrunch?d=2mJPEYqXBVI)](http://feeds.feedburner.com/~ff/Techcrunch?a=4oVmPv199HM:6T2YCpwUMAY:2mJPEYqXBVI) [![](http://feeds.feedburner.com/~ff/Techcrunch?d=7Q72WNTAKBA)](http://feeds.feedburner.com/~ff/Techcrunch?a=4oVmPv199HM:6T2YCpwUMAY:7Q72WNTAKBA) [![](http://feeds.feedburner.com/~ff/Techcrunch?d=yIl2AUoC8zA)](http://feeds.feedburner.com/~ff/Techcrunch?a=4oVmPv199HM:6T2YCpwUMAY:yIl2AUoC8zA) [![](http://feeds.feedburner.com/~ff/Techcrunch?i=4oVmPv199HM:6T2YCpwUMAY:-BTjWOF_DHI)](http://feeds.feedburner.com/~ff/Techcrunch?a=4oVmPv199HM:6T2YCpwUMAY:-BTjWOF_DHI) [![](http://feeds.feedburner.com/~ff/Techcrunch?i=4oVmPv199HM:6T2YCpwUMAY:D7DqB2pKExk)](http://feeds.feedburner.com/~ff/Techcrunch?a=4oVmPv199HM:6T2YCpwUMAY:D7DqB2pKExk) [![](http://feeds.feedburner.com/~ff/Techcrunch?d=qj6IDK7rITs)](http://feeds.feedburner.com/~ff/Techcrunch?a=4oVmPv199HM:6T2YCpwUMAY:qj6IDK7rITs)



![](http://feeds.feedburner.com/~r/Techcrunch/~4/4oVmPv199HM)

[Article Source](https://techcrunch.com/2019/04/13/spy-on-your-smart-home-with-this-open-source-research-tool/).
{: .center-text}

