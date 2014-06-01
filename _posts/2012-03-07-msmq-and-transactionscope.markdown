---
layout: post
title: "MSMQ and TransactionScope"
date: 2012-03-07 22:09:00
comments: true
categories: 
---

<p>It turns out that when using <strong>TransactionScope</strong> you should be creating the <strong>MessageQueue</strong> instance <em><strong>inside</strong></em> the TransactionScope:</p>
<p>[code:c#]</p>
<pre>&nbsp;&nbsp;&nbsp; using(var scope = new TransactionScope())</pre>
<pre>&nbsp;&nbsp;&nbsp; using(var queue = new MessageQueue(path, true))<br /></pre>
<pre>&nbsp;&nbsp;&nbsp; {</pre>
<pre>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // timeout should be higher for remote queues</pre>
<pre>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // transaction type should be None for non-transactional queues<br /></pre>
<pre>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; var message = queue.Receive(TimeSpan.FromMilliseconds(0), MessageQueueTransactionType.Automatic);</pre>
<pre>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // ...normal processing...</pre>
<pre>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; scope.Complete();<br /></pre>
<pre>&nbsp;&nbsp;&nbsp; }<br /></pre>
<p>[/code]</p>
<p>So MSMQ behaves the same way that a database connection does in <em><strong>that one has to</strong></em> create the database connection <em>inside</em> the scope.</p>