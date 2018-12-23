---
layout: post
title: Getting PFSense Loaded to Watchguard Firebox x-CORE-e UTM (Part 2 - LCD)
date: 2014-05-24 10:33:23.000000000 -04:00
author: Michael J. Ford
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- FreeBSD
- Linux
- Networking
- PFSense
tags: []
meta:
  _edit_last: '1'
  _responsive_layout: default
---
<p><strong>But That's Not All!!!</strong></p>
<p>Hello all and welcome to part 2 of getting PFSense on the WatchGuard Firebox x-core UTM. In <a title="Getting PFSense Loaded to Watchguard Firebox x-CORE-e UTM (Part 1)" href="https://www.slashetc.us/2014/05/getting-pfsense-loaded-to-watchguard-firebox-x-core-e-utm-part-1/" target="_blank">Part 1</a> we hacked our way into the bios of the Firebox changed the bios settings and booted our 4GB card with PFSense. We could stop there and be the end of it. As it sits the Firebox will now bend to our will with the awesome new firmware installed and doing nothing else done to the box. But why stop there, there's no fun in that. Instead there are a few other hacks to this box I would like to share to truly make it yours.</p>
<p><!--more--></p>
<p><strong>Lets Get That LCD a Bit more Useful:</strong></p>
<p>Stock after installing PFSense, the LCD after boot will display "PFSense B7... Booting OS....." that's kinda boring and way useful to me. But lucky for us, the forms come to the rescue again. In this section we will get that LCD to display some running stats live from PFSense (original forum post <a title="Firebox LCD Driver for LCDProc" href="https://forum.pfsense.org/index.php?topic=7920.msg344513#msg344513" target="_blank">here</a>). Using the following method to lasso in the LCD should not only work, but survive a firmware upgrade.</p>
<p>1. First thing we want to do is install a few packages on our Firewall. In PFSense navigate to <em>System -&gt; Packages</em>. From there click on the "Available Packages" tab. Then search and install "<em>LCDproc-dev</em>" and "<em>Shellcmd</em>". We need the <em>dev</em> version of LCDproc as it includes working drivers for the watchguard LCD. <em>Shellcmd</em> will allow us to start the LCDproc service once installed and configured.</p>
<p>2. Once installed, navigate to S<em>ervices -&gt; LCDproc</em>. Select/change the following: 'Enable LCDproc at startup' yes | Com port - Parallel Port 1 | Display Size - 2x20 | Driver - Watchguard Firebox with SDEC.</p>
<p>3, When settings are correct, hit save at the bottem. This will create the lcdd.conf file needed; but its in a temporary location. So we need to move&nbsp; it a bit more permanent. Open<em> Diagnostics -&gt; Command Prompt</em> and run:
</p><pre style="margin: 0; padding: 0;"><code class="bbc_code">cp /usr/local/etc/LCDd.conf /conf</code></pre>
<p>Note form forum: this can be done from the terminal. But the command tab will take care of the mount and remounting of the file system for you. (and I can be a bit lazy, but what geek isn't)</p>
<p>4. From here, go back into&nbsp;S<em>ervices -&gt; LCDproc</em> and uncheck start at boot. Also change the <em>Com port</em> to <em>none. </em>Setting com to none is a must. the Startup RC script expects this.</p>
<p>5. Open up Shellcmd from <em>Services -&gt; Shellcmd</em> and add the following command to start LCDproc server and client.
</p><pre style="margin: 0; padding: 0;"><code class="bbc_code">/usr/bin/nice -20 /usr/local/sbin/LCDd -r 0 -c /conf/LCDd.conf &gt; /dev/null &amp;</code></pre>
<p>and
</p><pre style="margin: 0; padding: 0;"><code class="bbc_code">/usr/bin/nice -20 /usr/local/bin/lcdproc C L I T U &amp;</code></pre>
<p>Make sure both types are set to 'shellcmd'. You can set screens you want to display. (the origonal post only used C T U, but I added a bit more. You can see the full list <a title="LCDproc manpage ubuntu" href="http://manpages.ubuntu.com/manpages/intrepid/man1/lcdproc.1.html" target="_blank">here</a> and <a title="LCDproc docs" href="http://lcdproc.omnipotent.net/docs.php3" target="_blank">here</a>).</p>
<p>6. Reboot or run the above commands in the terminal and see your LCD come to life!!!</p>
<p>--- UPDATE 20140717:&nbsp; This box is now live in my network. Upgraded Firmware from 2.1.3 to 2.1.4 and all changes (so far) survive the Firmware Upgrade. However the custom 'shellcmd' commands that run at boot do not run after required reboot. Simply reboot your Firebox or run manually to avoid rebooting again. ---</p>
<p>And that's all, for the LCD. Stay tuned for part 3, where we tackle cooling tweaks.</p>
<p>Part 1: <a title="Part 1" href="https://www.slashetc.us/2014/05/getting-pfsense-loaded-to-watchguard-firebox-x-core-e-utm-part-1/" target="_blank">https://www.slashetc.us/2014/05/getting-pfsense-loaded-to-watchguard-firebox-x-core-e-utm-part-1/</a></p>
<hr>
<p>&nbsp;</p>
<p>Credits:<br>
- PFsense: <a title="https://pfsense.org/" href="https://pfsense.org/" target="_blank">https://pfsense.org/</a><br>
- Install: <a title="https://doc.pfsense.org/index.php/PfSense_on_Watchguard_Firebox#X-Core-e" href="https://doc.pfsense.org/index.php/PfSense_on_Watchguard_Firebox#X-Core-e" target="_blank">https://doc.pfsense.org/index.php/PfSense_on_Watchguard_Firebox#X-Core-e</a><br>
- LCD: <a title="https://forum.pfsense.org/index.php/topic,7920.msg344513.html#msg344513" href="https://forum.pfsense.org/index.php/topic,7920.msg344513.html#msg344513" target="_blank">https://forum.pfsense.org/index.php/topic,7920.msg344513.html#msg344513</a><br>
- x550e thread: <a title="https://forum.pfsense.org/index.php?topic=20095.0" href="https://forum.pfsense.org/index.php?topic=20095.0" target="_blank">https://forum.pfsense.org/index.php?topic=20095.0</a><br>
- BIOS: <a title="https://sites.google.com/site/pfsensefirebox/home" href="https://sites.google.com/site/pfsensefirebox/home" target="_blank">https://sites.google.com/site/pfsensefirebox/home</a><br>
- BIOS CMD: <a title="http://forums.pcper.com/showthread.php?290476-AWDFLASH-command-line-switches" href="http://forums.pcper.com/showthread.php?290476-AWDFLASH-command-line-switches" target="_blank">http://forums.pcper.com/showthread.php?290476-AWDFLASH-command-line-switches</a><br>
- Minicom: <a title="https://alioth.debian.org/projects/minicom" href="https://alioth.debian.org/projects/minicom" target="_blank">https://alioth.debian.org/projects/minicom</a></p>
