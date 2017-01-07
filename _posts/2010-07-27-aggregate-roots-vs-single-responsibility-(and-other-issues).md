---
layout: post
title: "Aggregate Roots vs. Single Responsibility (and other issues)"
date: 2010-07-27 01:34:00
comments: true
categories: 
---

It is interesting to note how many questions there are around Aggregate Roots. Every-so-often someone will post a question on the [Domain-Driven Design Group](http://tech.groups.yahoo.com/group/domaindrivendesign/) regarding how to structure the Aggregate Roots. Now, I may be no genius but there are too many questions for my liking. It indicates that something is hard to understand; and when something is hard to understand it probably highlighting a larger problem.

### Aggregate vs. Aggregate Root

> "Cluster ENTITIES and VALUE OBJECTS into AGGREGATES and define boundaries around each. Choose one ENTITY to be the root of the AGGREGATE, and control all access to the objects inside the boundary through the root." (Evans, p. 129)

In one of Eric's presentations he touches on this; stating the difference between an Aggregate and an Aggregate Root. I have mentioned Eric's bunch of grapes example in a [previous post]({{ site.baseurl }}/domain-driven%20design/2009/11/24/ddd-!3d-ar/) so I will not re-hash it here.

The gang of four has two principles of object-oriented design:


- "Program to an interface, not an implementation." (Gamma *et. al.* p. 18)
- "Favor object composition over class inheritance." (Gamma *et. al.* p. 20)

So what's the point? At first glance it seems that we are working with composition for both the Aggregate and the Aggregate Root. On page 128 of Eric's blue book there is a class diagram modelling a car. The car class is adorned with two stereotypes: `<<Entity>>` and `<<Aggregate Root>>`.

#### Single Responsibility?
So is an Aggregate Root called Car sticking to the Single Responsibility Principle? It *is* responsible for Car behaviour; but also for the consistency within the Aggregate since it now represents an Aggregate with the Car entity as the Root. It seems as though the Car is doing more than it should and I think that this leads to many issues.

Could it be that the Car Aggregate Root concept is a specialisation of the Car entity? So, following this reasoning it is actually inheritance. The reason we do not see it is because it is flattened into the Car entity and, therefore, the car no longer adheres to SRP. My reasoning could be flawed so I am open to persuasion.

### Does the Aggregate Root change depending on the use case?

The problem the `Aggregate Root` concept is trying to solve is *consistency*.  It groups objects together to ensure that they are used as a unit.  When one looks at the philosophy behind [Data-Context-Interaction (DCI)](http://www.artima.com/articles/dci_vision.html) it appears as though the context is pushed into the root entity. When a different context (use-case) enters the fray that also makes use of the same `Aggregate Root` it appears as though the `Aggregate Root` is changing.

There *has* been some discussion around the issue of the `Aggregate Root` *changing* depending on the use case, i.e. a different entity is regarded as the root depending on the use case. Now some folks state that the `Aggregate Root` isn't really changing but the fact that it *appears* to be changing should be an indication that you are working with different **Bounded Context**. Now this is probably true; especially since the word **context** make an appearance.

#### A quick note on Bounded Contexts

Let's stay with the Car example. Let's say we have a car rental company and we have our own workshop. Now our car could do something along the lines of `Car.ScheduleMaintenance(_dependencies)` and `Car.MakeBooking(_dependencies)`.

This is where issues start creeping into the design. The maintenance management folks don't give two hoots about rentals and, likewise, the rental folks are not too interested in maintenance; they only want to know whether the car is available for rental. Enter the Bounded Context (**BC**). We have a **Maintenance Management BC** and **Rental Administration BC**. Of course we would probably also need a **Fleet Management BC** with e.g `Car.Commission()` and `Car.Decommission()`.

The particular Car is the same car in the real world. Just look at the registration number. However, the context within which it is used changes. It is, in OO terms, a specialization of a Car class to incorporate the context. This inevitably leads to data duplication of sorts since the data for each BC will probably be stored in different databases.

Assuming we view this as a problem, how could we solve this? As proposed by the GoF we could try composition. In DCI terms the context can aggregate the different entities. I previously blogged about an [aneamic use-case model]({{ site.baseurl }}/domain-driven%20design/2009/11/24/ddd-!3d-ar/) and the context looks an awful lot like a use-case. I have not played around with how to get these concepts into code but we'll get there.

### Repositories return Aggregate Roots?

Now this one is rather weird. I have no idea where this comes from. For those that have the Domain-Driven Design book by Eric Evans it is a simple case of opening the front cover and having a look at the diagram printed on the inside where it clearly shows two arrows that point to **Repositories**. One comes from **Entities** and the other from **Aggregates** (note: not Aggregate Root), like so:

- [Entities] --- *access* with --> [Repositories]
- [Aggregates] --- *access* with --> [Repositories]

The fact is that a repository **always** returns an entity whether or not that entity is an aggregate. So if folks want to call it an AR when there is no aggregation it probably will not restrict the design.
