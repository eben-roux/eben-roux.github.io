---
layout: post
title: "Silo, where are you!"
date: 2010-05-27 00:19:00
comments: true
categories: 
- opinion
tags:
- domain driven design
- bounded context
---

I just read [an interesting article](http://www.zdnet.com/blog/service-oriented/silos-for-lack-of-a-better-word-are-good/4909?tag=nl.e539).

The reason I say it is interesting is that silos have been denounced for quite some time now, and they should be. Yet, here is someone that appears to be a proponent thereof:

> "**Silos** are the only way to manage increasingly complex concepts"

I would paraphrase and say:

> "**Bounded Contexts** are the only way to manage increasingly complex concepts"

But bounded contexts do not solve complexity on their own. You definitely need a sprinkling of communication between them. A lack of communication is what leads to silos. Communication is a broad term but in this sense is simply means we need a publish / subscribe mechanism between the bounded contexts so that relevant parties are notified when a particularly interesting **event **takes place.

Without such communication systems duplicate data that does not directly relate to the core of that system. In this way systems get bloated, difficult to maintain, and complex to integrate with other systems. Eventually someone will come up with the _grand idea of a single, unified system to represent _everything in the organistion. Enter the _&uuml;ber silo. At this point we can say hello to a 3 to 5 year project and after the first 90% the second 90% can begin.

Don't do it!
