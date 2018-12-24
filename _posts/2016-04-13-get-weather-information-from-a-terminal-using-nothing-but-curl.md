---
layout: post
title: Get Weather Information From A Terminal Using Nothing But cURL
date: 2016-04-13 12:33:00.000000000 -04:00
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
<div><?xml version="1.0" encoding="utf-8" standalone="yes"?>
<div><a href="https://github.com/chubin/wttr.in" rel="noreferrer" target="_blank">wttr.in</a> is a web frontent for <a href="https://github.com/schachmat/wego" rel="noreferrer" target="_blank">Wego</a>, a weather app for the terminal. <b>Using nothing but cURL and wttr.in, you can get weather information from a terminal, without having to install anything (well, except for cURL).</b></div>
<div></div>
<div><a href="https://4.bp.blogspot.com/-DBo9ur9dcuA/Vw-FoZnltHI/AAAAAAAAXik/zRyyygipQfIxvYL3bV8Svp0tr8Fnm3r3gCLcB/s1600/wttrin-weather-terminal.png" imageanchor="1" rel="noreferrer" target="_blank"><img alt="" border="0" height="257" src="{{ site.baseurl }}/assets/wttrin-weather-terminal.png" title="wttr.in weather terminal" width="400" /></a></div>
<p><a name="more"></a>
<div><b>By default, the service tries to determine your location automatically but if that fails, it allows you to specify the location in multiple ways,</b> by city name, 3-letter airport code, ZIP code and it even allows retrieving the weather for the location of a domain name or IP address.</div>
<div></div>
<div>Other features include a <b>3-day weather forecast, display moon phase</b> and support for both metric (SI; used by default everywhere except the US) and USCS (used by default in the US).</div>
<div></div>
<div>Under the hood, wttr.in uses WorldWeatherOnline for the weather information, though this might change in the future because Wego, the tool which wttr.in uses as its backend, recently switched to forecast.io by default.</div>
<div></div>
<div>While it's not required to install wttr.in to use it, you can install it on your server if you want - see the installation instructions <a href="https://github.com/chubin/wttr.in#installation" rel="noreferrer" target="_blank">HERE</a>.</div>
<div></div>
<div>wttr.in: <a href="http://wttr.in/" rel="noreferrer" target="_blank">online service</a> | <a href="https://github.com/chubin/wttr.in" rel="noreferrer" target="_blank">source</a> (GitHub)</div>
<h2>Using wttr.in</h2>
<p>
<div><b>The only requirement for using wttr.in is "curl". Ubuntu / Debian / Linux Mint users (and derivatives) can install it using the following command:</b></div>
<pre><code>sudo apt-get install curl</code></pre>
<p>
<div><b>Then simply run the command below to get the weather for your current location:</b></div>
<pre><code>curl wttr.in</code></pre>
<p>
<div>If wttr.in fails to detect your current location or if you want to get weather information for a different location, you can specify 3-letter airport codes, city names, domain names or IP addresses. Here are a few examples:</div>
<div></div>
<p>- get the weather for New York (for city names containing spaces, you can either enter the name in quotes or use an underscore - for example, both "new york" and new_york work):
<pre><code>curl wttr.in/"new york"</code></pre>
<p>- get the weather for Paris:
<pre><code>curl wttr.in/paris</code></pre>
<p>- get the weather for London using a 3-letter airport code (I'm using the Heathrow Airport code below):
<pre><code>curl wttr.in/lhr</code></pre>
<p>- get weather information using a ZIP code (note that wttr.in calls this "area code" but area codes are actually a different thing):
<pre><code>curl wttr.in/60602</code></pre>
<p>
<div>To retrieve the weather using metric or USCS units, simply append "?m" or "?u" to the command (without the quotes). For instance, to force metric (SI) units to get weather information for Chicago, you would use:</div>
<pre><code>curl wttr.in/chicago?m</code></pre>
<p>
<div><b>To see all the available options, use "curl wttr.in/:help". Here's how it looks like:</b></div>
<pre><code>$ curl wttr.in/:help<br />Usage:<br /><br />$ curl wttr.in          # current location<br />$ curl wttr.in/muc      # weather in the Munic airport<br /><br />Supported location types:<br /><br />/paris                  # city name<br />/muc                    # airport code (3 letters)<br />/@stackoverflow.com     # domain name<br />/94107                  # area codes<br /><br />Special locations:<br /><br />/moon                   # Moon phase (add ,+US or ,+France for these<br />                          cities)<br /><br />Units:<br /><br />?m                      # metric (SI) (used by default everywhere <br />                          except US)<br />?u                      # USCS (used by default in US)<br /><br />Special URLs:<br /><br />/:help                  # show this page</code></pre>
<h3>Tip: add a wattr.io alias for your current location</h3>
<div>From what I've read, it looks like wttr.io fails to get the location pretty often so to make it easy to use, you can add an alias to your <i>~/.bashrc</i> file so you can simply type "wttr" to get the weather for your location.</div>
<div></div>
<p>To add an wttr.io alias for your current location, open <i>~/.bashrc</i> with a text editor - I'll use Gedit below:
<pre><code>gedit ~/.bashrc</code></pre>
<p>and at the end of the file, paste this:
<pre><code>alias wttr='curl wttr.in/CITY_NAME'</code></pre>
<div>... replace "CITY_NAME" with your city name (or another way supported by wttr.in for getting your location - ZIP code, airpode code, etc. - see above), then save the file and run the following command in a terminal to source the ~/.bashrc file (use its new configuration):</div>
<pre><code>. ~/.bashrc</code></pre>
<p>(there's a dot, then a space before <i>~/.bashrc</i>)</p>
<p><b>After you're done, you can simply type "wttr" in a terminal to check the weather.</b></p>
<div><i>seen @ <a href="https://ubuntulife.wordpress.com/2016/04/09/conocer-la-prediccion-del-tiempo-desde-la-terminal/" rel="noreferrer" target="_blank">UbuntuLife</a>, <a href="http://www.taringa.net/post/linux/19371841/Saber-el-clima-por-medio-de-la-terminal.html" rel="noreferrer" target="_blank">Taringa</a></i></div>
<div>
<p>Originally published at <a href="http://www.webupd8.org" rel="noreferrer" target="_blank">WebUpd8: Daily Ubuntu / Linux news and application reviews</a>.</p>
<p><img src="{{ site.baseurl }}/assets/counter.php?sc_project=4594652&amp;java=0&amp;security=c8929556&amp;invisible=1" border="0" width="0" height="0" /></div>
<div>
<a href="http://feeds.feedburner.com/~ff/webupd8?a=sIK8Ppqq3mc:Eh8DYEFfp9M:JO5tD9-akdk" rel="noreferrer" target="_blank"><img src="{{ site.baseurl }}/assets/webupd8?i=sIK8Ppqq3mc:Eh8DYEFfp9M:JO5tD9-akdk" border="0" /></a> <a href="http://feeds.feedburner.com/~ff/webupd8?a=sIK8Ppqq3mc:Eh8DYEFfp9M:yIl2AUoC8zA" rel="noreferrer" target="_blank"><img src="{{ site.baseurl }}/assets/webupd8?d=yIl2AUoC8zA" border="0" /></a> <a href="http://feeds.feedburner.com/~ff/webupd8?a=sIK8Ppqq3mc:Eh8DYEFfp9M:F7zBnMyn0Lo" rel="noreferrer" target="_blank"><img src="{{ site.baseurl }}/assets/webupd8?i=sIK8Ppqq3mc:Eh8DYEFfp9M:F7zBnMyn0Lo" border="0" /></a> <a href="http://feeds.feedburner.com/~ff/webupd8?a=sIK8Ppqq3mc:Eh8DYEFfp9M:qj6IDK7rITs" rel="noreferrer" target="_blank"><img src="{{ site.baseurl }}/assets/webupd8?d=qj6IDK7rITs" border="0" /></a> <a href="http://feeds.feedburner.com/~ff/webupd8?a=sIK8Ppqq3mc:Eh8DYEFfp9M:GpuqGqRPqxU" rel="noreferrer" target="_blank"><img src="{{ site.baseurl }}/assets/webupd8?d=GpuqGqRPqxU" border="0" /></a> <a href="http://feeds.feedburner.com/~ff/webupd8?a=sIK8Ppqq3mc:Eh8DYEFfp9M:3k7XFKuwfg0" rel="noreferrer" target="_blank"><img src="{{ site.baseurl }}/assets/webupd8?d=3k7XFKuwfg0" border="0" /></a> <a href="http://feeds.feedburner.com/~ff/webupd8?a=sIK8Ppqq3mc:Eh8DYEFfp9M:KGf-JMduG9I" rel="noreferrer" target="_blank"><img src="{{ site.baseurl }}/assets/webupd8?d=KGf-JMduG9I" border="0" /></a> <a href="http://feeds.feedburner.com/~ff/webupd8?a=sIK8Ppqq3mc:Eh8DYEFfp9M:Y9kCARWg6Fw" rel="noreferrer" target="_blank"><img src="{{ site.baseurl }}/assets/webupd8?d=Y9kCARWg6Fw" border="0" /></a> <a href="http://feeds.feedburner.com/~ff/webupd8?a=sIK8Ppqq3mc:Eh8DYEFfp9M:gIN9vFwOqvQ" rel="noreferrer" target="_blank"><img src="{{ site.baseurl }}/assets/webupd8?i=sIK8Ppqq3mc:Eh8DYEFfp9M:gIN9vFwOqvQ" border="0" /></a>
</div>
<p><img src="{{ site.baseurl }}/assets/sIK8Ppqq3mc" height="1" width="1" alt="" /></div>
