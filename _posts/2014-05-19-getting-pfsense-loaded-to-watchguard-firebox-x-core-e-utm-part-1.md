---
layout: post
title: Getting PFSense Loaded to Watchguard Firebox x-CORE-e UTM (Part 1)
date: 2014-05-19 22:13:25.000000000 -04:00
author: Michael J. Ford
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- FreeBSD
- Networking
- PFSense
tags: []
meta:
  _edit_last: '1'
  _responsive_layout: default
---
<p><strong>Basic Overview:</strong></p>
<p>The Installation of PFSense is fairly straight forward. Really, all that needs to be done is using a 512MB CF Card (as of v2.1.1 is the min size) we can flash PFSense 512MB embedded image to the card and we're all set. Slap that CF into the Watchguard and it will boot into PFSense. Connect your Serial cable and fire up your favorate terminal emulator&nbsp; program (mine is <a title="minicom" href="https://en.wikipedia.org/wiki/Minicom" target="_blank">minicom</a>) with settings 9600 8N1 and you can watch it boot. But lets not stop there. 512MB is quite small. With my test install I was left with about 32MB free on each slice, not too much breathing room. So I purchaced a 4GB card from newegg. Unfourntatly the Watchguard will not boot on any drive large than 512MB by Default, but that's where some clever hackery comes into play. Here we will modify the BIOS to allow access from the console and change settings to allow boot from larger CF cards. Plus some other goodies.</p>
<p>Ready... Set... Let's Hack something!!!</p>
<p><!--more--></p>
<p><em>Disclaimer</em>: I have only tested this on a Watchguard x1250e, but should also work for the x550e and x750e boxes. Also, this entire demo was done using Ubuntu 12.04 LTS as my OS Host.</p>
<p><em>Note</em>: Let it be said. I take no responsibility if you brick your Firebox, it bursts into flames, implodes, explodes, or just flat out disappears.</p>
<p><strong>Needed items:</strong><br>
Watchguard x-e series | Null Modem Cables (yes people still use these) | Serial Port on PC and Termial Emulator software<br>
64MB-256MB CF Card for flashing BIOS | 4GB CF Card for PFSense | of course other misc network cables (But you knew that right)</p>
<p><strong>Software:</strong><br>
PFSense: <a href="https://pfsense.org/download/download.php?url=http%3A%2F%2Ffiles.nyi.pfsense.org%2Fmirror%2Fdownloads%2FpfSense-2.1.3-RELEASE-4g-i386-nanobsd.img.gz" target="_blank">4GB Image</a> | <a href="https://files.pfsense.org/hashes/pfSense-2.1.3-RELEASE-4g-i386-nanobsd.img.gz.md5" target="_blank">MD5_Checksum</a> | <a href="https://files.pfsense.org/hashes/pfSense-2.1.3-RELEASE-4g-i386-nanobsd.img.gz.sha256" target="_blank">SHA256_Checksum</a><br>
BIOS Image: <a href="https://sites.google.com/site/pfsensefirebox/home/FreeDOSBios2.img.gz" target="_blank">BiosFlash</a>&nbsp; MD5: 5ebb3f11925a8a78f7829e3ca0823f5d</p>
<p><strong>Getting down to business:</strong><br>
Loading up the Firebox with PFSense is fairly straight forward, if you know were to look anyway. I used several sources to get mine up and running. All the info is there on the internet but putting it all together was a task all in itself. So I would like to create a nice tutorial on how i got mine all nice and running smooth.</p>
<p>- First, we need to get into the BIOS. Stock and unmodified, the Firebox will boot into PFSense without issue onto a 512MB CF card, but that doesn't leave you with much room to spare. And any larger size card just refuses to boot. What we have to do is change some settings in the BIOS to "trick" it into booting my 4GB card. BUT..... BIOS access is not easy to come by. The Firebox is a blind box by default; i.e. you cannot see what is happening as the device boots and there is no (easy) VGA/USB to use for access. There is 2 USB ports on the board, but are unpopulated, and needs either headers installed or actual port installed, also there is a VGA pinout on board, but (not only did I not find it) is uses a non standard pitch and is a weird pinout. More info can be found on the forums about access that way. What I ended up doing is flashing a modified BIOS [ in the link above ] by the awesome <a href="https://forum.pfsense.org/index.php?action=profile;u=23621" target="_blank">stephenw10</a> on the PFSense <a href="https://forum.pfsense.org/index.php" target="_blank">Forums</a>.</p>
<p>- Flashing the BIOS<br>
[note: found out this can be done via PFSense as well, if only 512MB card available. see <a href="https://forum.pfsense.org/index.php?topic=20095.msg263376#msg263376" target="_blank">here</a>. but haven't tested. Why fix what's not broke.]</p>
<p>To install the modified BIOS we need a CF card between 64MB and 256MB and flash it with a special FreeDOS image, also created by stephenw10, and boot in the firebox. I flashed mine using the following line in Ubuntu: [ your mileage may vary ]</p>

    zcat /user/Downloads/FreeDOSBios2.img.gz | dd of=/dev/sdb bs=16k

<p>Once the image has been flashed to the CF card, pop that in your Firebox and connect a PC to the serial port. Fire up a terminal emulator [ minicom FTW ] and using settings 9600 8N1. Then power up the Firebox.</p>
<p>Once FreeDOS has booted, you should hear 3 beeps and the C:&gt; prompt in the terminal (So nostalgic). Change to the bios directory and run the biosid program.</p>

    cd bios
    biosid

<p>You should see the following output regardless of the date on the BIOS chip:</p>

    BIOS DATE : 12/21/2005
    CHIPSET ID : Alviso
    BIOS ID : 6A79GAKAC-00
    BIOS TYPE : Phoenix Technologies, Ltd.
    OEM INFO : **** BIOS Ver.ETAC0017 (2005/12/21&gt; **** &nbsp; &nbsp;

<p>The output may be formatted different but ALL the info should be the same, if yours does not match stop NOW. you WILL brick your box.</p>
<p>But if it matches feel free to continue. Create a backup of the BIOS. One is already in the image, but doesn't hurt.</p>

    awdflash backup1.bin /pn /sy /e

<p>The flash the modified bin: <strong>[DO NOT REBOOT FIREBOX WHILE FLASHING!!! WILL RESULT IN BRICK!!!]</strong></p>

    awdflash x750eb6.bin /py /sn /cc /e

<p>When the program finishes it will clear the cmos and bump back to the DOS prompt. At this point you may now reset. If the command seems to be hung, something is probably set incorrectly. Let sit for about 10min or so and reset.</p>
<p>- Accessing the Bios [ I've got some nifty photos <a title="Firebox x-core-e bios" href="http://www.slashetc.us/firebox-x-core-e-bios/" target="_blank">here</a>. ]</p>
<p>To access the BIOS you need to change the terminal settings to 115200 8N1. This should allow access to the BIOS. DEL is not valid char to send over serial so we need to hit TAB to enter BIOS. It may not work the first time ( I had to boot twice ) as the Firebox freaks out the first time because of the cmos being cleared. For first boot to BIOS, power on and let system run about 3-5 min, then try to access again. If still cannot then something is seriously wrong.</p>
<p>If done correctly, you may now sit and enjoy for a moment as you have just shoved your way to where the OEM does not want you to go. :)</p>
<p>All there is to do now, change the Harddisk settings to 'manual' and set heads to '2'. Yes it will report your 4GB card as 512MB, but PFSense does not care, it will boot any way.</p>
<p>- Flash PFSense to CF Card | This is well documented: see <a href="http://https://doc.pfsense.org/index.php/HOWTO_Install_pfSense#Embedded_.28Compact_Flash.29_Installation" target="_blank">here</a>.</p>
<p>- Booting.</p>
<p>Hard part done. To Boot PFSense, stick that newly flashed CF Card into the Firebox. Connect Serial and back to 9600 8N1. The Power on the box. may take a moment or 2 to post. Then you should see PFSense start to boot up. At this point is just like any other PFSense install.</p>
<p>Now we could stop here, but there is more to explore on this box. For starters, the LCD will continue to display "Booting OS...." which bothers me. Also the ARM/DISARM led is red when should be green. Both these issues will be fixed in Part 2. And maybe a few other things (still playing with the box).</p>
<p>Stay tuned....</p>
<p>Part 2: <a title="Part 2" href="https://www.slashetc.us/2014/05/getting-pfsense-loaded-to-watchguard-firebox-x-core-e-utm-part-2-lcd/" target="_blank">https://www.slashetc.us/2014/05/getting-pfsense-loaded-to-watchguard-firebox-x-core-e-utm-part-2-lcd/</a></p>
<hr>
<p>Credits:<br>
- PFsense: <a title="https://pfsense.org/" href="https://pfsense.org/" target="_blank">https://pfsense.org/</a><br>
- Install: <a title="https://doc.pfsense.org/index.php/PfSense_on_Watchguard_Firebox#X-Core-e" href="https://doc.pfsense.org/index.php/PfSense_on_Watchguard_Firebox#X-Core-e" target="_blank">https://doc.pfsense.org/index.php/PfSense_on_Watchguard_Firebox#X-Core-e</a><br>
- LCD: <a title="https://forum.pfsense.org/index.php/topic,7920.msg344513.html#msg344513" href="https://forum.pfsense.org/index.php/topic,7920.msg344513.html#msg344513" target="_blank">https://forum.pfsense.org/index.php/topic,7920.msg344513.html#msg344513</a><br>
- x550e thread: <a title="https://forum.pfsense.org/index.php?topic=20095.0" href="https://forum.pfsense.org/index.php?topic=20095.0" target="_blank">https://forum.pfsense.org/index.php?topic=20095.0</a><br>
- BIOS: <a title="https://sites.google.com/site/pfsensefirebox/home" href="https://sites.google.com/site/pfsensefirebox/home" target="_blank">https://sites.google.com/site/pfsensefirebox/home</a><br>
- BIOS CMD: <a title="http://forums.pcper.com/showthread.php?290476-AWDFLASH-command-line-switches" href="http://forums.pcper.com/showthread.php?290476-AWDFLASH-command-line-switches" target="_blank">http://forums.pcper.com/showthread.php?290476-AWDFLASH-command-line-switches</a><br>
- Minicom: <a title="https://alioth.debian.org/projects/minicom" href="https://alioth.debian.org/projects/minicom" target="_blank">https://alioth.debian.org/projects/minicom</a></p>
