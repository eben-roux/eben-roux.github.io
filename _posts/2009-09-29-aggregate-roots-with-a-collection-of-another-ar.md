---
layout: post
title: "Aggregate Roots with a collection of another AR"
date: 2009-09-29 23:43:00
comments: true
categories:
- design
tags:
- domain driven design
- aggregate root
---

I have been wondering whether an aggregate root should ever contain a collection of another aggregate root. Back to the Customer. Is it OK to have a collection of Orders? One could argue that a customer may have many thousands of orders after some time so it would be impractical. However, such a collection (in the domain) would mean that the customer only has a collection of the *open* orders. So in a real sense this list should never be all that big. Using CQS one may even remove the completed orders from the domain store.

Even so, what purpose would this collection serve. It seems, to me anyway, that the only time such a collection of ARs should be considered is when it falls within the *consistency boundary* of the containing AR. So, as per DDD, an aggregate root represents a consistency boundary. For example, an order with its contained order items needs to be consistent; and it seems rather obvious.

Then one comes across issues such as: "A *bronze* customer may not have more than 2 open orders at any one time and with a total of no more than $10,000". Similar rules apply to other customer types. A typical reaction is to have a reference to the customer in an order to get the type (bronze, etc.) and to be able to ask the OrderRepository for the total of all open orders for the customer. That is *one* way to do it, Iguess.

However, suddenly the *consistency boundary* has moved. It is now around the customer. This means that one would no longer add an order willy-nilly but rather add the order to the customer. So *now* we need an Orders collection for the customer. This does not mean that an Order is no longer an aggregate. This is also why some people reckon that the aggregate root changes depending on the use case. It is possible that when applying an 'apply order discount' task that the order stays the AR since it doesn't affect the overall collection of orders within the customer boundary. It also does not mean that we will not have an OrderRepository. But it does mean that when applying the CreateOrderCommand in our domain that the customer will be loaded, the newly created order will be added to the customer, the consistency checked and if OK the order will be persisted using the OrderRepository.
