---
layout: post
title: Triple Monitor Gaming with Nvidia Graphics on Linux
date: 2016-07-18 13:35:03.000000000 -04:00
author: Michael J. Ford
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Antergos
- Arch
- CLI Fu
- Gaming
- Linux
- Linux Mint
- Runs Linux
- Ubuntu
tags:
- Antergos
- CLI
- gaming
- Linux
- nvidia
- stereo
- surround
meta:
  _edit_last: '1'
  _responsive_layout: default
  _wpas_done_all: '1'
---
<p>I've been gaming exclusivity on <a href="https://antergos.com/" target="_blank">Arch Linux</a> for some time now, but one thing that I have been longing for is Triple Monitor Gaming that our M$ Windows counterparts have been enjoying for some time now. I have an Nvidia GTX 760 in my rig (yes I know out dated at this point but still is no slouch) which supports quad displays just fine on Linux - I currently have 3 displays connected; however Nvidia does not "<em>support</em>" <a href="http://www.geforce.com/hardware/technology/surround" target="_blank">Stereoscopic Surround</a> on Linux (shame on them).</p>
<p>Today I finally figured out how to make this work. I was actually trying to fix another graphics issue (games opening on wrong monitor) and came across&nbsp;<a href="https://www.youtube.com/channel/UCvEdeev3sZoxi5hMksZI4KA" target="_blank">Jakejw93</a> <a href="https://www.youtube.com/watch?v=qLTnNvlFzk0" target="_blank">youtube video</a>&nbsp;on how to enable gaming across all 3 monitors.</p>
<p><!--more--></p>
<p>It all boils down to one line in the nvidia xorg configuration file.</p>
<p>For Arch Linux, if you have not already done so, generate 20-nvidia.conf using nvidia-settings GUI - this should be saved to:
</p><pre>/etc/X11/xorg.conf.d/20-nvidia.conf</pre>
<p>Then change the following line:
</p><pre>Option         "Xinerama" "0"</pre>
<p>to
</p><pre>Option         "Xinerama" "1"</pre>
<p>Finally reboot. When you log back in to your desktop, you'll find that the desktop is <em>spanned</em>&nbsp;much like the window$ desktop would be with Surround enabled (but looks much better and is much more functional). The usual caveats apply such as making a window full screen or maximize spans it across panels. But what this also means is all games (that launch full screen) now have the proper Surround monitor resolution set, naturally play at that resolution [5760x1080], and are no different than doing the same on some other OSes.</p>
<p>To revert back, simply remove the Xinerama line entirely or change its value back to "0" and reboot (my file did not auto-magically get edited like Jakejw93's). Your displays will act normally again. So the biggest pain in the ass with this setup is you have to reboot each time - or completely restart X11, regardless all open applications will be closed. This is probably because of the way X11 is designed (it's actually rather <a href="http://itvision.altervista.org/why.linux.is.not.ready.for.the.desktop.current.html" target="_blank">fragile</a>), I'm also willing to bet this is why it is a completely undocumented feature on Nvidia's part. Maybe this type of stuff will be resolved in the next set of display managers Mir/Wayland.</p>
<p>All things aside, its nice to now play games on all 3 of my panels again.... Now I just need to setup a 4th panel so I can monitor system stats while gaming :).</p>
<p>&nbsp;</p>
<p>Thanks Jakejw93 for sharing this wonderful piece of information.</p>
