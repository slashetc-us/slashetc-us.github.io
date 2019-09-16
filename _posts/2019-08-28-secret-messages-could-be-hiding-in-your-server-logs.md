---
layout: post
title: Secret Messages Could be Hiding in Your Server Logs
date: 2019-08-28 11:00:00 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

\[Ryan Flowers\] writes in with a clever little hack that can allow you to hide data where nobody is going to go looking for it. By exploiting the fact that a web server will generally log all HTTP requests whether or not it's valid, he shows how [you can covertly send a message by asking the server for a carefully crafted fictitious URL](https://miscdotgeek.com/curlytp-every-web-server-is-a-dead-drop/).

We aren't talking about requesting "yousuck.txt" from the server that hosts your least favorite website, either. As \[Ryan\] demonstrates, you can compress a text file, encode it with `uuencode`, and then send it line by line to the destination server with `curl`. He shows how the process, which he calls "CurlyTP" can be done manually on the command line, but it would be a simple matter of wrapping it up in a Bash script.

To get the message back, you just do the opposite. Use `grep` to find the lines in the log file that contain the encoded data, and then put them through `uudecode` to get the original text back. Finding the appropriate lines in the log file is made easier by prepending a prearranged keyword to the beginning of the URL requests. The keyword can be changed for each message to make things easier to keep track of.

If you're still wondering why anyone would go through the trouble to do this, \[Ryan\] provides an excellent example: a covert "dead drop" where people could leave messages they'd rather not send through the usual channels. As long as the sender used a service to mask their true IP address, they could anonymously deliver messages onto the server without having to use any special software or protocol they might not have access to. Even the most restrictive firewalls and security measures aren't likely to be scanning URLs for compressed text files.

We've seen [web-based dead drops done with Python](https://hackaday.com/2013/08/03/dead-drop-concept-inspired-by-ender-wiggin-family/) in the past, and even [purpose built "PirateBoxes" that allow people to covertly exchange files](https://hackaday.com/2014/05/31/piratebox-for-all-your-wireless-dead-drop-needs/), but we like how this method doesn't require any special configuration on the server side. You should check your server logs, somebody might be trying to tell you something.

![](http://feeds.feedburner.com/~r/hackaday/LgoM/~4/WXU8tB01D3E)

[Article Source](https://hackaday.com/2019/08/28/secret-messages-could-be-hiding-in-your-server-logs/).
{: .center-text}

