---
layout: post
title: Capture all plaintext passwords
date: 2019-02-03 05:13:58 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

`$ sudo tcpdump port http or port ftp or port smtp or port imap or port pop3 or port telnet -l -A | egrep -i -B5 'pass=|pwd=|log=|login=|user=|username=|pw=|passw=|passwd=|password=|pass:|user:|username:|password:|login:|pass |user '`





-   [View this command to comment, vote or add to favourites](http://www.commandlinefu.com/commands/view/24407/capture-all-plaintext-passwords)
-   [View all commands by **wuziduzi**](https://app.slashetc.us/commands/by/wuziduzi)

[![commandlinefu.com](http://www.commandlinefu.com/images/small-logo.jpg)](http://www.commandlinefu.com)

Diff your entire server config at [ScriptRock.com](http://www.scriptrock.com?utm_campaign=clfu&utm_medium=clfu%20api%20rss&utm_source=clfu)

![](http://feeds.feedburner.com/~r/Command-line-fu/~4/jPh4mrCMUjo)

[Article Source](https://www.commandlinefu.com/commands/view/24407/capture-all-plaintext-passwords).
{: .center-text}

