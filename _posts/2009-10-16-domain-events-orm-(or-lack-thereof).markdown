---
layout: post
title: "Domain-Events / ORM (or lack thereof)"
date: 2009-10-16 13:46:00
comments: true
categories: 
---

<p>Now I don't particularly fancy any ORM.&nbsp; I can, however, see the usefulness of these things but I still don't like them.&nbsp; One of the useful features is change-tracking.&nbsp; So as you fiddle with your domain objects so the ORM keeps track and will commit the changes to the data store when called to do so.</p>
<p>Now let's say I create an order with it's order lines.&nbsp; My use-case is such that I have to call <span style="font-family: courier new,courier;">Customer.AddOrder(order)</span>.&nbsp; That is OK I guess but what about now storing the changes?&nbsp; What were the changes?&nbsp; For each use case I need to have my repository be aware of what to do.&nbsp; Maybe my <span style="font-family: courier new,courier;">OrderRepository.Add(order)</span> is clever enough to save the changes --- well, it better be if <strong><em>I</em></strong> am the one responsible for making the code work.&nbsp; But what if adding the order changed some state in the Customer.&nbsp; Perhaps an <span style="font-family: courier new,courier;">ActiveOrdersTotal</span>?</p>
<p>This has bugged me for a while and made me consider an ORM on more than one occasion.&nbsp; I usually fetch a cup of coffee and by the time I get back to my desk the urge has left me.&nbsp; But today I read about Udi Dahan's domain events again when someone brought it up on the Domain-Driven Design group.&nbsp; The particular person was using it to perform persistence.&nbsp; It immediately made sense and I set about giving it a bash on some of my objects.&nbsp; <em>And I like it!</em></p>
<p>You can read Udi's post <a href="http://www.udidahan.com/2009/06/14/domain-events-salvation/">here</a>.</p>
<p>What I like about it is that I can have something like this in my service bus command handler:</p>
<p>[code:c#]</p>
<pre>using (var uow = UnitOfWorkFactory.Create())<br />{</pre>
<pre>&nbsp;&nbsp; var customer = CustomerRepository.Get(command.CustomerId);<br />   <br />   customer.ProcessCommand(command);<br />   <br />   uow.Commit();<br /></pre>
<pre>}<br /></pre>
<p>[/code]</p>
<p>The ProcessCommand could create the new order and add it to the internal active orders collection and also update the ActiveOrdersTotal.&nbsp; So far nothing is saved and the domain state has changed.&nbsp; Now the 'magic' happens.&nbsp; After the internal AddOrder is called an <span style="font-family: courier new,courier;">OrderAdded </span>domain event is raised along with a <span style="font-family: courier new,courier;">ActiveOrdersTotalChanged </span>domain event.&nbsp; The domain event handlers for these two events will then use the relevant repositories to persist the data.&nbsp; Since the events are raised in the unit of work and are, therefore, within the same transaction scope so the changes adhere to the ACID properties.</p>
<p>Now I have thought about a situation previously where one may need to send an e-mail.&nbsp; But what if the transaction fails and the e-mail has been sent.&nbsp; Udi did mention this in his post, though.&nbsp; The simple answer is to always have all operations be <em>transactional</em>.&nbsp; So rather than sending the mail directly using some SMTP gateway one would rather use a service bus and send a SendEMailMessage to the e-mail processing node using a transactional queue, for instance.&nbsp; In this way if the transaction fails the message will never be sent and the 'problem' is solved.</p>