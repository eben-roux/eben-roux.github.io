---
layout: post
title: "Silo, where are you!"
date: 2010-05-27 00:19:00
comments: true
categories: 
---

<p>I just read <a href="http://www.zdnet.com/blog/service-oriented/silos-for-lack-of-a-better-word-are-good/4909?tag=nl.e539">an interesting article</a>.</p>
<p>The reason I say it is interesting is that silos have been denounced for quite some time now, and they should be.&nbsp; Yet, here is someone that appears to be a proponent thereof:</p>
<p style="padding-left: 30px;"><em>"<strong>Silos </strong>are the only way to manage increasingly complex concepts"</em></p>
<p>Almost well said.&nbsp; I would paraphrase and say:</p>
<p style="padding-left: 30px;">"<strong><em>Bounded Contexts</em></strong> <em>are the only way to manage increasingly complex concepts"</em></p>
<p>But bounded contexts do not solve complexity on their own.&nbsp; You definitely need a sprinkling of communication between them.&nbsp; A lack of communication is what leads to silos.&nbsp; Communication is a broad term but in this sense is simply means we need a publish / subscribe mechanism between the bounded contexts so that relevant parties are notified when a particularly interesting <strong>event </strong>takes place.</p>
<p>Without such communication systems duplicate data that does not directly relate to the core of that system.&nbsp; In this way systems get bloated, difficult to maintain, and complex to integrate with other systems.&nbsp; Eventually someone will come up with the <em>grand</em> idea of a single, unified system to represent <em>everything</em> in the organistion.&nbsp; Enter the <em>&uuml;ber</em> silo.&nbsp; At this point we can say hello to a 3 to 5 year project and after the first 90% the second 90% can begin.</p>
<p>Don't do it!</p>