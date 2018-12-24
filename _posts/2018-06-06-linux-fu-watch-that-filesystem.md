---
layout: post
title: 'Linux Fu: Watch That Filesystem'
date: 2018-06-06 17:01:05.000000000 -04:00
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
<p>The UNIX Way<img src="{{ site.baseurl }}/assets/2122.png" alt="&trade;" /> is to cobble together different, single-purpose programs to get the effect you want, for instance in a Bash script that you run by typing its name into the command line. But sometimes you want the system to react to changes in the system without your intervention. For example, you might like to watch a directory and kick off some program automatically when a file appears from a completed FTP transaction, without having to sit there and refresh the directory yourself.</p>
<p>The simple but ugly way to do this just scans the directory periodically. Here&rsquo;s a really dumb shell script:</p>
<pre>#!/bin/bash
while true
 do
   for I in `ls`
    do cat $I; rm $I
   done
 sleep 10
done</pre>
<p>Just for an example, I dump the file to the console and remove it, but in real life, you&rsquo;d do something more interesting. This is really not a good script because it executes all the time and it just isn&rsquo;t a very elegant solution. (If you think I should use <code>for I in *</code>, try doing that in an empty directory and you&rsquo;ll see why I use the <code>ls</code> command instead.)</p>
<p><span></span></p>
<h2>Increase Elegance</h2>
<p>Honestly, you want something more elegant right? Modern kernels (2.6.13 and later) have filesystem notifications in the form of an interface called <code>inotify</code>. You can use these calls programmatically with the <code>sys/inotify.h</code> header file. There is also a set of command-line programs you can install, usually packaged as <code>inotify-tools</code>.</p>
<p>One of those tools is <code>inotifywait</code> and it makes for a nicer script. For example:</p>
<pre>#!/bin/bash
while true
 do
   if FN=`inotifywait &ndash;e close_write,moved_to --format %f .`
   then
    cat $FN
    rm $FN
   fi
 done</pre>
<p>That&rsquo;s better, I think. It doesn&rsquo;t wake up frequently, only when something has changed. I figure any sane program putting something in the directory will either open the file for writing and close it, or it will move it. Either way will work and the <code>%f</code> tells the command to report the file name. There are other events you can wait for as well, of course.</p>
<p>If you are wondering why the move case is necessary, think about how most text editors and network download software works. Usually, a new file doesn&rsquo;t have the final name until it is complete. For example, Chrome will download the file <code>test.txt</code> as <code>test.txt.crdownload</code> or something like that. Only when the file is done will it rename (move) the file to <code>test.txt</code>.</p>
<p>If you want to try the command without a script so you can see the effect, just open up two terminal windows like this:</p>
<p><a href="https://hackadaycom.files.wordpress.com/2018/05/ss.png" target="_blank" rel="noopener noreferrer"><img src="{{ site.baseurl }}/assets/ss.png?w=800&amp;h=418" alt="" width="800" height="418" /></a></p>
<p>In the lower terminal, issue the <code>inotifywait</code> command. Don&rsquo;t forget the period at the end which tells it to monitor the current directory. Then in the other terminal create a file in the same directory. The name of the file will appear in the first terminal and the program will exit. The script just takes advantage of this behavior to set the FN variable, takes action, and then relaunches <code>inotifywait</code>. You can ask the program not to quit, by the way, but that makes scripting a little more difficult. However, it also removes the problem of a file changing while you are doing your processing.</p>
<p>The other command line, <code>inotifywatch</code>, also outputs file change events but it watches for a certain amount of time and then gives you a summary of changes. I won&rsquo;t talk about it any further. If you think you need that capability, you can read the man page.</p>
<h2>A New Cron</h2>
<p>The script is still less than ideal, though. Presumably, a system might have lots of different directories it wants to monitor. You really don&rsquo;t want to repeat this script, or a variation of it, for each case.</p>
<p>There is another program for that, called <code>incron</code> (you will almost surely have to install this one). The <code>incron</code> program is like cron but instead of time-based events, the events are based on file notifications. Once you install it, you will probably have to change <code>/etc/incron.allow</code> and <code>/etc/incron.deny</code> if you want to actually use it, especially as a normal user.</p>
<p>Suppose you want to run a script when a file appears in the hexfiles directory. You can use the command <code>incrontab -e</code> to edit your <code>incron</code> table. The format is very picky (it wants spaces, not tabs, for example). Here&rsquo;s a line from the file that will do the job:</p>
<pre>/home/alw/Downloads/hexfiles IN_CLOSE_WRITE,IN_MOVED_TO /home/alw/bin/program_cpu $@/$#</pre>
<p>The <code>$@/$#</code> at the end provides a full path to the file affected. You can also grab the vent time as text (<code>$%</code>) or a number (<code>$&amp;</code>). You can monitor all the usual events and also set options to do things like not dereference symbolic links. You can find it all in the <code>incron</code> man pages.</p>
<h2>GUI</h2>
<p><a href="https://hackadaycom.files.wordpress.com/2018/05/jincron.png" target="_blank" rel="noopener noreferrer"><img src="{{ site.baseurl }}/assets/jincron.png?w=248&amp;h=250" alt="" width="248" height="250" /></a>I&rsquo;m not a big fan of GUI editors, but I know I&rsquo;m in the minority. If you like, there&rsquo;s <a href="http://jincron.aiken.cz/" target="_blank" rel="noopener noreferrer">a Java-based incrontab editor</a> available. There isn&rsquo;t much documentation, but you can import your <code>incrontab</code> &mdash; if it exists &mdash; from <code>/var/spool/incron/your_user_id</code>. If you look at the image below, you can see it offers a form that builds the <code>incron</code> table line for you.</p>
<p>You can find the system files in <code>/etc/incron.d</code>, usually. All the locations can be set by the <code>/etc/incron.conf</code> file, so if you aren&rsquo;t sure where to look or you want to change the location for the table files, start there.</p>
<h2>Go Forth and Watch</h2>
<p>Using <code>incron</code> is quite elegant. A system program does all the waiting and our script only runs when necessary. It is easy to look and see all the things you have notifications set for. You can do a lot with these tools, and not just in the embedded space. How are you going to use them?</p>
<p><img src="{{ site.baseurl }}/assets/fQpUPcCsNKc" height="1" width="1" alt="" /></div>
<p>Read More <a href="http://feedproxy.google.com/~r/hackaday/LgoM/~3/fQpUPcCsNKc/" target="_blank">here</a>.</p>
