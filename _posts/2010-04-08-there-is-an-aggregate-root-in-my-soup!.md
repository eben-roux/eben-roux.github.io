---
layout: post
title: "There is an aggregate root in my soup!"
date: 2010-04-08 01:01:00
comments: true
categories:
- domain driven design
tags:
- aggregate root
---

So there was yet [another discussion](http://tech.groups.yahoo.com/group/domaindrivendesign/message/17972) around value objects and repositories on the domain driven design yahoo group. In this case it revolves around a the definition of a **Country**.

Firstly, a repository should return an entity; never a value object. Well, so the definition goes. One may want to break that rule but it probably is not necessary. Now the definition of a value object vs. an entity has been rehashed to a point of boredom but another way to look at a value object is a value (albeit a composite value) that _never changes. The date '27 April' _never changes.

But getting back to a country: one would hear an argument along the lines of the name of a country being able to change. Actually, no. It doesn't. A _new country comes into being. The fact that Rhodesia became Zimbabwe does not mean that the country of Rhodesia no longer exists (as a value object that we care about). It may not be any use in the present or ever again in the future but it definitely did exist. The capital Salisbury was never in Zimbabwe; it was in Rhodesia.

It may seem a bit confusing when a value object appears to have an identity; or even when is does have an _identifier. The fact of the matter is that there are different codes that countries may be referred to. But the thing to note is that those codes do not change either.

Now if we make a country an entity suddenly 3 different databases can have 3 different IDs. This happens quite readily when using synthetic keys for entities and aggregate roots; even though they may have some stable, unchanging natural key. Something I'll discuss in another post.
