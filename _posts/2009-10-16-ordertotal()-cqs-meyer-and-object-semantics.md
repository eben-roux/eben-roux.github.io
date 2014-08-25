---
layout: post
title: "Order.Total(), CQS [Meyer], and object semantics"
date: 2009-10-16 13:24:00
comments: true
categories: 
- design
tags:
- cqs
- object-orientation
- orm
---

Right, so I have seen some examples of determining an order total by adding the totals for the order lines, and then applying tax, etc. Firstly, however, we need to determine whether an order would actually ever do this. Why? Because an order represents an agreement of sorts and is a near-immutable object. One typically starts out with a quote (or shopping cart). This quote may have items that are on special. We may even apply a certain discount (based on our client segmentation or a discretionary discount) to the quote. There will probably never really be any rules around how many quotes a client may have or maximum amounts or the like.

Now once a client accepts a quote it becomes an order. The quote can, for all intents and purposes, disappear from the transaction store since it is now history. The order, on the other hand, is now an agreement that needs to be fulfilled. Some items may be placed on back-order or be removed totally if stock is no longer available. But the point is that the order now has it's own life cycle. It may even be cancelled in it's entirety. So why can we not 'edit' an order? The problem lies in determining the validity. Let's say that yesterday there was a special on lollipops and the client ended up ordering 100. Today he phones and says that he would rather like to order 200. What pricing would one use? The simplest would be to give the client a new quote and create a new order for the next 100 lollipops. The complete data used to quote the client is now available for this new order.

Now let's get to the `Order.Total()`. Now our order may still need to do this since we may place certain of the items on a `BackOrder` and need to recalculate the total based on the values stored in the remaining items. A typical scenario is to run through the order items and add up the totals. Now this, to me, seems to go against CQS [Meyer] since our query is changing the object state by changing the total. "But wait!", I hear you say, "The total is a calculated value so it is not state." This may be true when the order is the Aggregate Root for the use case, but what happens when we have a rule such as "A client may not have active orders totalling more than $5,000"? Now I know with an ORM the order items may be lazy-loaded. I do not use an ORM and lazy-loading borders on evil (IMO). So in the Customer class may have an `AddOrder` method for this use-case. But how does it get the total. Well, the total has to be stored as state.

### Changing the order total
OK, so we could have a method such as `CalculateTotal()` on the order. But _that_ would result in the order not being valid at all times since I could add an order line and call `Total()` before calling `CalculateTotal()`. So the answer is to perhaps call `CalculateTotal()` internally each time it needs to be called and to then keep the method private.
