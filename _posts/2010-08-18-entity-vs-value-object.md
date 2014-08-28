---
layout: post
title: "Entity vs. Value Object"
date: 2010-08-18 23:54:00
comments: true
categories: 
- domain-driven design
tags:
- aggregate root
- entity
---

This is another question that keeps popping up on Domain-Driven Design. It seems to be a difficult task and some of the answers seem to complicate matters since they come from a technical context.

### How do you use your object?

The same concept may very well be implemented as a value object in one context and as an entity in the next and depending on the type of object this distinction would be more, or less, pronounced. So it is important to understand how you *intend* using the object. If you are you interested in a *very* particular instance it is an entity and that's that. An object like an `Employee` or an `Invoice` can immediately be identified as an entity since we are all familiar with these concepts and 'know' that we need to work with specific instances. So we'll take something a bit more fluid like an `Address`.

Now when would an `Address` need to be an entity? Well, do we care about a specific instance? Is our application (in our Bounded Context) interested in a particular address?

### Example 1: Courier - Delivery Bounded Context

Let's imagine that we are couriers and when we receive a parcel we need to deliver it to a recipient at a particular address. Since we specialise in same-day business delivery we frequently deliver to office blocks that have the same street address but may house many of our clients. Here we care about a particular `Address` and we link recipients to it. The **Address** is an **Entity**.

### Example 2: Courier - HR Bounded Context
In our courier company we also have an HR system so we store **Employee** objects. Each employee has a home address stored as fields in the employee record in our database. However, in our object model we have an **Address** object represented by the **Employee._HomeAddress_** property (this is just for illustration so we won't split hairs as far as software design is concerned). In this case it seems quite obvious that **Address** has to be a value object since it is purely a container.

So let's say this same **Employee** object can have a list of ways to contact the employee and we model a **ContactMethod** class. In our data store we will have a one-to-many relationship between our **Employee** table and the **ContactMethod** table. In fact, we go so far as to give **ContactMethod** an **Id** so that we can directly update the data in the database (for whatever reason). **ContactMethod** would be aggregated with **Employee** so whenever we save the employee the contact methods are re-populated in the database (deleted and re-inserted). The ContactMethod is still a value object. Even though it may have its own life-cycle and identifier we do not care about a *specifc* instance in our application. We will never, and can never, say "*go and fetch me contact method... {what}*". So there is no way to uniquely identify a **Value Object** using the Ubiquitous Language for our domain even though it may have an identifier that is universally unique. It is simply a synthetic key used for technical efficiency.

### Immutable Value Objects
Some folks are of the opinion that value objects must be immutable. This is not necessarily so. As with our first address example an immutable value object would mean we need to create a new instance to make simple changes such as fixing a spelling mistake. It is perfectly acceptable to use immutable objects but there is also no reason why we can't change the properties of the same object instance. The only time that an immutable value object would be required is when the object instance is shared. But the only time you would share a value object in this way is when you are implementing the flywieght pattern and those cases are pretty rare.




