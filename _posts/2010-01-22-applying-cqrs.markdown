---
layout: post
title: "Applying CQRS"
date: 2010-01-22 02:21:00
comments: true
categories: 
---

<p>Whenever a new technique comes to the fore it takes a while for it to settle in.&nbsp; Quite often one tries to apply it everywhere and that is when it seems as though it doesn't always fit.&nbsp; Of course, one should at least *try* to give it a go but there are instances where it may just not be the right tool for the job.</p>
<p>Command / Query Responsibily Segregation is one such an animal.&nbsp; It may not be applicable in every scenario.</p>
<p>Now don't get me wrong.&nbsp; CQRS really is the way to go but it works only when there is actually data to query.&nbsp; Probably seems obvious.&nbsp; So if the data you require is *not* there and you want to CQRS-it then you first need to create it.&nbsp; Let me present an example or two.</p>
<p>One may often hear a question along the lines of:</p>
<p style="padding-left: 30px;">"I need to present the total for the quote / order to the user.&nbsp; This total will incorporate tax and discounts and other bells and whistles.&nbsp; It seems as though I need to duplicate my domain behaviour on the front-end.&nbsp; How can I use CQRS to do this?"</p>
<p>More recently someone asked about repeating calendar events on the DDD group.&nbsp; Same thing.&nbsp; The data is not there.&nbsp; It cannot be queried.&nbsp; So CQRS does not work when domain interaction is required.&nbsp;</p>
<p>So how now?&nbsp; There are two options:</p>
<ol> </ol>
<ul>
<li>Request the domain to calculate the results, persist it, and then query it - CQRS</li>
</ul>
<ol> </ol>
<ul>
<li>Request the domain to calculate the results, and return it - no CQRS</li>
</ul>
<ol> </ol>
<p>It really is as simple as that.</p>
<p>So for situations where you do not need, or want, to persist the results get the domain to simply return the transient results and throw them away when you are done.&nbsp;</p>
<p>Therefore, it is not a case of CQRS Everywhere.</p>