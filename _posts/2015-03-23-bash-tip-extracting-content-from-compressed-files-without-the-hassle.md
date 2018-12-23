---
layout: post
title: 'Bash tip: Extracting content from compressed files without the hassle'
date: 2015-03-23 21:09:31.000000000 -04:00
author: Michael J. Ford
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Bash tips
- General Computing
- Linux
- Servers
tags:
- bash
- CLI
- command line
- Linux
meta: {}
---
<p>If you work with file archives that come in many different "flavors", you might find this little trick to be a handy one. Add a function to your .bash_profile that runs the proper extraction command based on the file's extension(s). This will keep you from ever having to scratch your head to make sure you're selecting the right options for any of these commands. Add a "v" (for verbose) to the tar commands in the list if you want to see the files listed as they are being extracted.</p>
<p>See the script after the jump. </p>
<p><!--more--> </p>
<p><a href="http://www.itworld.com/article/2895888/extracting-content-without-the-hassle.html">http://www.itworld.com/article/2895888/extracting-content-without-the-hassle.html</a></p>
