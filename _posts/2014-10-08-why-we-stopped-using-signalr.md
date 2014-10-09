---
layout: post
title: "Why we stopped using SignalR"
date: 2014-10-08 06:22:00
comments: true
categories:
- opinion
tags:
- signalr
---

My current development team has been making use of SignalR for communication since I came on board 1 February 2013.  However, we have been replacing the communication infrastructure bit-by-bit as we ran into issues.

A while back we upgraded SignalR to version 2.1.1 and it just didn't seem to work.  We rolled back to version 2.0.3 to get things going again.  It then dawned on us that we had removed so much of SignalR that we *really* did not need it at all.  It was then removed after consultation with our product owners and the architecture folks.

Our main issues:

- we would require a backplane that would slow down communication with our front-end
- we had no control over the `JsonSerializer` instance that SignalR used internally and some of our messages failed since they had levels that were too deep
- the number of issues on the GitHub repository even though SignalR should be more mature by now
- issues being closed without a resolution

In case you are wondering: we are now using plain old polling :)
