---
layout: post
title: DNS-over-HTTPS Is the Wrong Partial Solution
date: 2019-10-21 14:00:46 00:00
author: TT-RSS Published Article
type: post
published: true
status: publish
categories:
- TT-RSS Published Articles
---

Openness has been one of the defining characteristics of the Internet for as long as it has existed, with much of the traffic today still passed without any form of encryption. Most requests for HTML pages and associated content are in plain text, and the responses are returned in the same way, even though HTTPS has been around since 1994.

But sometimes there's a need for security and/or privacy. While the encryption of internet traffic has become more widespread for online banking, shopping, the privacy-preserving aspect of many internet protocols hasn't kept pace. In particular, when you look up a website's IP address by hostname, the DNS request is almost always transmitted in plain text, allowing all the computers and ISPs along the way to determine what website you were browsing, even if you use HTTPS once the connection is made.

The idea of also encrypting DNS requests isn't exactly new, with the first attempts starting in the early 2000s, in the form of DNSCrypt, DNS over TLS (DoT), and others. Mozilla, Google, and a few other large internet companies are pushing a new method to encrypt DNS requests: DNS over HTTPS (DoH).

DoH not only encrypts the DNS request, but it also serves it to a "normal" web server rather than a DNS server, making the DNS request traffic essentially indistinguishable from normal HTTPS. This is a double-edged sword. While it protects the DNS request itself, just as DNSCrypt or DoT do, it also makes it impossible for the folks in charge of security at large firms to monitor DNS spoofing and it moves the responsibility for a critical networking function from the operating system into an application. It also doesn't do anything to hide the IP address of the website that you just looked up --- you still go to visit it, after all.

And in comparison to DoT, DoH centralizes information about your browsing in a few companies: at the moment Cloudflare, who says they will throw your data away within 24 hours, and Google, who seems intent on retaining and monetizing every detail about everything you've ever thought about doing.

DNS and privacy are important topics, so we're going to dig into the details here.

Name Servers and Trust
----------------------

The concept of the [Domain Name System](https://en.wikipedia.org/wiki/Domain_Name_System) dates all the way back to its [ARPANET](https://en.wikipedia.org/wiki/ARPANET) days, when a single text file on each ARPANET node -- called HOSTS.TXT  -- contained the mapping of system names on the ARPANET to their numeric addresses. When you wrote this file yourself, it was easy to be sure it was correct. As the network grew, it became unrealistic to maintain both the central and local copies of this file. By the early 1980s efforts were underway to create a system to automate this process.

The first DNS name server (Berkeley Internet Name Domain Server, or [BIND](https://en.wikipedia.org/wiki/BIND)) was written in 1984 by a group of UC Berkeley students, based on [RFC 882](https://tools.ietf.org/html/rfc882) and [RFC 883](https://tools.ietf.org/html/rfc883). By 1987 the DNS standard had been revised a number of times, resulting in [RFC 1034](https://tools.ietf.org/html/rfc1034) and [RFC 1035](https://tools.ietf.org/html/rfc1035), which have largely remained unchanged since then.

![](https://hackaday.com/wp-content/uploads/2019/10/Domain_name_space.svg_.png-had2.png){sizes="(max-width: 800px) 100vw, 800px" srcset="https://hackaday.com/wp-content/uploads/2019/10/Domain_name_space.svg_.png-had2.png 1280w, https://hackaday.com/wp-content/uploads/2019/10/Domain_name_space.svg_.png-had2.png?resize=250,199 250w, https://hackaday.com/wp-content/uploads/2019/10/Domain_name_space.svg_.png-had2.png?resize=400,319 400w, https://hackaday.com/wp-content/uploads/2019/10/Domain_name_space.svg_.png-had2.png?resize=784,625 784w"}

The essential structure of DNS is that of a tree-like configuration, with its nodes and leaves subdivided into zones. The [DNS root zone](https://en.wikipedia.org/wiki/DNS_root_zone) is the top-level zone, which consists out of thirteen root server clusters, which form the authoritative DNS root servers. Any newly set up DNS server (e.g. at an ISP or at a company) will end up getting its DNS records from at least one of those servers.

Each further [DNS zone](https://en.wikipedia.org/wiki/DNS_zone) adds a further domain to the name system. Each country tends to manage its own domains, with special domains (like .org, .com) which aren't bound to any specific country managed by a separate entity. When resolving a domain name using DNS, this means starting with the domain name (e.g. .com), then the name (e.g. 'google') and finally any sub-domains. This can involve a few trips through DNS zones if the requested data has not been cached already.

DNSSEC: Adding Trust to DNS
---------------------------

Before we get around to encrypting DNS requests, it's important to be sure that the [DNS server we're talking to can be trusted](https://en.wikipedia.org/wiki/DNS_hijacking). The need for this became clear during the 1990s, culminating into the first workable [DNS Security Extensions](https://en.wikipedia.org/wiki/Domain_Name_System_Security_Extensions) (DNSSEC) standard ([RFC 2353](https://tools.ietf.org/html/rfc2535)) and the revised [RFC 4033](https://tools.ietf.org/html/rfc4033) (DNSSEC-bis).

![A map of the internet in 2006. (Opte project, CC BY 2.5)](https://hackaday.com/wp-content/uploads/2019/10/Internet_map_1024.png?w=250){sizes="(max-width: 250px) 100vw, 250px" srcset="https://hackaday.com/wp-content/uploads/2019/10/Internet_map_1024.png 1280w, https://hackaday.com/wp-content/uploads/2019/10/Internet_map_1024.png?resize=250,250 250w, https://hackaday.com/wp-content/uploads/2019/10/Internet_map_1024.png?resize=400,400 400w, https://hackaday.com/wp-content/uploads/2019/10/Internet_map_1024.png?resize=625,625 625w"}

[DNSSEC](https://www.dnssec.net/) works by signing the DNS lookup records with public-key cryptography. The authenticity of a DNS record can thus be verified by the public keys for the DNS root zone, which is the trusted third party in this scenario. Domain owners generate their own keys, which are signed by the zone operator and added to the DNS.

While DNSSEC allows one to be relatively certain that the responses one gets from the DNS resolver is genuine, it does require DNSSEC to be enabled in one's OS. Unfortunately few OSes implement a DNS service that is more than just 'DNSSEC-aware', meaning that they do not actually validate the DNS responses. This means that today one cannot be sure that the DNS responses one receives are genuine.

The Problems with DoH
---------------------

But let's imagine that you are using DNSSEC. You're now ready to encrypt the communication to add a level of privacy to the transaction. There are a number of motivations for keeping one's DNS queries secret from prying eyes. The more innocent reasons include dodging corporate and ISP filters, preventing tracking of one's internet habits and so on. More serious motivations include avoiding political persecution for [expressing one's views on the internet](https://en.wikipedia.org/wiki/Political_repression_of_cyber-dissidents). Naturally, encrypting one's DNS queries prevents people from snooping on those queries, but this ignores most larger security issues with DNS and of course every other communication protocol.

Here, the main contenders are DoT, using TLS, and the proposed DoH, using HTTPS. The most obvious difference between the two is the port they run on: DoT has a dedicated port, TCP 853, whereas DoH mixes in with other HTTPS traffic on port 443. This has the questionable benefit of DNS queries not being distinguishable at all, meaning that it removes options for network operators (private and corporate) to secure their own network, as one of the architects behind DNS, Paul Vixie, [pointed out on Twitter](https://twitter.com/paulvixie/status/1053765281917661184) last year.

The second main difference is that whereas DoT simply sends DNS queries over a TLS connection, DoH is essentially DNS-over-HTTP-over-TLS, resulting in its own mime Media Type of *application/dns-message* and significant added complexity. By mixing DoH in with existing protocols, it means that every DNS request and response goes through an HTTPS stack. For embedded applications this is a nightmare scenario, but it is also incompatible with nearly every piece of existing security hardware out there.

DoT has the other advantage that it's already implemented and has been in use for far longer than DoH, with many parties, including [Cloudflare](https://developers.cloudflare.com/1.1.1.1/dns-over-tls/), Google, some national ISPs and standard DNS server software like BIND supporting DoT out of the box. On Android Pie (version 9, for those keeping track) and later, DNS over TLS [will be used by default](https://www.techrepublic.com/article/how-to-enable-dns-over-tls-in-android-pie/) if the selected DNS resolver supports DoT.

Why switch up to DoH just as DoT is finally gaining traction? By having rogue apps like Firefox circumvent the system's DoT-based DNS and use its own DNS resolver over DoH instead, this makes for a highly opaque security situation. That DNS resolving would move into individual applications, as we see happening now, seems like a massive step backwards. Do you know which DNS resolver each application uses? If it mixes in with TCP port 443 traffic, how would you even know?

Encryption Doesn't Stop Tracking
--------------------------------

Two big parties behind DNS over HTTPS are Cloudflare and Mozilla, the latter of which has produced [this cutesy little cartoon](https://hacks.mozilla.org/2018/05/a-cartoon-intro-to-dns-over-https/) in which they try to explain DoH. Not unsurprisingly, in it they completely omit to mention DNSSEC (despite it being referenced as 'crucial' in RFC 8484), instead proposing something called Trusted Recursive Resolver (TRR), which seems to basically mean 'use a trustworthy DNS resolver', which for Mozilla means 'Cloudflare'.

Unrelated to DoH, they mention a standard called 'QNAME minimization' ([RFC 7816](https://tools.ietf.org/html/rfc7816)) which aims to reduce the amount of non-critical information the DNS resolver sends along to DNS, as covered by [this Verisign blog article](https://blog.verisign.com/security/minimum-disclosure-what-information-does-a-name-server-need-to-do-its-job/). As said, this standard has no bearing on DoH and would even work fine without any DNS encryption. Like DNSSEC it's a further evolution of the DNS standard that improves its security and privacy aspects.

The kicker is in the 'What isn't fixed by TRR with DoH?' section, however. As [pointed out by experts](https://www.zdnet.com/article/dns-over-https-causes-more-problems-than-it-solves-experts-say/) on many occasions, encrypting DNS doesn't prevent tracking. Any subsequent requests to the IP address that one so secretly resolved would still be visible clear as day. Everybody will still know that you're visiting Facebook.com, or that risky dissident website. No amount of DNS and internet traffic encryption will hide information that is crucial to the functioning of a network like the internet.

The Future Internet is a Single Point of Failure?
-------------------------------------------------

Mozilla's answer to the IP tracking problem is to essentially say that there is no problem, because of the [Cloud](https://en.wikipedia.org/wiki/Cloud_computing). As more and more websites and content distribution networks (CDNs) get lumped onto a handful of services (Cloudflare, Azure, AWS, etc.), the meaning of that single IP becomes less and less meaningful, you just have to trust whichever Cloud service you pick to not steal your data, or go down for a day.

This year, there was a massive downtime event on June 24, when a [configuration mistake at Verizon](https://blog.cloudflare.com/how-verizon-and-a-bgp-optimizer-knocked-large-parts-of-the-internet-offline-today/) led to Cloudflare , Amazon, Linode and many others being unavailable for much of the day. Then on July 2nd of this year Cloudflare as a whole [went down for about half an hour](https://www.theregister.co.uk/2019/07/02/cloudflare_down/), taking down with it many websites that rely on its services.

Coincidentally Microsoft's Cloud-hosted Office365 also had a multi-hour outage that same day, leaving many of its users stranded and unable to use the service. Meanwhile, on US Labor Day weekend, a power outage over at [AWS' US-East-1 data center](https://www.theregister.co.uk/2019/09/04/aws_power_outage_data_loss/) led to 1 TB of customer data vanishing as the hardware it was stored on went FUBAR. Clearly there are some issues to be ironed out with this 'centralizing the internet is good' message.

What's Old is New Again
-----------------------

It's in many ways astounding that in this whole discussion about privacy and tracking there's no mention of [Virtual Private Networks](https://en.wikipedia.org/wiki/Virtual_private_network) (VPN). These solve the issues of encrypting your data and DNS queries, of hiding your IP address and so much more by simply moving the point where your PC or other internet-enabled device 'exists' on the internet. VPNs have been very commonly used by dissidents in authoritarian regimes for decades to get around [internet censorship](https://en.wikipedia.org/wiki/Internet_censorship) and along with specialized forms such as the [Tor network](https://en.wikipedia.org/wiki/Tor_%28anonymity_network%29) are a crucial element in online freedom.![](https://hackaday.com/wp-content/uploads/2019/10/Tor-onion-network.png?w=250){sizes="(max-width: 250px) 100vw, 250px" srcset="https://hackaday.com/wp-content/uploads/2019/10/Tor-onion-network.png 510w, https://hackaday.com/wp-content/uploads/2019/10/Tor-onion-network.png?resize=250,160 250w, https://hackaday.com/wp-content/uploads/2019/10/Tor-onion-network.png?resize=400,256 400w"}

If one can trust a big commercial entity like Cloudflare in a scheme like DoH, then finding a trustworthy VPN provider who'll not store or sell your data should be just as easy. Even better, the Opera browser comes with a free, built-in proxy that [offers many benefits of VPN](https://proprivacy.com/review/operavpn).

In summary, one can state that DoH honors its acronym by poorly doing what DoT already does. More focus should be on getting DNSSEC fully implemented everywhere along with DoT and QNAME minimization. And if true privacy by dodging tracking is your goal, then you should be looking at VPNs, especially if you're a dissident trapped in some authoritarian regime.

![](http://feeds.feedburner.com/~r/hackaday/LgoM/~4/SbOH9toZUMY)

[Article Source](https://hackaday.com/2019/10/21/dns-over-https-is-the-wrong-partial-solution/).
{: .center-text}

