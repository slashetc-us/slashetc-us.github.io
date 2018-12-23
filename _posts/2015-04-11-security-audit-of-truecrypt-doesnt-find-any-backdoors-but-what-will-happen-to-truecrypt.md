---
layout: post
title: Security Audit Of TrueCrypt Doesn't Find Any Backdoors -- But What Will Happen
  To TrueCrypt?
date: 2015-04-11 21:37:16.000000000 -04:00
author: Michael J. Ford
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories: []
tags: []
meta:
  _edit_last: '1'
  _wpas_done_all: '1'
  _responsive_layout: default
---
<p><i>The TL;DR is that based on this audit, Truecrypt appears to be a relatively well-designed piece of crypto software. The NCC audit found no evidence of deliberate backdoors, or any severe design flaws that will make the software insecure in most instances.</p>
<p>That doesn't mean Truecrypt is perfect. The auditors did find a few glitches and some incautious programming -- leading to a couple of issues that could, in the right circumstances, cause Truecrypt to give less assurance than we'd like it to.</p>
<p>For example: the most significant issue in the Truecrypt report is a finding related to the Windows version of Truecrypt's random number generator (RNG), which is responsible for generating the keys that encrypt Truecrypt volumes. This is an important piece of code, since a predictable RNG can spell disaster for the security of everything else in the system. </i></p>
<p>&nbsp;</p>
<p>Read the full story on <a href="https://www.techdirt.com/articles/20150403/17034930544/security-audit-truecrypt-doesnt-find-any-backdoors-what-will-happen-to-truecrypt.shtml" target="_blank">techDirt</a>.</p>
