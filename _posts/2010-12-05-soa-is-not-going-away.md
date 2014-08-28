---
layout: post
title: "SOA is not going away"
date: 2010-12-05 04:00:00
comments: true
categories: 
- soa
---

Sometimes it is difficult to explain things that appear to be extremely simple. I guess this is where [tacit knowledge](http://en.wikipedia.org/wiki/Tacit_knowledge) comes into the picture. It may very well be that service orientation falls into this category.

Let's say that I wanted to pay a bill 50 years ago by sending my creditor a cheque by mail. I also expect a letter in reply stating that my cheque has been received. I firstly place my cheque, along with some relevant information, in an envelope and write the address of the creditor to send the mail to on the front. I could also write the return address on the back. I would need to add a stamp to the envelope to demonstrate payment of the requested service. Next I would place the letter envelope in a post box. If everything is in order I can reasonably expect the recipient to receive the letter. I can assume that the post office will provide the expected **_service**. Of course, this doesn't mean that the service will be provided as expected. There are a number of things that could go wrong but we'll hope for the best. So service orientation has been around for a while.

I, as the user of the mail system, see a very **coarse-grained** service, Within the post office there are many different services of varying granularity (some quite **fine-grained**) provided by different departments.

Now, although we expect a response from the creditor I am pretty sure that very few people will run off to their own post box and expect the reply to pop in; nor will they wait for the response. It is assumed that it will take a couple of days.

Let's jump back to the present and send an e-mail to a creditor. Once again, we expect a reply but we will not be watching our e-mail client application all day to see when it comes in. Again we expect some time to pass but hopefully not a couple of days. So our service will be completed **eventually**.

On the other hand some services are expected to be completed **immediately**. It depends on how consistent our information needs to be at any one time. For instance, I have a bank account with no overdraft facility. I draw my last ZAR100 from an ATM. Since my wife may draw money using the same account from another ATM I guess that bank would prefer that she not draw _"the last ZAR100" also. So the first transaction would block the second until it has been updated after which the second would fail since there would be insufficient funds. There *may* be no place for eventual consistency here. I say 'may' because the bank could decide to allow such behaviour and simply impose penalties.
