---
layout: post
title: Check if your desired password is already available in haveibeenpwnd database.
date: 2018-08-13 07:41:55.000000000 -04:00
author: TT-RSS Published Article
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- TT-RSS Published Articles
tags: []
meta:
  _wpas_done_all: '1'
---
<div><code>$ function hibp() { sha1=$(echo -n "$1"|sha1sum|awk '{print toupper($0)}'|tr -d 'n'); curl -s -H $'Referer: https://haveibeenpwned.com/' https://api.pwnedpasswords.com/range/$(echo -n $sha1|cut -c1-5)|grep -i $(echo -n $sha1|cut -c6-40); }</code></p>
<div></div>
<ul>
<li><a href="http://www.commandlinefu.com/commands/view/24193/check-if-your-desired-password-is-already-available-in-haveibeenpwnd-database.-this-command-uses-the-api-provided-by-hibp" rel="noopener noreferrer" target="_blank">View this command to comment, vote or add to favourites</a></li>
<li><a href="https://app.slashetc.us/commands/by/zmonkey" rel="noopener noreferrer" target="_blank">View all commands by <strong>zmonkey</strong></a></li>
</ul>
<p><a href="http://www.commandlinefu.com" rel="noopener noreferrer" target="_blank"><img src="{{ site.baseurl }}/assets/small-logo.jpg" alt="commandlinefu.com" /></a></p>
<p>Diff your entire server config at <a href="http://www.scriptrock.com?utm_campaign=clfu&amp;utm_medium=clfu%20api%20rss&amp;utm_source=clfu" rel="noopener noreferrer" target="_blank">ScriptRock.com</a></p>
<p><img src="{{ site.baseurl }}/assets/ZpUISAVvvIc" alt="" /></div>
<p>Read More <a href="https://www.commandlinefu.com/commands/view/24193/check-if-your-desired-password-is-already-available-in-haveibeenpwnd-database.-this-command-uses-the-api-provided-by-hibp" target="_blank">here</a>.</p>
