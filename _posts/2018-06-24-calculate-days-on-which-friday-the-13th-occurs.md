---
layout: post
title: Calculate days on which Friday the 13th occurs
date: 2018-06-24 09:20:57.000000000 -04:00
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
<div><code>$ for y in $(seq 1996 2018); do echo -n "$y -&gt; "; for m in $(seq 1 12); do NDATE=$(date --date "$y-$m-13" +%w); if [ $NDATE -eq 5 ]; then PRINTME=$(date --date "$y-$m-13" +%B);echo -n "$PRINTME "; fi; done; echo; done</code></p>
<div>I removed the dependency of the English language</div>
<ul>
<li><a href="http://www.commandlinefu.com/commands/view/24099/calculate-days-on-which-friday-the-13th-occurs" rel="noopener noreferrer" target="_blank">View this command to comment, vote or add to favourites</a></li>
<li><a href="https://app.slashetc.us/commands/by/ginochen" rel="noopener noreferrer" target="_blank">View all commands by <strong>ginochen</strong></a></li>
</ul>
<p><a href="http://www.commandlinefu.com" rel="noopener noreferrer" target="_blank"><img src="{{ site.baseurl }}/assets/small-logo.jpg" alt="commandlinefu.com" /></a></p>
<p>Diff your entire server config at <a href="http://www.scriptrock.com?utm_campaign=clfu&amp;utm_medium=clfu%20api%20rss&amp;utm_source=clfu" rel="noopener noreferrer" target="_blank">ScriptRock.com</a></p>
<p><img src="{{ site.baseurl }}/assets/-F2AgWXOrTA" height="1" width="1" alt="" /></div>
<p>Read More <a href="http://feedproxy.google.com/~r/Command-line-fu/~3/-F2AgWXOrTA/calculate-days-on-which-friday-the-13th-occurs" target="_blank">here</a>.</p>
