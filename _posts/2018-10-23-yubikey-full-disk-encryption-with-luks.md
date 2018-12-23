---
layout: post
title: Yubikey Full Disk Encryption with LUKS
date: 2018-10-23 20:07:03.000000000 -04:00
author: Michael J. Ford
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Bash tips
- CLI Fu
- Debian
- General Computing
- Linux
- Ubuntu
tags: []
meta:
  _edit_last: '1'
  _responsive_layout: default
  responsive_meta_box_designation: ''
  responsive_meta_box_facebook: ''
  responsive_meta_box_twitter: ''
  responsive_meta_box_googleplus: ''
  responsive_meta_box_text_linkedin: ''
  _oembed_efea5193051e44c4d56bc1fb3ab2e279: "{{unknown}}"
  _wpas_done_all: '1'
---
<p>So I finally got my self a yubikey, and what was the first thing I tried with it - luks full disk encryption. Yea.. better said than done. After a bit of research (read: failed attempts) I found this <a href="https://github.com/cornelinux/yubikey-luks">repo</a> on github, seemed promising. However it didn't work.. what gives.. after a bit of research into the repo I found a few pull requests that fix a couple of things. I went ahead and cloned the repo, made the changes, and what do you know it works. So I decided to fork the project and added some of the changes <a href="https://github.com/finlstrm/yubikey-luks">here</a> where I'll attempt to maintain it. See below the README.md.</p>
<p>I've tested this on Debian Sid, so your mileage may vary. If it breaks, you get to keep both halfs :).</p>
<p>Pull requests welcome!!!</p>
<p><!--more--></p>
<p><code><br />
Yubikey for LUKS<br />
================</code></p>
<p>This package is inspired and based on https://github.com/tfheen/ykfde.<br />
This package is forked from: https://github.com/cornelinux/yubikey-luks</p>
<p>This enables you to use the yubikey as 2FA for LUKS.<br />
The Password you enter is used as challenge for the yubikey</p>
<p>The keyscript allows to boot the machine with either<br />
the password and the Yubikey or with a normal password<br />
from any key slot.</p>
<p>luksSuspend/luksResume integration is inspired and based on https://github.com/zhongfu/ubuntu-luks-suspend</p>
<p>initialize Yubikey<br />
------------------</p>
<p>Initialize the Yubikey for challenge response in slot 2</p>
<p>ykpersonalize -2 -ochal-resp -ochal-hmac -ohmac-lt64 -oserial-api-visible</p>
<p>install package<br />
---------------</p>
<p>Build the package (without signing it):</p>
<p>make builddeb NO_SIGN=1</p>
<p>Install the package:</p>
<p>dpkg -i DEBUILD/yubikey-luks_0.*-1_all.deb</p>
<p>Assign a Yubikey to an LUKS slot<br />
--------------------------------</p>
<p>You can now assign the Yubikey to a slot using the tool</p>
<p>yubikey-luks-enroll</p>
<p>Technically this is done by writing the response to your password (1st factor<br />
knowledge) created by the Yubikey (2nd factor possession) to a key slot.</p>
<p>Admitted - If the attacker was able to phish this response which looks like<br />
this:<br />
bd438575f4e8df965c80363f8aa6fe1debbe9ea9<br />
it can be used as normal password.</p>
<p>If you set CONCATENATE=1 option in the file /etc/ykluks.cfg then both your password and Yubikey response will be bundled together and written to key slot: passwordbd438575f4e8df965c80363f8aa6fe1debbe9ea9</p>
<p>If you set HASH=1 option in the file /etc/ykluks.cfg then your password will be hashed with sha256 algorithm before using as challenge for yubikey: printf password | sha256sum | awk '{print $1}'<br />
5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8</p>
<p>Changing the welcome text<br />
-------------------------</p>
<p>If you want to change the welcome text a.k.a. the passphrase prompt you can edit<br />
the file /etc/ykluks.cfg.</p>
<p>After changing this file, you need to run</p>
<p>update-initramfs -u</p>
<p>so that the changes get transferred to the initramfs.</p>
<p>Enable yubikey-luks-suspend module<br />
------------------------------------</p>
<p>You can enable yubikey-luks-suspend module which allows for automatically locking encrypted LUKS containers and wiping keys from memory on suspend and unlocking them on resume by using luksSuspend, luksResume commands.</p>
<p>systemctl enable yubikey-luks-suspend.service</p>
<p>Open LUKS container protected with yubikey-luks<br />
------------------------------------</p>
<p>You can open LUKS container protected with yubikey-luks on running system</p>
<p>yubikey-luks-open</p>
<p>Manage several Yubikeys and Machines<br />
------------------------------------</p>
<p>It is possible to manage several Yubikeys and machines.<br />
You need to use privacyIDEA to manage the Yubikeys and<br />
the privacyIDEA admin client to push the Yubikey responses<br />
to the LUKS slots.</p>
<p>See https://github.com/privacyidea/privacyideaadm and<br />
https://github.com/privacyidea/privacyidea</p>
