---
layout: post
title: Is Your Computer Stable?
date: 2016-02-13 06:58:39.000000000 -05:00
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
<p>Over the last twenty years, I've probably built around a hundred computers. It's not very difficult, and in fact, it's gotten a whole lot easier over the years as computers become more highly integrated. Consider what it would take to build something very modern like <a href="http://blog.codinghorror.com/the-scooter-computer/" rel="noopener noreferrer" target="_blank">the Scooter Computer</a>:</p>
<ol>
<li>Apply a dab of thermal compound to top of case.</li>
<li>Place motherboard in case.</li>
<li>Screw motherboard into case.</li>
<li>Insert SSD stick.</li>
<li>Insert RAM stick.</li>
<li>Screw case closed.</li>
<li>Plug in external power.</li>
<li>Boot.</li>
</ol>
<p>Bam done.</p>
<p>It's stupid easy. My six year old son and I have built Lego kits that were way more complex than this. Even a traditional desktop build is only a few more steps: insert CPU, install heatsink, route cables. And a server build is merely a few additional steps on top of that, maybe with some 1U or 2U space constraints. Scooter, desktop, or server, if you've built one computer, you've basically built them all.</p>
<p>Everyone breathes a sigh of relief when their newly built computer boots up for the first time, no matter how many times they've done it before. But booting is only the beginning of the story. Yeah, it boots, great. Color me unimpressed. What we really need to know is whether <strong>that computer is stable</strong>.</p>
<p>Although commodity computer parts <a href="https://www.pugetsystems.com/labs/articles/Most-Reliable-Hardware-of-2015-749/" rel="noopener noreferrer" target="_blank">are more reliable every year</a>, and vendors test their parts plenty before they ship them, there's no guarantee all those parts will work reliably <em>together</em>, in your particular environment, under your particular workload. And there's always the possibility, however slim, of getting very, very unlucky with subtly broken components.</p>
<p>Because we're rational scientists, we test stuff in our native environment, and collect data to <strong>prove our computer is stable</strong>.  Right? So after we boot, we test.</p>
<h5>Memory</h5>
<p>I like to start with memory tests, since those require bootable media and work the same on all x86 computers, even before you have an operating system. <a href="https://en.wikipedia.org/wiki/Memtest86" rel="noopener noreferrer" target="_blank">Memtest86</a> is the granddaddy of all memory testers. I'm not totally clear what caused the split between that and Memtest86+, but all of them work similarly. The one from passmark seems to be most up to date, so <a href="http://www.memtest86.com/download.htm" rel="noopener noreferrer" target="_blank">that's what I recommend</a>.</p>
<p>Download the version of your choice, write it to a bootable USB drive, plug it into your newly built computer, boot and let it work its magic. It's all automatic. Just boot it up and watch it go.</p>
<p><img src="{{ site.baseurl }}/assets/memtest86.png" alt="" /></p>
<p>(If your computer supports UEFI boot you'll get <a href="http://www.memtest86.com/support/ver_history.htm" rel="noopener noreferrer" target="_blank">the newest version 6.x</a>, otherwise you'll see version 4.2 as above.)</p>
<p>I recommend <strong>one complete pass of memtest86</strong> at minimum, but if you want to be extra careful, let it run overnight. Also, if you have a lot of memory, memtest can take a while! For our servers with 128GB it took about three hours, and I expect that time scales linearly with the amount of memory.</p>
<p>The "Pass" percentage at the top should get to 100% and the "Pass" count in the table should be greater than one. If you get any errors at all, anything whatsoever other than a clean 100% pass, <em>your computer is not stable</em>. Time to start removing RAM sticks and figure out which one is bad.</p>
<h5>OS</h5>
<p>All subsequent tests will require an operating system, and one basic iron clad test of stability for any computer is <strong>whether it can install an operating system</strong>. Pick your free OS of choice, and begin a default install. I recommend <a href="http://www.ubuntu.com/download/server" rel="noopener noreferrer" target="_blank">Ubuntu Server LTS x64</a> since it assumes less about your video hardware. Download the ISO and write it to a bootable USB drive. Then boot it.</p>
<p><img src="{{ site.baseurl }}/assets/ubuntu-server-install.png" alt="" /></p>
<p>(Hey look it has a memory test option! How convenient!)</p>
<ul>
<li>Be sure you have network connected for the install with DHCP; it makes the install go faster when you don't have to wait for network detection to time out and nag you about the network stuff.</li>
<li>In general, you'll be pressing <kbd>enter</kbd> a whole lot to accept all the defaults and proceed onward. I know, I know, we're installing Linux, but believe it or not, they've gotten the install bit down by now.</li>
<li>About all you should be prompted for is the username and password of the default account. I recommend <code>jeff</code> and <code>password</code>, because I am one of the world's preeminent computer security experts.</li>
<li>If you are installing from USB and get nagged about a missing CD, remove and reinsert the USB drive. No, I don't know why either, but <a href="http://askubuntu.com/questions/593002/fail-to-install-ubuntu-server-14-04-64bit-lts-from-usb-drive" rel="noopener noreferrer" target="_blank">it works</a>.</li>
</ul>
<p>If <em>anything</em> weird happens during your Ubuntu Server install that prevents it from finalizing the install and booting into Ubuntu Server &hellip; <em>your computer is not stable</em>. I know it doesn't sound like much, but this is a decent holistic test as it exercises the whole system in very repeatable ways.</p>
<p>We'll need an OS installed for the next tests, anyway. I'm assuming you've installed Ubuntu, but any Linux distribution should work similarly.</p>
<h5>CPU</h5>
<p>Next up, let's make sure the brains of the operation are in order: the CPU. To be honest, if you've gotten this far, past the RAM and OS test, the odds of you having a completely broken CPU are fairly low. But we need to be <em>sure</em>, and the best way to do that is to call upon our old friend, Marin Mersenne.</p>
<p><img src="{{ site.baseurl }}/assets/marin-mersenne.jpg" alt="" /></p>
<blockquote>
<p>In mathematics, a Mersenne prime is a prime number that is one less than a power of two. That is, it is a prime number that can be written in the form M<sub><i>n</i></sub> = 2<sup><i>n</i></sup> &minus; 1 for some integer <i>n</i>. They are named after Marin Mersenne, a French Minim friar, who studied them in the early 17th century. The first four Mersenne primes are 3, 7, 31, and 127.</p>
</blockquote>
<p>I've been using <a href="http://www.mersenne.org/download/" rel="noopener noreferrer" target="_blank">Prime95 and MPrime</a> &ndash; tools that attempt to rip through as many giant numbers as fast as possible to determine if they are prime &ndash; for the last 15 years. Here's how to download and install <code>mprime</code> on that fresh new Ubuntu Server system you just booted up.</p>
<pre><code>mkdir mprime
cd mprime
wget ftp://mersenne.org/gimps/p95v289.linux64.tar.gz
tar xzvf p95v289.linux64.tar.gz
rm p95v289.linux64.tar.gz
</code></pre>
<p>(You may need to replace the version number in the above command with the current latest from the <a href="http://www.mersenne.org/download/" rel="noopener noreferrer" target="_blank">mersenne.org download page</a>, but as of this writing, that's the latest. Also, if you prefer an older version without the very heat intensive AVX and AVX2 instructions added in 2011 and 2014 respectively, get <code>mprime266-linux64.tar.gz</code>)</p>
<p>Now you have a copy of mprime in your user directory. Start it by typing <code>./mprime</code></p>
<p><img src="{{ site.baseurl }}/assets/mprime-prompt.png" alt="" /></p>
<p>Just passing through, thanks. Answer <kbd>N</kbd> to the GIMPS prompt.</p>
<p>Next you'll be prompted for the number of torture test threads to run. They're smart here and always pick an equal number of threads to logical cores, so press <kbd>enter</kbd> to accept that. You want a full CPU test on all cores. Next, select the test type.</p>
<ol>
<li>Small FFTs (maximum heat and FPU stress, data fits in L2 cache, RAM not tested much).</li>
<li>In-place large FFTs (maximum power consumption, some RAM tested).</li>
<li>Blend (tests some of everything, lots of RAM tested).</li>
</ol>
<p>They're not kidding when they say "maximum power consumption", as you're about to learn. Select <kbd>2</kbd>. Then select <kbd>Y</kbd> to begin the torture and watch your CPU squirm in pain.</p>
<pre><code>Accept the answers above? (Y):
[Main thread Feb 14 05:48] Starting workers.
[Worker #2 Feb 14 05:48] Worker starting
[Worker #3 Feb 14 05:48] Worker starting
[Worker #3 Feb 14 05:48] Setting affinity to run worker on logical CPU #2
[Worker #4 Feb 14 05:48] Worker starting
[Worker #2 Feb 14 05:48] Setting affinity to run worker on logical CPU #3
[Worker #1 Feb 14 05:48] Worker starting
[Worker #1 Feb 14 05:48] Setting affinity to run worker on logical CPU #1
[Worker #4 Feb 14 05:48] Setting affinity to run worker on logical CPU #4
[Worker #2 Feb 14 05:48] Beginning a continuous self-test on your computer.
[Worker #4 Feb 14 05:48] Test 1, 44000 Lucas-Lehmer iterations of M7471105 using FMA3 FFT length 384K, Pass1=256, Pass2=1536.
</code></pre>
<p>Now's the time to break out your Kill-a-Watt or similar power consumption meter, if you have it, so you can <a href="http://blog.codinghorror.com/why-estimate-when-you-can-measure/" rel="noopener noreferrer" target="_blank">measure the maximum CPU power draw</a>. On most systems, unless you have an absolute beast of a gaming video card installed, the CPU is the single device that will pull the most heat and power in your system. This is full tilt, every core of your CPU burning as many cycles as possible.</p>
<p>I suggest running the <code>i7z</code> utility from another console session so you can monitor core temperatures and speeds while <code>mprime</code> is running its torture test.</p>
<pre><code>sudo apt-get install i7z
sudo i7z
</code></pre>
<p><strong>Let mprime run overnight in maximum heat torture test mode</strong>. The Mersenne calculations are meticulously checked, so if there are any mistakes the whole process will halt with an error at the console. And if mprime halts, ever &hellip; <em>your computer is not stable.</em></p>
<p><img src="{{ site.baseurl }}/assets/i7z-status.png" alt="" /></p>
<p><strong>Watch those CPU temperatures!</strong> In addition to absolute CPU temperatures, you'll also want to keep an eye on total heat dissipation in the system. The system fans (if any) should spin up, and the whole system should be kept at reasonable temperatures through this ordeal, or else you're going to have a sick, overheating computer one day.</p>
<p>The bad news is that it's extremely rare to have any kind of practical, real world workload remotely resembling the stress that Mersenne lays on your CPU. The good news is that if your system can survive the onslaught of Mersenne overnight, it's definitely ready for anything you can conceivably throw at it in the future.</p>
<h5>Disk</h5>
<p>Disks are probably the easiest items to replace in most systems &ndash; and the ones most likely to fail over time. We know the disk can't be totally broken since we just installed an OS on the thing, but let's be sure.</p>
<p>Start with a <a href="https://en.wikipedia.org/wiki/Badblocks" rel="noopener noreferrer" target="_blank">bad blocks test</a> for the whole drive.</p>
<pre><code>sudo badblocks -sv /dev/sda
</code></pre>
<p>This exercises the full extent of the disk (in safe read only fashion). Needless to say, any errors here should prompt serious concern for that drive.</p>
<pre><code>Checking blocks 0 to 125034839
Checking for bad blocks (read-only test): done
Pass completed, 0 bad blocks found. (0/0/0 errors)
</code></pre>
<p>Let's check the <a href="https://help.ubuntu.com/community/Smartmontools" rel="noopener noreferrer" target="_blank">SMART readings</a> for the drive next.</p>
<pre><code>sudo apt-get install smartmontools
smartctl -i /dev/sda 
</code></pre>
<p>That will let you know if the drive supports SMART. Let's enable it, if so, and see the basic drive stats:</p>
<pre><code>smartctl -s on /dev/sda
smartctl -a /dev/sda    
</code></pre>
<p>Now we can run some SMART tests. But first check how long the tests on offer will take:</p>
<pre><code>smartctl -c /dev/sda
</code></pre>
<p>Run the <code>long</code> test if you have the time, or the <code>short</code> test if you don't:</p>
<pre><code>smartctl -t long /dev/sda
</code></pre>
<p>It's done asynchronously, so after the time elapses, show the SMART test report and ensure you got a pass:</p>
<pre><code>smartctl -l selftest /dev/sda 
=== START OF READ SMART DATA SECTION ===
SMART Self-test log structure revision number 1
Num  Test_Description    Status                  Remaining  LifeTime(hours)  LBA_of_first_error
# 1  Extended offline    Completed without error       00%       100         -
</code></pre>
<p>Next, run a simple disk benchmark to see if you're getting roughly the performance you expect from the drive or array:</p>
<pre><code>dd bs=1M count=512 if=/dev/zero of=test conv=fdatasync
hdparm -Tt /dev/sda
</code></pre>
<p>For a system with a basic SSD you should see results at least this good, and perhaps considerably better:</p>
<pre><code>536870912 bytes (537 MB) copied, 1.52775 s, 351 MB/s
Timing cached reads:   11434 MB in  2.00 seconds = 5720.61 MB/sec
Timing buffered disk reads:  760 MB in  3.00 seconds = 253.09 MB/sec
</code></pre>
<p>Finally, let's try a more intensive test with <a href="https://en.wikipedia.org/wiki/Bonnie%2B%2B" rel="noopener noreferrer" target="_blank">bonnie++</a>, a disk benchmark:</p>
<pre><code>sudo apt-get install bonnie++
bonnie++ -f
</code></pre>
<p>We don't care too much about the resulting benchmark numbers here, what we're looking for is to pass without errors. And if you get errors during any of the above &hellip; <em>your computer is not stable.</em></p>
<p>(I think these disk tests are sufficient for general use, particularly if you consider drives easily RAID-able and replaceable as I do. However, if you want to test your drives more exhaustively, a good resource is the <a href="https://forums.freenas.org/index.php?threads/how-to-hard-drive-burn-in-testing.21451/" rel="noopener noreferrer" target="_blank">FreeNAS "how to burn in hard drives" topic</a>.)</p>
<h5>Network</h5>
<p>I don't have a lot of experience with network hardware failure, to be honest. But I do believe in the cult of bandwidth, and that's one thing we can check.</p>
<p>You'll need two machines for an <a href="https://en.wikipedia.org/wiki/Iperf" rel="noopener noreferrer" target="_blank">iperf</a> test, which makes it more complex. Here's the server, let's say it's at 10.0.0.1:</p>
<pre><code>sudo apt-get install iperf
iperf -s
</code></pre>
<p>and here's the client, which will connect to the server and record how fast it can transmit data between the two:</p>
<pre><code>sudo apt-get install iperf
iperf -c 10.0.0.1

------------------------------------------------------------
Client connecting to 10.0.0.1, TCP port 5001
TCP window size: 23.5 KByte (default)
------------------------------------------------------------
[  3] local 10.0.0.2 port 43220 connected with 10.0.0.1 port 5001
[ ID] Interval       Transfer     Bandwidth
[  3]  0.0-10.0 sec  1.09 GBytes    933 Mbits/sec
</code></pre>
<p>As a point of reference, you should expect to see roughly <strong>120 megabytes/sec (aka 960 megabits)</strong> of real world throughput on a single gigabit ethernet connection. If you're lucky enough to have a 10 gigabit connection, well, good luck reaching that meteoric 1.2 Gigabyte/sec theoretical throughput maximum.</p>
<h5>Video Card</h5>
<p>I'm not covering this, because very few of the computers I build these days need more than the stuff built into the CPU to handle video. Which is getting <a href="http://blog.codinghorror.com/the-2016-htpc-build/" rel="noopener noreferrer" target="_blank">surprisingly decent, at last</a>.</p>
<p>You're a gamer, right? So you'll probably want to boot into Windows and try something like <a href="http://www.ozone3d.net/benchmarks/fur/" rel="noopener noreferrer" target="_blank">furmark</a>. And you <em>should</em> test, because GPUs &ndash; especially gaming GPUs &ndash; are rather cutting edge bits of kit and burn through a lot of watts. Monitor temperatures and system heat, too.</p>
<p>If you have recommendations for gaming class video card stability testing, share them in the comments.</p>
<h5>OK, <em>Maybe</em> It's Stable</h5>
<p>This is the regimen I use on the machines I build and touch. And it's worked well for me. I've identified faulty CPUs (once), faulty RAM, faulty disks, and insufficient case airflow early on so that I could deal with them in the lab, before they became liabilities in the field. Doesn't mean they won't fail <em>eventually</em>, but I did all I could to make sure my <s>babies</s> computers can live long and prosper.</p>
<p>Who knows, with a bit of luck maybe you'll end up like the guy whose <a href="http://arstechnica.com/information-technology/2013/03/epic-uptime-achievement-can-you-beat-16-years/" rel="noopener noreferrer" target="_blank">netware server had <strong>sixteen years</strong> of uptime</a> before it was decommissioned.</p>
<p><img src="{{ site.baseurl }}/assets/16-years-of-uptime.jpg" alt="" /></p>
<p>These tests are just a starting point. What techniques do you use to ensure the computers you build are stable? How would you improve on these stability tests based on your real world experience?</p>
<table>
<tr>
<td>
[advertisement] At Stack Overflow, we help developers learn, share, and grow. Whether you&rsquo;re looking for your next dream job or looking to build out your team, <a href="http://careers.stackoverflow.com" rel="noopener noreferrer" target="_blank">we've got your back</a>.
</td>
</tr>
</table>
</div>
</div>
<p>Read More <a href="https://blog.codinghorror.com/is-your-computer-stable/" target="_blank">here</a>.</p>
