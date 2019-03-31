---
layout: post
title: Programmatic way to find and set your timezone
date: 2019-03-09 21:52:32 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

`$ sudo timedatectl set-timezone $(curl worldtimeapi.org/api/ip/$(curl ifconfig.io/ip)|cut -d" -f16)`



1\. it find your public ip via ifconfig.io2. than use this IP to request your timezone via worldtimeapi.org3. and send it to the command timedatectl set-timezone



-   [View this command to comment, vote or add to favourites](http://www.commandlinefu.com/commands/view/24463/programmatic-way-to-find-and-set-your-timezone)
-   [View all commands by **jodumont**](https://app.slashetc.us/commands/by/jodumont)

[![commandlinefu.com](http://www.commandlinefu.com/images/small-logo.jpg)](http://www.commandlinefu.com)

Diff your entire server config at [ScriptRock.com](http://www.scriptrock.com?utm_campaign=clfu&utm_medium=clfu%20api%20rss&utm_source=clfu)

![](http://feeds.feedburner.com/~r/Command-line-fu/~4/3JDL3PoATDU)

[Article Source](https://www.commandlinefu.com/commands/view/24463/programmatic-way-to-find-and-set-your-timezone).
{: .center-text}

