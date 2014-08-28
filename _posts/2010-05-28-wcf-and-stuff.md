---
layout: post
title: "WCF issues"
date: 2010-05-28 03:42:00
comments: true
categories:
- gotchas
tags:
- wcf
---

I installed a WCF client on a test machine this morning. Since it is using message encryption I installed the relevant certificate in the TrustedPeople store.

When the application started I received the following exception:

> **X509InvalidUsageTime**

Turns out that the machines date was incorrect and fell outside of the certificate's valid period. A quick date change fixed that.

All was still not well as I swiftly moved on to the following exception:

> **MessageSecurityException**

After some investigation and searching the Net it turned out that the time was way off too and that threw something out of joint. Setting the time to something more relevant sorted that.

So do make sure that you have your servers and workstations more-or-less synchronised.
