---
layout: post
title: "Domain-Events / ORM (or lack thereof)"
date: 2009-10-16 13:46:00
comments: true
categories: 
- design
tags:
- domain driven design
- domain event
- orm
---

Now I don't particularly fancy any ORM. I can, however, see the usefulness of these things but I still don't like them. One of the useful features is change-tracking. So as you fiddle with your domain objects so the ORM keeps track and will commit the changes to the data store when called to do so.

Now let's say I create an order with it's order lines. My use-case is such that I have to call `Customer.AddOrder(order)`. That is OK I guess but what about now storing the changes? What were the changes? For each use case I need to have my repository be aware of what to do. Maybe my `OrderRepository.Add(order)` is clever enough to save the changes --- well, it better be if **I** am the one responsible for making the code work. But what if adding the order changed some state in the `Customer`. Perhaps an `ActiveOrdersTotal`?

This has bugged me for a while and made me consider an ORM on more than one occasion. I usually fetch a cup of coffee and by the time I get back to my desk the urge has left me. But today I read about Udi Dahan's domain events again when someone brought it up on the Domain-Driven Design group. The particular person was using it to perform persistence. It immediately made sense and I set about giving it a bash on some of my objects. And I like it!

You can read Udi's post [here](http://www.udidahan.com/2009/06/14/domain-events-salvation/).

What I like about it is that I can have something like this in my service bus command handler:

``` c#
using (var uow = UnitOfWorkFactory.Create())
{
	var customer = CustomerRepository.Get(command.CustomerId);      
	customer.ProcessCommand(command);      
	uow.Commit();
}
```

The ProcessCommand could create the new order and add it to the internal active orders collection and also update the `ActiveOrdersTotal`. So far nothing is saved and the domain state has changed. Now the 'magic' happens. After the internal `AddOrder` is called an `OrderAdded` domain event is raised along with a `ActiveOrdersTotalChanged` domain event. The domain event handlers for these two events will then use the relevant repositories to persist the data. Since the events are raised in the unit of work and are, therefore, within the same transaction scope so the changes adhere to the ACID properties.

Now I have thought about a situation previously where one may need to send an e-mail. But what if the transaction fails and the e-mail has been sent. Udi did mention this in his post, though. The simple answer is to always have all operations be _transactional_. So rather than sending the mail directly using some SMTP gateway one would rather use a service bus and send a `SendEMailMessage` to the e-mail processing node using a transactional queue, for instance. In this way if the transaction fails the message will never be sent and the 'problem' is solved.
