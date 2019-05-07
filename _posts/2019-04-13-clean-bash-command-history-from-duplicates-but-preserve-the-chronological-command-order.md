---
layout: post
title: Clean BASH command history from duplicates, but preserve the chronological command order
date: 2019-04-13 13:31:44 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

`$ cp -a ~/.bash_history ~/.bash_history.bak;history|sort -k2 -k 1,1nr|uniq -f2|sort -n|tr -s ' '|cut -d' ' -f3- >~/.bash_history`





-   [View this command to comment, vote or add to favourites](http://www.commandlinefu.com/commands/view/24492/clean-bash-command-history-from-duplicates-but-preserve-the-chronological-command-order)
-   [View all commands by **knoppix5**](https://app.slashetc.us/commands/by/knoppix5)

[![commandlinefu.com](http://www.commandlinefu.com/images/small-logo.jpg)](http://www.commandlinefu.com)

Diff your entire server config at [ScriptRock.com](http://www.scriptrock.com?utm_campaign=clfu&utm_medium=clfu%20api%20rss&utm_source=clfu)

![](http://feeds.feedburner.com/~r/Command-line-fu/~4/DqhWrbp-fLM)

[Article Source](https://www.commandlinefu.com/commands/view/24492/clean-bash-command-history-from-duplicates-but-preserve-the-chronological-command-order).
{: .center-text}

