---
layout: post
title: Download mp3 files linked in a RSS podcast feed
date: 2021-08-09 13:40:26 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

`$ curl http://radiofrance-podcast.net/podcast09/rss_14726.xml | grep -Eo "(http|https)://[a-zA-Z0-9./?=_%:-]*mp3" | sort -u | xargs wget`



Directly download all mp3 files of the desired podcast



-   [View this command to comment, vote or add to favourites](http://www.commandlinefu.com/commands/view/25469/download-mp3-files-linked-in-a-rss-podcast-feed)
-   [View all commands by **pascalv**](https://app.slashetc.us/commands/by/pascalv)

[![commandlinefu.com](http://www.commandlinefu.com/images/small-logo.jpg)](http://www.commandlinefu.com)

Diff your entire server config at [ScriptRock.com](http://www.scriptrock.com?utm_campaign=clfu&utm_medium=clfu%20api%20rss&utm_source=clfu)

![](http://feeds.feedburner.com/~r/Command-line-fu/~4/LKuGSdFn7hA)

[Article Source](https://www.commandlinefu.com/commands/view/25469/download-mp3-files-linked-in-a-rss-podcast-feed).
{: .center-text}

