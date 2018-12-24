---
layout: post
title: 'Linux-Fu: Keeping Things Running'
date: 2017-03-09 18:01:10.000000000 -05:00
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
<p>If you&rsquo;ve used Linux from the early days (or, like me, started with Unix), you didn&rsquo;t have to learn as much right away and as things have become more complex, you can kind of pick things up as you go. If you are only starting with Linux because you are using a Raspberry Pi, became unhappy with XP being orphaned, or you are running a cloud server for your latest Skynet-like IoT project, it can be daunting to pick it all up in one place.</p>
<p>Recently my son asked me how do you make something run on a Linux box even after you log off. I thought that was a pretty good question and not necessarily a simple answer, depending on what you want to accomplish.</p>
<p>There&rsquo;s really four different cases I could think of:</p>
<ol>
<li>You want to launch something you know will take a long time.</li>
<li>You run something, realize it is going to take a long time, and want to log off without stopping it.</li>
<li>You want to write a script or other kind of program that detaches itself and keeps running (known as a daemon).</li>
<li>You want some program to run all the time, even if you didn&rsquo;t log in after a reboot.</li>
</ol>
<p><span></span></p>
<p>One of the things that makes Linux tough is that there are lots of options and so what works on one system may not work on another. If you can assume a single distribution, you might have a better chance of finding that things work the same. To keep things manageable, I&rsquo;m going to focus on the first two items and maybe catch up on the last two at a later date. I&rsquo;m also going to assume we are talking about command line programs. If you need to run graphical programs after you log out, that opens a lot of strange questions &mdash; it&rsquo;s certainly&nbsp;possible, but it is strange since your graphical user environment will go away when you log out (hint: use <a href="https://en.wikipedia.org/wiki/Virtual_Network_Computing" target="_blank" rel="noreferrer">VNC</a> or <a href="https://www.nomachine.com/" target="_blank" rel="noreferrer">Nx</a> to create a persistent desktop).</p>
<p>I will, however, give you clues for the last two use cases. First, a program that detaches itself is a <a href="http://www.netzmafia.de/skripten/unix/linux-daemon-howto.html" target="_blank" rel="noreferrer">daemon</a>. The steps to do that can be involved, or you can <a href="http://libslack.org/daemon/" target="_blank" rel="noreferrer">outsource it</a>. Making a program run all the time can be simple or complicated. Most Linux distributions will have a file /etc/rc.local that runs as root on start up (at least, normal start up). You can add things there if you are just doing a one-off. Otherwise, you need to know if you are using SystemV init, Upstart, OpenRC, or Systemd. There are probably some others to contend with, as well. But that&rsquo;s a topic for another day. If you can&rsquo;t wait, try your Portuguese &nbsp;(or Google&rsquo;s) on this <a href="https://www.ibm.com/developerworks/community/blogs/752a690f-8e93-4948-b7a3-c060117e8665/entry/comparativo_upstart_sysvinit_systemd_openrc?lang=en" target="_blank" rel="noreferrer">IBM paper</a>.</p>
<h2>A Tale of Two Cases</h2>
<p><a href="https://hackadaycom.files.wordpress.com/2017/02/offline.png" target="_blank" rel="noreferrer"><img src="{{ site.baseurl }}/assets/offline.png?w=250&amp;h=250" alt="offline" width="250" height="250" /></a>Back to the first two cases, though. Suppose you are going to run a program <code>remote_backup</code> and you know you will launch it (perhaps using an ssh session) and then you&rsquo;ll want to disconnect or make sure it doesn&rsquo;t stop running if you accidentally get disconnected for some reason. Either way, just launching it from the command line means when your session exits, the program will stop. Or will it?</p>
<p>This can be as easy as&nbsp;running programs in a way that they will be immune to hangups. If you use bash and your options are set right, the answer can be very simple. If you use the &amp; to start a program in the background, or you can suspend a running program (Control+Z) and move it to the background with the bg command, it might run even after you exit your session. This is another case of Linux&rsquo;s flexibility &mdash; many ways to accomplish similar results &mdash; getting in the way.</p>
<p>If you run bash, you can see your &ldquo;shell options&rdquo; which include the &ldquo;hangup on exit&rdquo; settings. Try running this at&nbsp;a shell prompt:</p>
<pre>shopt | grep huponexit</pre>
<p>If you see that huponexit is set to off, then simply pushing a program into the background will let it survive your session disconnecting. Of course, if you try the same thing on another system, it might not work, and you&rsquo;ll wonder why.</p>
<p>If you know you are running bash with huponexit off, you can run your program in the background very simply by ending with a single ampersand:</p>
<pre>remote_backup &amp;</pre>
<p>However, it is safer to expressly prevent the program from dying if you log off. If for no other reason, because you won&rsquo;t see any output from the program after you log off if you use the above method. If you are thinking ahead you can just run the program with &ldquo;no hangup&rdquo;:</p>
<pre>nohup&nbsp;remote_backup</pre>
<p>If you need arguments, just put them on the end like usual. Nohup will do a few things:</p>
<ol>
<li>Redirect stderr to stdout</li>
<li>Redirect stdout to nohup.out (depending on your version of nohup, that may be in ~/nohup.out)</li>
<li>Redirect stdin to an unreadable file</li>
<li>Run the program and return to the shell prompt</li>
</ol>
<p>The end result is the program will run, can&rsquo;t get any input, and will put any output into nohup.out. If there is any data already in nohup.out, the new data will go to the end.</p>
<p>The reason all of these redirections occur is because nohup detects that each stream is connected to a terminal. If you already redirect things to a file, nohup won&rsquo;t disturb them. So you could say:</p>
<pre>nohup remote_backup &gt;/tmp/backupstatus.log &amp;</pre>
<p>or</p>
<pre>nohup bash -c 'echo y | remote_backup &gt;tmp/backupstatus.log' &amp;</pre>
<p>That last line will take input from the pipe so it won&rsquo;t redirect. All output (stdout and stderr) will go to /tmp/backupstatus.log.</p>
<h2>Prove It!</h2>
<p><a href="https://hackadaycom.files.wordpress.com/2017/02/tuxwiz.png" target="_blank" rel="noreferrer"><img src="{{ site.baseurl }}/assets/tuxwiz.png?w=192&amp;h=250" alt="tuxwiz" width="192" height="250" /></a>If you want to see the difference nohup makes, log into a Linux server with ssh and type the following at a shell prompt:</p>
<pre>echo '#!/bin/bash' &gt;~/huptest</pre>
<pre>echo sleep 60 &gt;&gt;~/huptest
echo 'date &gt;/tmp/test.txt' &gt;&gt;~/huptest
chmod +x ~/huptest
~/huptest</pre>
<p>Before the 60 seconds expire, press the tilde key (~). Then press a period. The tilde is the SSH escape character (if it is at the start of a line). Press ~? if you want to know about more things you do with it. Now, go have a cup of whatever you drink and come back&nbsp;way after a minute. Log in. You won&rsquo;t find a file at /tmp/test.txt (unless there was one already there, but the contents should make that clear). Your session ending killed the waiting program.</p>
<p>Now try this:</p>
<pre>~/huptest &amp;</pre>
<p>That tells the shell not to wait for the program to finish. If you are using bash, this will work as long as huponexit is set to off. You can experiment by using:</p>
<pre>shopt -s huponexit</pre>
<p>and</p>
<pre>shopt -u huponexit</pre>
<p>The first command will turn the flag on. The second one resets the flag to off.</p>
<p>Finally, you can run the same commands with nohup:</p>
<pre>nohup ~/huptest &amp;</pre>
<p>Even nohup isn&rsquo;t perfect. If the program you are running intercepts the nohup flag itself, nohup won&rsquo;t help you. However, most programs you are going to worry about do not catch the flag and using nohup will work regardless of the shell and its configuration.</p>
<p>There are many other ways you could do this. For example, investigate the at command which will run things at a specified time (which could be a second after the current time). The programs run will use sh, not bash (without some work) but will run detached.</p>
<h2>Lack of Planning</h2>
<p>There are two cases where nohup doesn&rsquo;t help, though. First, you may not have planned for this ahead of time. If you start running something and realize it is going to take awhile or that you suddenly have to leave, what do you do? You might also have the case where you need to start a program, give it some manual input, and then want it to run without you watching it.</p>
<p>You may know that you can suspend a running program using the Control+Z key. Bash will tell you that it has created a job and will give you a number for that job. You can force it into the background using bg. For example, if the job number is 3:</p>
<pre>bg %3</pre>
<p>If you have huponexit off, that&rsquo;s all you need to do. But in the general case, you need to tell the shell to either stop sending HUP signals to it or to remove it from the job table completely. You can use disown (built into bash) to do both of these functions.</p>
<p>Using disown with no options will remove the named jobs from the job table. If you don&rsquo;t want that extreme, use the -h option to just inhibit the HUP signal from the shell for that one job. You can also specify -a to hit all jobs or -r to affect all running jobs. This is a built-in command for bash (so man bash to read more) and so it is shell dependent. Once you&rsquo;ve disowned a background task, you can log off with no fear</p>
<h2>Persistent Sessions</h2>
<p><a href="https://hackadaycom.files.wordpress.com/2017/02/by.png" target="_blank" rel="noreferrer"><img src="{{ site.baseurl }}/assets/by.png?w=400&amp;h=302" alt="by" width="400" height="302" /></a>As usual with Linux, there is more than one way to go for any given task. You could use screen or tmux to provide a session. This is similar to VNC where you can log in and find whatever you were working on still there. If you do a lot of work at the command line, you might want to try byobu which gives a nicer interface to screen or tmux (see right).</p>
<p>If you want to try that, log in as usual, run screen, tmux, or byobu. Execute the test script again (with or without the &amp; at the end). And use the ~. trick to kill the ssh session. &nbsp;Then log back in and restart the same program (screen, tmux, or byobu). You&rsquo;ll see the screen there just like you left it. That&rsquo;s handy in general. Plus you can build multiple graphic-like windows easily, but that&rsquo;s not germane to this post.</p>
<h2>Options</h2>
<p>Part of the power of Linux is you have plenty of options. Part of the problem with Linux is you have plenty of options. The real problem isn&rsquo;t if you are configuring a single Raspberry Pi where you control everything. You make it work and that&rsquo;s that. The real issue is when you try to deploy something to multiple boxes with different users and maybe even different distributions.</p>
<p>Of course, this isn&rsquo;t the only thing that causes you trouble if you are going for portability. If you are trying to write scripts that can go to many different Posix systems, you might read the <a href="https://www.gnu.org/savannah-checkouts/gnu/autoconf/manual/autoconf-2.69/html_node/Portable-Shell.html#Portable-Shell" target="_blank" rel="noreferrer">GNU documentation for </a>autoconf which has a lot of good information about the problems and solutions.</p>
<p>Filed under: <a href="http://feeds.wordpress.com/1.0/gocomments/hackadaycom.wordpress.com/246002/" rel="noreferrer" target="_blank">Hackaday Columns</a>, <a href="http://hackaday.com/category/linux-hacks/" rel="noreferrer" target="_blank">linux hacks</a>, <a href="http://hackaday.com/category/curated/skills/" rel="noreferrer" target="_blank">Skills</a>  <a rel="nofollow" target="_blank"><img alt="" border="0" src="{{ site.baseurl }}/assets/246002" /></a> <img alt="" border="0" src="{{ site.baseurl }}/assets/b.gif?host=hackaday.com&amp;blog=4779443&amp;post=246002&amp;subd=hackadaycom&amp;ref=&amp;feed=1" width="1" height="1" /><img src="{{ site.baseurl }}/assets/E_p3ULAsmi4" height="1" width="1" alt="" /></div>
<p>Read More <a href="https://hackaday.com/2017/03/10/linux-fu-keeping-things-running/" target="_blank">here</a>.</p>
