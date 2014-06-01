---
layout: post
title: "Natural Aggregates vs Synthetic Aggregates"
date: 2010-08-19 23:47:00
comments: true
categories: 
---

<p>In databases we have natural keys such as company code, client number, and order number.&nbsp; Then we have synthetic keys that are typically globally unique identifiers (GUID) or auto-incrementing numbers (IDENTITY).</p>
<p>It seams to me that we may need to make this same natural and synthetic distinction when it comes to aggregates in domain-driven design (DDD).&nbsp; The whole Aggregate Root (AR) concept makes it extremely difficult to define certain structures; especially when starting out with DDD.&nbsp; I find the reason being that some entities don't work well as aggregate roots either:</p>
<ol>
<li>because the aggregate root seems to change depending on context; or</li>
<li>the entity is not a natural fit for an aggregate root.</li>
</ol>
<p>An example of a <strong>natural </strong>aggregate is something like <strong>Order</strong> and <strong>OrderLine</strong>.&nbsp; An order line can never exist without an order.</p>
<ul>
<li>Order has a Total that is calculated by summing the OrderLine Totals.</li>
<li>Total on order cannot mean anything other than the total of the order lines.</li>
</ul>
<p>An example of a <strong>synthetic</strong> aggregate is something like <strong>GrapeBunch</strong> containing a <strong>GrapeStem</strong> and a <strong>GrapeBerry</strong> collection.</p>
<ul>
<li>GrapeBunch can have a Weight that is calculated by summing the Weight of each GrapeBerry in the collection and adding the Weight of the GrapeStem.</li>
<li>The weight of the grape bunch cannot mean anything other than the sum of the grape berry weights and adding the weight of the grape stem.</li>
</ul>
<p>An example of a <strong>problematic </strong>aggregate by using an aggregate root is <strong>GrapeStem </strong>with a <strong>GrapeBerry </strong>collection.</p>
<ul>
<li>What does the Weight on GrapeStem mean?</li>
<li>We could have another mothed called TotalWeight but this is confusing.</li>
</ul>
<p>So, although an aggregate root can be <em>made </em>to work it just does not feel very comfortable.</p>