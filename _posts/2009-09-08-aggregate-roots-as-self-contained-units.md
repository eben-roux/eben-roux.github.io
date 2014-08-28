---
layout: post
title: "Aggregate Roots as self-contained units"
date: 2009-09-08 07:23:00
comments: true
categories:
- design
tags:
- domain driven design
- aggregate root
---

OK, I know Aggregate Roots (AR) are a consistency boundary but I believe that they need to do even more. As Eric Evans mentions in [DDD] relationships need to be scaled down to simple forms or even eliminated since it leads to simpler software. I agree.

I think we, as developers, have managed to paint ourselves into a corner over the years and since some of our techniques have been developed over a long period we just don't see it. It's like the guy sitting on his porch with his dog lying next to him. The dog is moaning and groaning. A neighbour walks by and enquires about the dog. So the chap answers: "Ther is a nail poking through the floorboard and the dog is lying on top of it.". So the neighbour asks: "Why doesn't the dog just move.". And the answer: "It doesn't hurt enough."

When philosophizing about some of the software issues that bug me I like to think about a time when there were no computers. How were these situations handled using manual systems? Let's take an Order (again). That piece of paper would contain absolutely all the information required about the order. There would be no customer defined but maybe a customer number. We used to have those 'traditional' primary and foreign keys in our databases. Problem was, whenever we **_changed** the key (for whatever reason) we needed to update the foreign keys, etc. So how would this work in a manual system? Probably a little note next to the number saying something like "new number C01-2009". So no data was actually _changed.

There proponents of CR as opposed to CRUD. So data is only ever added and read. However, the same idea can be achieved using CQRS. Although there will be updates the entire history will remain so it is essentially CR with a latest snapshot.

Over time, then, we got around to the idea of *surrogate keys*. But I think that this has lead to the higher coupling I mentioned in a <a href="http://www.ebenroux.co.za/post.aspx?id=195cc850-f763-4674-8fda-c3975a47abfc">previous post</a>. One will also often hear arguments along the line of requiring the customer object for discount information, etc. (Gold, Silver, Bronze clients). But I reckon that *that* information simply serves as defaults. Once the customer places an order and the customer is a gold customer at that point in time then the discount is assigned to the order and that's that. Shoud the customer be downgraded the next day the original data remains in tact.

The same can be said of the items being ordered. The actual products aren't *really* required in the object structure. The items can be loaded with the relevant description and bar codes and SKU codes and the like and if ever a lookup is required there should be more than enough information to retrieve whatever is required. Even database joins are possible.
