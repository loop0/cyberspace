---
title: "Owning my email server"
date: 2021-10-23T15:29:34-04:00
draft: false
---

A couple weeks ago I decided I should start de-googling and owning my own stuff on the internet, I thought it would be a good exercise to start by hosting my own email server. The attacks that fastmail (my current email provider for this domain) have been taking lately also contributed to the decision of hosting email on my own. 

Disclaimer: I've worked for many years in the past as a network/sysadmin and I've had to admin servers with thousands and thousands of email accounts.

After spending a couple of hours reading documentation and setting configurations (nothing much changed from 10 years ago) I came with the following stack to start:

* dovecot as an imap and lmtp server
* postfix as a smtp server authenticating via dovecot lmtp
* dns configuration for spf
* opendkim for DKIM signatures and verification

I've decided to not have a webmail, since I will always have a device capable of imap and smtp at hand. And I didn't want to have one extra application exposing my server to be exploited. The less attack surface the better.

The things that I still have to figure out are:

* antispam software
* backup solution

Am I missing something? Please let me know your suggestions for antispam and backup solutions, find me out on mastodon network `@loop0@fosstodon.org`