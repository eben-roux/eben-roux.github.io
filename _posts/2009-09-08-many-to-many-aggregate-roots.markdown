---
layout: post
title: "Many-to-Many Aggregate Roots"
date: 2009-09-08 22:53:00
comments: true
categories: 
---

<p>There has been some discussion around many-to-many relationships.&nbsp; Here is what Udi Dahan has to say: <a href="http://www.udidahan.com/2009/01/24/ddd-many-to-many-object-relational-mapping/">DDD &amp; Many to Many Object Relational Mapping</a></p>
<p>In his example Udi uses a Job and a JobBoard.&nbsp; As he states the typical modeling for this situation ends up with the Job having a collection of JobBoards and a JobBoard having a collection of Jobs.&nbsp; This is, as he states, somewhat problematic.&nbsp; He then reduces the relationship to a JobBoard having a collection of Jobs and a Job having no relation to JobBoards.&nbsp; This reduction of relationships is more-or-less what Eric Evans also states in his DDD book.</p>
<p>I would like to take another example: Order to Product.&nbsp; One order can contain many products and a Product can contain many orders.&nbsp; Yet we don't model it anywhere near an Order having a collection of products or a Product having a collection of orders.</p>
<h3>Relationships represented as collections</h3>
<p>In this example it may seem obvious since we have all been modeling Order objects for a hundred years.&nbsp; Everyone should have the idea of an 'extended' association table since we have extra data that needs to be carried in the form of quantity and price.&nbsp; So we end up with the OrderItem.&nbsp; Funny that it is not called OrderProduct, yet the OrderItem refers to a product.&nbsp; That is because there is a natural boundary around an order.&nbsp; For some reason, these boudaries are not quite that apparent when working with something like a Job and a JobBoard.</p>
<p>Or am I totally off course here?</p>