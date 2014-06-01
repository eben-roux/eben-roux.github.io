---
layout: post
title: "There is an aggregate root in my soup!"
date: 2010-04-08 01:01:00
comments: true
categories: 
---

<p>So there is <a href="http://tech.groups.yahoo.com/group/domaindrivendesign/message/17972">another discussion</a> around value objects and repositories on the domain driven design yahoo group.&nbsp; In this case it revolves around a the definition of a <strong>Country</strong>.</p>
<p>Firstly, a repository should return an entity; never a value object.&nbsp; Well, so the definition goes.&nbsp; One may want to break that rule but it probably is not necessary.&nbsp; Now the definition of a value object vs. an entity has been rehashed to a point of boredom but another way to look at a value object is a value (albeit a composite value) that <em>never</em> changes.&nbsp; The date '27 April' <em>never</em> changes.</p>
<p>But getting back to a country: one would hear an argument along the lines of the name of a country being able to change.&nbsp; Actually, no.&nbsp; It doesn't.&nbsp; A <em>new</em> country comes into being.&nbsp; The fact that Rhodesia became Zimbabwe does not mean that the country of Rhodesia no longer exists (as a value object that we care about).&nbsp; It may not be any use in the present or ever again in the future but it definitely did exist.&nbsp; The capital Salisbury was never in Zimbabwe; it was in Rhodesia.</p>
<p>It may seem a bit confusing when a value object appears to have an identity; or even when is does have an <em>identifier</em>.&nbsp; The fact of the matter is that there are different codes that countries may be referred to.&nbsp; But the thing to note is that those codes do not change either.</p>
<p>Now if we make a country an entity suddenly 3 different databases can have 3 different IDs.&nbsp; This happens quite readily when using synthetic keys for entities and aggregate roots; even though they may have some stable, unchanging natural key.&nbsp; Something I'll discuss in another post.</p>