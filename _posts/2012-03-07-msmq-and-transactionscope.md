---
layout: post
title: "MSMQ and TransactionScope"
date: 2012-03-07 22:09:00
comments: true
categories:
- gotchas
tags:
- msmq 
---

It turns out that when using `TransactionScope` you should be creating the `MessageQueue` instance *inside* the `TransactionScope`:

``` c#
using(var scope = new TransactionScope())
using(var queue = new MessageQueue(path, true))
{
	// timeout should be higher for remote queues
	// transaction type should be None for non-transactional queues

	var message = queue.Receive(TimeSpan.FromMilliseconds(0), MessageQueueTransactionType.Automatic);

	// ...normal processing...

	scope.Complete();
}
```

So MSMQ behaves the same way that a database connection does in that one *has* to create the database connection *inside* the scope.
