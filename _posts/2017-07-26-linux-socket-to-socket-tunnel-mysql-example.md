---
layout: post
title: Linux socket-to-socket tunnel (MySQL example)
date: 2017-07-26 11:32:19.000000000 -04:00
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
<div><code>$ socat "UNIX-LISTEN:/tmp/mysqld.temp.sock,reuseaddr,fork" EXEC:"ssh username@remoteserver.com socat STDIO UNIX-CONNECT:/var/run/mysqld/mysqld.sock"</code></p>
<div>
<p>Won't work with password login. You must add your RSA key to the server's authorizedkeys file, or change the ssh command adding the -i option for a custom RSA key:</p>
<p><code>socat "UNIX-LISTEN:/tmp/mysqld.temp.sock,reuseaddr,fork" EXEC:"ssh username@remoteserver.com -i /home/user/rsa-keys/id_rsa socat STDIO UNIX-CONNECT:/var/run/mysqld/mysqld.sock"</code></p>
<p>---</p>
<p>/tmp/mysqld.temp.sock will be created locally by socat, don't create it yourself. The folder it lives must be writable. Connect your MySQL client to this socket, with database and username set properly.</p>
<p>---</p>
<p>In case you need to forward a remote socket to a LOCAL PORT instead, check <a href="http://www.commandlinefu.com/commands/view/9436/socat-tcp-listen5500-execssh-userremotehost-socat-stdio-unix-connectvarrunmysqldmysqld.sock" rel="noopener noreferrer" target="_blank">http://www.commandlinefu.com/commands/view/9436/socat-tcp-listen5500-execssh-userremotehost-socat-stdio-unix-connectvarrunmysqldmysqld.sock</a></p>
</div>
<ul>
<li><a href="http://www.commandlinefu.com/commands/view/20957/linux-socket-to-socket-tunnel-mysql-example" rel="noopener noreferrer" target="_blank">View this command to comment, vote or add to favourites</a></li>
<li><a href="https://app.slashetc.us/commands/by/paulera" rel="noopener noreferrer" target="_blank">View all commands by <strong>paulera</strong></a></li>
</ul>
<p><a href="http://www.commandlinefu.com" rel="noopener noreferrer" target="_blank"><img src="{{ site.baseurl }}/assets/small-logo.jpg" alt="commandlinefu.com" /></a></p>
<p>Diff your entire server config at <a href="http://www.scriptrock.com?utm_campaign=clfu&amp;utm_medium=clfu%20api%20rss&amp;utm_source=clfu" rel="noopener noreferrer" target="_blank">ScriptRock.com</a></p>
<p><img src="{{ site.baseurl }}/assets/QzDORsGJoXQ" height="1" width="1" alt="" /></div>
<p>Read More <a href="http://www.commandlinefu.com/commands/view/20957/linux-socket-to-socket-tunnel-mysql-example" target="_blank">here</a>.</p>
