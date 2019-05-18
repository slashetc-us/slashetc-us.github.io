---
layout: post
title: Get Your IP Geographic Location with curl and jq
date: 2019-04-26 09:15:00 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

`$ curl -s https://ipvigilante.com/$(curl -s https://ipinfo.io/ip) | jq '.data.latitude, .data.longitude, .data.city_name, .data.country_name'`





-   [View this command to comment, vote or add to favourites](http://www.commandlinefu.com/commands/view/24514/get-your-ip-geographic-location-with-curl-and-jq)
-   [View all commands by **emphazer**](https://app.slashetc.us/commands/by/emphazer)

[![commandlinefu.com](http://www.commandlinefu.com/images/small-logo.jpg)](http://www.commandlinefu.com)

Diff your entire server config at [ScriptRock.com](http://www.scriptrock.com?utm_campaign=clfu&utm_medium=clfu%20api%20rss&utm_source=clfu)

![](http://feeds.feedburner.com/~r/Command-line-fu/~4/IOVHdhovPP8)

[Article Source](https://www.commandlinefu.com/commands/view/24514/get-your-ip-geographic-location-with-curl-and-jq).
{: .center-text}

