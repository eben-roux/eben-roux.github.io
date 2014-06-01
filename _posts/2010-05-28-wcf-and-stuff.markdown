---
layout: post
title: "WCF issues"
date: 2010-05-28 03:42:00
comments: true
categories: 
---

<p>I installed a WCF client on a test machine this morning.&nbsp; Since it is using message encryption I installed the relevant certificate in the TrustedPeople store.</p>
<p>When the application started I received the following exception:</p>
<p style="padding-left: 30px;"><em><strong>X509InvalidUsageTime</strong></em></p>
<p>Turns out that the machines date was incorrect and fell outside of the certificate's valid period.&nbsp; A quick date change fixed that.</p>
<p>All was still not well as I swiftly moved on to the following exception:</p>
<p style="padding-left: 30px;"><em><strong>MessageSecurityException</strong></em></p>
<p>After some investigation and searching the Net it turned out that the time was way off too and that threw something out of joint.&nbsp; Setting the time to something more relevant sorted that.</p>
<p>So do make sure that you have your servers and workstations more-or-less synchronised.</p>