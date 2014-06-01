---
layout: post
title: "Bite-Size Chunks"
date: 2010-11-04 14:03:00
comments: true
categories: 
---

<p class="h s-1">I came across this article on ZDNet: <a href="http://www.zdnet.com/blog/service-oriented/the-bigger-the-system-the-greater-the-chance-of-failure/6099?tag=mantle_skin;content"><em>"The bigger the system, the greater the chance of failure"</em></a></p>
<p>Now, I am all for "bite-size chunks" and Roger Sessions does make a lot of sense.&nbsp; However, simply breaking a system into smaller parts is only part of the solution.&nbsp; The greater problem is the degree of coupling within the system.&nbsp; A monolithic system is inevitably highly coupled.&nbsp; This makes the system fragile and complex.&nbsp; The fragility becomes apparent when a "simple" change in one part of the system has high ramifications in another part.&nbsp; The complexity stems from the number of states within the system that increases exponentially since they are all coupled.</p>
<p>If we simply break the system down into smaller components without removing the high coupling we end up with the exact same problems.&nbsp; This is typically what happens when folks implement <a href="http://agilitator.com/blog/?p=135">JBOWS</a>.</p>
<p>What is required is to break down the system into components and remove the coupling.&nbsp; One option is to make use of a service bus to facilitate sending messages between the components asynchronously.&nbsp; In this way the different components rely only on the message structures in the same way that an interface (as in class interface) abstracts the contract between classes.&nbsp; So in a way it is a form of dependency inversion since the components no longer depend on the concrete component implementation but rather on the message structures (the data contracts).</p>
<p>Coupling comes in two forms: <a href="http://iansrobinson.com/2009/04/27/temporal-and-behavioural-coupling/">temporal and behavioural</a>.&nbsp; Web services are a classic case of high temporal coupling.&nbsp; Since a service bus is asynchronous it results in low temporal coupling.&nbsp; Behavioural coupling is tricky either way.</p>
<p><img src="/image.axd?picture=2010%2f11%2fcoupling.png" alt="" /></p>