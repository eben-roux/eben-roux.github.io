---
layout: post
title: "Behavioural Coupling"
date: 2010-10-28 23:32:00
comments: true
categories: 
---

<p>When I left work yesterday I had to work through a turnstile.&nbsp; One of those big ones that only one person can go throught at any one time, in either direction.&nbsp; So it turns both ways.&nbsp; It is secured so I have to swipe my access card to get in or out.</p>
<p>This got me thinking about how that system works.&nbsp; Let's look at a highly coupled mechanism:</p>
<p>Since the turnstile allows either in or out the service the readers needs to know which reader does what.&nbsp; When a request arrives to allow someone access into the building our card reader service checks with the autorization system whether the person has access and if so sends a message to the turnstile to allow it to turn in the relevant direction.&nbsp; So here our card reader service needs to know how the turnstile works and which card reader is attached to which action.&nbsp; This is rather high behavioural coupling.</p>
<p>Now for something a bit less coupled.&nbsp; Let's say our turnstile simply 'unlocks' allowing the turnstile to turn once in any direction.&nbsp; Now the card reader service simply has to ask whether the person has access to unlock the turnstile and send the unlock message.&nbsp; So our card reader service now does not care how the turnstile works.&nbsp; This is low behavioural coupling.</p>
<p>Either way can be implemented but I'd say the first is somewhat more complex to get done.</p>