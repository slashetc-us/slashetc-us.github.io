---
layout: post
title: unset all http proxy related environment variables in one go in the current shell
date: 2019-06-28 10:40:41 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

`$ eval "unset $(printenv | grep -ioP '(?:https?|no)_proxy' | tr 'n' ' ')"`





-   [View this command to comment, vote or add to favourites](http://www.commandlinefu.com/commands/view/24563/unset-all-http-proxy-related-environment-variables-in-one-go-in-the-current-shell)
-   [View all commands by **acavagni**](https://app.slashetc.us/commands/by/acavagni)

[![commandlinefu.com](http://www.commandlinefu.com/images/small-logo.jpg)](http://www.commandlinefu.com)

Diff your entire server config at [ScriptRock.com](http://www.scriptrock.com?utm_campaign=clfu&utm_medium=clfu%20api%20rss&utm_source=clfu)

![](http://feeds.feedburner.com/~r/Command-line-fu/~4/yhd-1y8W21Q)

[Article Source](https://www.commandlinefu.com/commands/view/24563/unset-all-http-proxy-related-environment-variables-in-one-go-in-the-current-shell).
{: .center-text}

