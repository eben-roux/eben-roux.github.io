---
layout: post
title: "NServiceBus V5 Configuration"
date: 2014-10-29 06:41:00
comments: true
categories:
- opinion
tags:
- nservicebus
---

Since I am active on [StackOverflow](http://www.stackoverflow.com) I came across this question: [How to reconfigure SQLTransport based NServicebus in Asp.net Web API?](http://stackoverflow.com/questions/26124882/how-to-reconfigure-sqltransport-based-nservicebus-in-asp-net-web-api)

The *answer* caught my attention.  It seems as though V5 of NServiceBus has been redesigned to make the configuration less static.  The reason I find it interesting is that [shuttle-esb](https://github.com/Shuttle/shuttle-esb) has been designed like this from the start.  Shuttle has *also* had a `Pipeline` concept from very early on and NServiceBus seem to be toying with the idea of a "chain of responsibility".

NServiceBus is what got me started on the Shuttle journey and Udi Dahan (creator of NServiceBus and CEO of Particular Software) has even contacted me about working for them.  I find it terribly exciting that a commercial product is mimicking what a similar free open-source project has been doing for quite some time :)