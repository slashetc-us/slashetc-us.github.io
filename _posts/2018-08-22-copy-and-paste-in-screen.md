---
layout: post
title: Copy and Paste in Screen
date: 2018-08-22 13:08:08.000000000 -04:00
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
<div>
<div>
<div>
<div>  <img src="{{ site.baseurl }}/assets/bigstock--224038156.png" alt="" /></div>
<div>by <a title="View user profile." href="https://www.linuxjournal.com/users/kyle-rankin" rel="noopener noreferrer" target="_blank">Kyle Rankin</a></div>
<div>
<p><em>Put the mouse down, and copy and paste inside a terminal with your<br />
keyboard using Screen.</em></p>
<p>
Screen is a command-line tool that lets you set up multiple terminal windows<br />
within it, detach them and reattach them later, all without any graphical<br />
interface. This program has existed since before I started using Linux, but<br />
first I clearly need to address the<br />
fact that I'm even using Screen at all prior to writing a tech tip about it. I can<br />
already hear you ask, "Why not tmux?"<br />
Well, because every time someone tries to convince me to make the switch, it's<br />
usually for one of the following reasons:</p>
<ul>
<li>
Screen isn't getting updates:<br />
I've been happy with the current Screen feature set for more than a decade, so as<br />
long as distributions continue to package it, I don't feel like I need any<br />
newer version.</li>
<li>
tmux key bindings are so much simpler:<br />
I climbed the Screen learning curve more than a decade ago, so to me, the Screen key<br />
bindings are second nature.</li>
<li>
But you can do vertical and horizontal splits in tmux:<br />
you can do them in Screen too, and since I climbed that learning curve ages<br />
ago,<br />
navigating splits are part of my muscle memory just like inside vim.
</li>
</ul>
<p>
So now that those arguments are out of the way, I thought those of you<br />
still using Screen might find it useful to learn how to do copy and paste<br />
within Screen itself. Although it's true that you typically can use your mouse<br />
to highlight text and paste it, if you are a fan of staying on the home row<br />
like I am, you realize how much faster and more efficient it is if you can<br />
copy and paste from within Screen itself using the keyboard. In fact, I found<br />
that once I learned this method, I ended up using it multiple times every day.</p>
<h3>
Enter Copy Mode</h3>
<p>
The first step is to enter copy mode from within Screen. Press Ctrl-a-[ to<br />
enter copy mode. Once you're in this mode, you can use arrow keys or<br />
vi-style keybindings to navigate up and down your terminal window. This is<br />
handy if you are viewing a log or other data that has scrolled off the screen<br />
and you want to see it. Typically people who are familiar with copy mode<br />
just use it for scrolling and then press q to exit that mode, but once you<br />
are in copy mode, you also can move the cursor to an area you want to copy.</p>
<h3>
Copy Text</h3>
<p>
To copy text once in copy mode, move your cursor to where you want to start<br />
to copy and then press the space bar. This will start the text selection, and<br />
you'll see the cursor change so that it highlights the text as you then<br />
move the cursor to select everything you want to copy. Once you are done<br />
selecting text, press the space bar again, and it will be copied to Screen's<br />
copy buffer. Once text is copied to Screen's clipboard, it automatically<br />
will exit copy mode.</p>
</div>
<div>  <a href="https://www.linuxjournal.com/content/copy-and-paste-screen" rel="noopener noreferrer" target="_blank">Go to Full Article</a>
</div>
</p></div>
</p></div>
</div>
<p>Read More <a href="https://www.linuxjournal.com/content/copy-and-paste-screen" target="_blank">here</a>.</p>
