---
layout: post
title: Tor Hidden Services
date: 2018-05-22 12:00:00.000000000 -04:00
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
<div>  <img src="{{ site.baseurl }}/assets/Tor-logo-2011-flat.svg_.png" width="640" height="387" alt="" /></div>
<div>by <a title="View user profile." href="https://www.linuxjournal.com/users/kyle-rankin" rel="noopener noreferrer" target="_blank">Kyle Rankin</a></div>
<div>
<p><em><br />
Why should clients get all the privacy? Give your servers some<br />
privacy too!</em></p>
<p>
When people write privacy guides, for the most part they are written from<br />
the perspective of the client. Whether you are using HTTPS, blocking<br />
tracking cookies or going so far as to browse the internet over Tor,<br />
those privacy guides focus on helping end users protect themselves<br />
from the potentially malicious and spying web. Since many people who<br />
read <em>Linux Journal</em> sit on the other side of that equation&mdash;they run the<br />
servers that host those privacy-defeating services&mdash;system administrators also<br />
should step up and do their part to help user privacy. Although part of that just<br />
means making sure your services support TLS, in this article, I describe<br />
how to go one step further and make it possible for your<br />
users to use your services completely anonymously via Tor hidden services.</p>
<h3>
How It Works</h3>
<p>
I'm not going to dive into the details of how Tor itself works<br />
so you can use the web anonymously&mdash;for those details, check out<br />
<a href="https://tor.eff.org" rel="noopener noreferrer" target="_blank">https://tor.eff.org</a>. Tor hidden services work within the Tor network and<br />
allow you to register an internal, Tor-only service that gets its own<br />
.onion hostname. When visitors connect to the Tor network, Tor resolves<br />
those .onion addresses and directs you to the anonymous service sitting<br />
behind that name. Unlike with other services though, hidden services<br />
provide two-way anonymity. The server doesn't know the IP of the client,<br />
like with any service you access over Tor, but the client also doesn't<br />
know the IP of the server. This provides the ultimate in privacy since<br />
it's being protected on both sides.</p>
<h3>
Warnings and Planning</h3>
<p>
As with setting up a Tor node itself, some<br />
planning is involved if you want to set up a Tor hidden service<br />
so you don't defeat Tor's anonymity via some operational<br />
mistake. There are a lot of rules both from an operational<br />
and security standpoint, so I recommend you read this <a href="https://riseup.net/en/security/network-security/tor/onionservices-best-practices" rel="noopener noreferrer" target="_blank">excellent<br />
guide</a><br />
to find the latest best practices all in one place.</p>
<p>
Without diving into all of those steps, I do want to list a few general-purpose<br />
guidelines here. First, you'll want to make sure that whatever<br />
service you are hosting is listening only on localhost (127.0.0.1) and<br />
isn't viewable via the regular internet. Otherwise, someone may be able<br />
to correlate your hidden service with the public one. Next, go through<br />
whatever service you are running and try to scrub specific identifying<br />
information from it. That means if you are hosting a web service, modify<br />
your web server so it doesn't report its software type or version, and<br />
if you are running a dynamic site, make sure whatever web applications<br />
you use don't report their versions either.</p>
</div>
<div>  <a href="https://www.linuxjournal.com/content/tor-hidden-services" rel="noopener noreferrer" target="_blank">Go to Full Article</a>
</div>
</p></div>
</p></div>
</div>
<p>Read More <a href="https://www.linuxjournal.com/content/tor-hidden-services" target="_blank">here</a>.</p>
