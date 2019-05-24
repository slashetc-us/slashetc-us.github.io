---
layout: post
title: Scary Emails
date: 2019-03-31 13:00:22 00:00
author: Michael J. Ford
type: post
published: true
status: publish
---

Last night I recived an email that I'd like to talk a bit about. This email
arrived in my inbox on Sun, 31 Mar 2019 05:11:29 -0400 (EDT), with the title
'michael.ford'. On the surface, it's an email I sent to myself. Well that can't
be true as I was sleeping at the time. So at this point this email is very
concerning. Below is a screenshot taken from my email client of the inital
headers and the body of the email: 

![](/assets/image/scary-email-simple-header.png)
![](/assets/image/scary-email-body.png)

OK... so I may have had a slight heart attack when I first read this, but
before I dive in and share why this email is nothing and simply a drive by,
lets go over what 'facts' this guy is presenting:

1. Account has been infected
2. The email was sent 'proper'; *αs in, the email was sent from my email server*
3. Malware has been installed on all of my devices
4. Apparently I watch porn; *ahem, adult videos*
5. My screen has been recoreded while watching said videos with RDP
6. My webcam was turned on and recording while watching said videos
7. The malware on all of my devices contains a keylogger
8. The malware runs every 5 minutes to *capture* changed passwords
9. All of my devices have been backed up by the hacker
10. (S)he wants 1000 USD in bitcoin or all my *warez* will be shared with all
of my contacts in a short 2 days
11. There is a '*pixel*' in the email that will notify them when I have
read the email

Lets look over the evidence we have at hand and disprove all of these....

### 1. Account has been infected.

The first thing I did was go through all of my system logs on all servers and
workstations I've used lately, no evidence of account breach. Checked auth logs,
syslogs, email logs (this one gets interesting), etc. etc. Not one sign of breach.

- Fact Check False

### 2. The email was sent 'proper'

This guy is trying to make me think that my email account and/or server had been
compromised, and that the email originated from *MY* account/server. Well, we've
already proved in #1 there has been no breach. So let's take a look at the [email headers](/assets/text/scary-email.txt)\*.

    Return-Path: <luise@saomarcos-ma.ind.br>

THE FIRST LINE.... has the email address this 'scary email' was actually sent
from. So right away 'proper', is false. This guy simply changed the 'From:'
header to my email to make it look like it was sent from my account. The rest
of the headers dictates what server it actually originated from. So...

- Fact Check False

### 3. Malware has been installed on all of my devices

No evidence of malware has been installed on any of my devices. Nothing is
running that shouldn't be. No erroneous entrys in crontabs. No system is
acting werid. 

- Fact Check False

### 4. Apparently I watch porn; *ahem, adult videos*

- Fact Check False; you're just going to have to take my word for it... 

### 5. My screen has been recoreded while watching said videos with RDP

I run Linux.... Soooooo ....... RDP isn't really a thing. No TeamViewer, or
other screensharing.

Also, No malware was found. So how is this happening anyway.

- Fact Check False

### 6. My webcam was turned on and recording while watching said videos

Yes, I have a webcam. But on my desktop it is physically disconnected while not
in use, and tape over the lense on laptops. No recording here...

Also, No malware was found. So how is this happening anyway.

- Fact Check False

### 7. The malware on all of my devices contains a keylogger

See #3, No evidence of malware

- Fact Check False

### 8. The malware runs every 5 minutes to *capture* changed passwords

See #3, No evidence of malware

- Fact Check False

### 9. All of my devices have been backed up by the hacker

What a good guy. If I need anything I'll ask them for my files back. But I didn't
see any evidence of 3 or 4TB leaving my network...

- Fact Check False

### 10. (S)he wants 1000 USD in bitcoin or all my *warez* will be shared with all of my contacts in a short 2 days

Well, I can't find any way they can prove any of the above, so... 

- Fact Check Probably False

### 11. There is a '*pixel*' in the email that will notify them when I have read the email

This one made me laugh a little, a pixel by itself in an image is not able to be
used as a tracking point - it must be the whole image. But wait, the entire email
body is an image, so I took a look at the base64 encoded message. Nothing, it's just
a simple JPG. For this to have actually worked, the attacker probably should have
sent the email in HTML with an embedded 1x1 image used for tracking. HTML would
have also made it cheap to use my name/email in the body of the message. However
an HTML message would have blocked the image load.

- Fact Check False

So we just proved everything false... At this point, we can say with confidence that
this email was simply a drive-by, and nothing to worry about. But I dug a little deeper.

## Analysis

Fact #1 and #2, is what really got me going. Was someone really able to breach my
security, take over my systems, and steal my data. So glad that turned out not to
be true. As if it were, іt would have been the result of a very determined hacker,
I don't think I'm *that* interesting.

Digging a little deeper I decided to lookup the .br TLD - it's Brazil. I also looked
up the IP location of the email server this email came from, also Brazil. There is
a webserver running on the email server hosting Zimbra, but they have a self-signed
cert - at least it changes every year. The User-Agent used to send this email was
`Workspace Webmail 5.8.2`, but I was unable to verify if that is Zimbra's user agent
or not. I also found the website belonging to the domain, it looks like a school -
and no SSL.

So - IF the website and email server are legit, then I can only assume that the server
or a single user (probably used the same password everywhere) has been compromised.

Nice Try...

## Final Thoughts

Security is important. If you're the type to think you are not a target, you have
nothing to hide, or you'll probably never get attacked - think again. We are all
targets, and we all have something to protect. This person attempted and failed
to extort money out of me under false accusations. The email was probably scripted
using my email address which as been leaked in database breaches. However the sad
part of the story is for every one person like me, who will take the time to look
through every detail, there are probably 10s or 100s (if not 1000s/10000s etc.)
of people that will figure out how to give 1000USD in bitcoin - even knowning
that at least some of the message is false.

Here's the thing about emails. Spammers at one time would query an email server
hundreds if not millions of times looking for valid email addresses. Once the
valid emails are found, they are then used for spam campaigns. The problem with
that method is it's extremely noisy - the server operator will notice. Today
they have pivioted to using breached databases. Even just an email is worth
something because the provider, be it a website/service/etc., has typiclly
already verified the address, and they (the spammers) now have a vetted list
of *real* email address that can be used straight away for a laundry list of
malicious activitys.

\* _Full email body text has been provided to look though. It has only been changed
so that bots don't pickup my email_

*Edit 201905024* TLDR; found this nice 30 second video from a Sophos Naked Security Article: [Sextortion mail from yourself? It doesn’t mean you’ve been hacked](https://nakedsecurity.sophos.com/2019/05/09/sextortion-mail-from-yourself-it-doesnt-mean-youve-been-hacked/).

<iframe width="854" height="480" src="https://www.youtube.com/embed/aEmhW0t6vzI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
