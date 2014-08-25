---
layout: post
title: "Temporal Data Coupling"
date: 2009-09-03 01:13:00
comments: true
categories:
- design
tags:
- coupling
---

I spent some time this past weekend thinking about how the OO development community has done its utmost to rid itself of high coupling within object models. It has, for the most part, been rather successful.

So I got to thinking how there appears to be a high level of coupling when it comes to the actual data being stored; so coupling within the database structure. If one considers that a customer may be invoiced then an Invoice has a direct relationship to a Customer; hence, the customer foreign key resides in the invoice. This tightly couples the data. It means that we now absolutely have to have a customer for an invoice. In a real sense it is probably true. Although, since we could capture an invoice as a cash sale (for whatever reason) we may not have a real customer. Be that as it may, we always need to store some form of a customer.

So where is the tight coupling? I think it turns out to be not so much within the structure but rather it has to do with the temporal nature of the data within the model. Add to this the fact that we have transactional data (Domain / Command) and 'analytical' data (Reporting / Query) then it becomes quite hectic.

### Object State vs. Object Type vs. Object Lifetime

The objects within our broader domain change as time goes by. Now these changes need to be handled correctly; it may be quite easy to use the _state pattern incorrectly. The archetypal sample of this pattern is the vending machine. As actions take place within the machine so the state transitions occur. The vending machine however, stays a vending machine.

Now how about an egg? Is it a chicken in another state? Let's say we want to track all our eggs. Each gets a number. Then one day egg number 1 'dies'. Do we care about the egg? How about the cost of getting the egg to the point it was before it 'died'? The next day egg number 2 hatches. Do we care about this egg? It no longer exists. We do care about chicken number 1, though. But after six weeks chicken number one becomes roast chicken number 20. The chicken is gone.

This is where CQS (Architecture) comes in. In our transaction store we could start with the two eggs and track all the required information and reporting data in the reporting store. Once egg number 1 dies we can remove it from the transaction store. All the historical data remains in the reporting store. However, the _state of that egg has probably changed to 'dead', though. The same goes for egg number 2. It's state changed to 'hatched'; but suddenly we now have a chicken added to our chicken repository. Once the chicken is roasted we can remove the chicken from our checking repository in the transactional store since all the audit data is still in the query store and the chicken is, well, history. But now we have an extra item added to the RoastedChickenRepository.

The same idea goes for a Quote leading to an Order leading to an Invoice leading a PaymentReceivedTransaction; each living and dying as it transitions along the path. So it has a lifetime in our transaction store. The reporting store, however, never forgets and is the keeper of all things good and bad that happened.

### Now the coupling

So let's say we have a 'LastQuote' on our Customer class. Since our domain model represents our transactional store, *that* reference is only valid while the quote is actually 'alive'. Once it becomes an Order or the quote is rejected we no longer have a 'LastQuote'. What do we do?

One option is to simply set LastQuote to null when the quote is deleted; if the last quote actually refers to the quote being deleted. Now this is where the <a title="Don't Delete - Just Don't" href="http://feedproxy.google.com/~r/UdiDahan-TheSoftwareSimplist/~3/UtJGgoADBzA/">Don't Delete - Just Don't</a> thinking probably does not apply since deleting from the domain store may just be OK. Maybe I'm off-track but I'm sure someone will let me know :)

Another option is not to model it like this in the first place but to rather use an application service to return whatever data we need from the last quote, from the query store. Something like QuoteService.LastQuoteDataForCustomer(123).
