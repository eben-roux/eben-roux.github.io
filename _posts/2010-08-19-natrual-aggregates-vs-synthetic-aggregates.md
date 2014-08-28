---
layout: post
title: "Natural Aggregates vs Synthetic Aggregates"
date: 2010-08-19 23:47:00
comments: true
categories: 
- design
tags:
- aggregate root
---

In databases we have natural keys such as company code, client number, and order number. Then we have synthetic keys that are typically globally unique identifiers (GUID) or auto-incrementing numbers (IDENTITY).

It seams to me that we may need to make this same natural and synthetic distinction when it comes to aggregates in domain-driven design (DDD). The whole Aggregate Root (AR) concept makes it extremely difficult to define certain structures; especially when starting out with DDD. I find the reason being that some entities don't work well as aggregate roots either:

- because the aggregate root seems to change depending on context; or
- the entity is not a natural fit for an aggregate root.

An example of a **natural **aggregate is something like `Order` and `OrderLine`. An order line can never exist without an order.

- Order has a Total that is calculated by summing the OrderLine Totals.
- Total on order cannot mean anything other than the total of the order lines.

An example of a **synthetic** aggregate is something like `GrapeBunch` containing a `GrapeStem` and a `GrapeBerry` collection.

- GrapeBunch can have a Weight that is calculated by summing the Weight of each GrapeBerry in the collection and adding the Weight of the GrapeStem.
- The weight of the grape bunch cannot mean anything other than the sum of the grape berry weights and adding the weight of the grape stem.

An example of a **problematic** aggregate by using an aggregate root is `GrapeStem` with a `GrapeBerry` collection.

- What does the `Weight` on GrapeStem mean?
- We could have another method called `TotalWeight` but this is confusing.

So, although an aggregate root can be *made* to work it just does not feel very comfortable.
