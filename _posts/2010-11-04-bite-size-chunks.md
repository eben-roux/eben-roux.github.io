---
layout: post
title: "Bite-Size Chunks"
date: 2010-11-04 14:03:00
comments: true
categories: 
- architecture
tags:
- component
---

I came across this article on ZDNet: [The bigger the system, the greater the chance of failure](http://www.zdnet.com/blog/service-oriented/the-bigger-the-system-the-greater-the-chance-of-failure/6099?tag=mantle_skin;content)

Now, I am all for "bite-size chunks" and Roger Sessions does make a lot of sense. However, simply breaking a system into smaller parts is only part of the solution. The greater problem is the degree of coupling within the system. A monolithic system is inevitably highly coupled. This makes the system fragile and complex. The fragility becomes apparent when a "simple" change in one part of the system has high ramifications in another part. The complexity stems from the number of states within the system that increases exponentially since they are all coupled.

If we simply break the system down into smaller components without removing the high coupling we end up with the exact same problems. This is typically what happens when folks implement [JBOWS](http://agilitator.com/blog/?p=135).

What is required is to break down the system into components and remove the coupling. One option is to make use of a service bus to facilitate sending messages between the components asynchronously. In this way the different components rely only on the message structures in the same way that an interface (as in class interface) abstracts the contract between classes. So in a way it is a form of dependency inversion since the components no longer depend on the concrete component implementation but rather on the message structures (the data contracts).

Coupling comes in two forms: [temporal and behavioural](http://iansrobinson.com/2009/04/27/temporal-and-behavioural-coupling/). Web services are a classic case of high temporal coupling. Since a service bus is asynchronous it results in low temporal coupling. Behavioural coupling is tricky either way.

![Component-Variations Image]({{ site.baseurl }}/assets/images/component-variations.png "Component-Variations")
