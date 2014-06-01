---
layout: post
title: "Aggregate Roots vs. Single Responsibility (and other issues)"
date: 2010-07-27 01:34:00
comments: true
categories: 
---

<p>It is interesting to note how many questions there are around Aggregate Roots.&nbsp; Every-so-often someone will post a question on the <a href="http://tech.groups.yahoo.com/group/domaindrivendesign/">Domain-Driven Design Group</a> regarding how to structure the Aggregate Roots.&nbsp; Now, I may be no genius but there are too many questions for my liking.&nbsp; It indicates that something is hard to understand; and when something is hard to understand it probably highlighting a larger problem.</p>
<h3>Aggregate vs. Aggregate Root</h3>
<p style="padding-left: 30px;"><em>"Cluster ENTITIES and VALUE OBJECTS into AGGREGATES and define boundaries around each. &nbsp;Choose one ENTITY to be the root of the AGGREGATE, and control all access to the objects inside the boundary through the root."</em> (Evans, p. 129)</p>
<p>In one of Eric's presentations he touches on this; stating the difference between an Aggregate and an Aggregate Root.&nbsp; I have mentioned Eric's bunch of grapes example in a <a href="http://www.ebenroux.co.za/post/2009/11/24/DDD-!3d-AR.aspx">previous post</a> so I will not re-hash it here.</p>
<p>The gang of four has two principles of object-oriented design:</p>
<ol>
<li><em>"Program to an interface, not an implementation."</em> (Gamma <em>et. al.</em> p. 18)</li>
<li><em>"Favor object composition over class inheritance."</em> (Gamma <em>et. al.</em> p. 20)</li>
</ol>
<p>So what's the point?&nbsp; At first glance it seems that we are working with composition for both the Aggregate and the Aggregate Root.&nbsp; On page 128 of Eric's blue book there is a class diagram modelling a car.&nbsp; The car class is adorned with two stereotypes: &lt;&lt;Entity&gt;&gt; and &lt;&lt;Aggregate Root&gt;&gt;.</p>
<h4>Single Responsibility?</h4>
<p>So is an Aggregate Root called Car sticking to the Single Resonsibility Principle?&nbsp; It <em>is</em> responsible for Car behaviour; but also for the consistency within the Aggregate since it now represents an Aggregate with the Car entity as the Root.&nbsp; It seems as though the Car is doing more than it should and I think that this leads to many issues.</p>
<p>Could it be that the Car Aggregate Root concept is a specialisation of the Car entity?&nbsp; So, following this reasoning it is actually inheritance.&nbsp; The reason we do not see it is because it is flattened into the Car entity and, therefore, the car no longer adheres to SRP.&nbsp; My reasoning could be flawed so I am open to persuasion.</p>
<h3>Does the Aggregate Root change depending on the use case?</h3>
<p>The problem the Aggregate Root concept is trying to solve is <em>consistency</em>.  &nbsp;It groups objects together to ensure that they are used as a unit.  &nbsp;When one looks at the philosophy behind <a href="http://www.artima.com/articles/dci_vision.html">Data-Context-Interaction  (DCI)</a> it appears as though the context is pushed into the root  entity. &nbsp;When a different context (use-case) enters the fray that also makes use of the same Aggregate Root it appears as though the Aggregate Root is changing.</p>
<p>There <em>has</em> been some discussion around the issue of the Aggregate Root <em>changing</em> depending on the use case, i.e. a different entity is regarded as the root depending on the use case.&nbsp; Now some folks state that the Aggregate Root isn't really changing but the fact that it <em>appears</em> to be changing should be an indication that you are working with different <strong><em>Bounded Context</em></strong>.&nbsp; Now this is probably true; especially since the word <strong><em>context</em></strong> make an appearance.</p>
<p style="padding-left: 30px;">A quick note on Bounded Contexts:</p>
<p style="padding-left: 30px;">Let's stay with the Car example.&nbsp; Let's say we have a car rental company and we have our own workshop.&nbsp; Now our car could do something along the lines of <strong>Car.ScheduleMaintenance(<em>dependencies</em>)</strong> and <strong>Car.MakeBooking(<em>dependencies</em>)</strong>.</p>
<p style="padding-left: 30px;">This is where issues start creeping into the design.&nbsp; The maintenance management folks don't give two hoots about rentals and, likewise, the rental folks are not too interested in maintenance; they only want to know whether the car is available for rental.&nbsp; Enter the Bounded Context (<strong>BC</strong>).&nbsp; We have a <strong>Maintenance Management BC</strong> and <strong>Rental Administration BC</strong>.&nbsp; Of course we would probably also need a <strong>Fleet Management BC</strong> with e.g <strong>Car.Commission()</strong> and <strong>Car.Decommission()</strong>.</p>
<p style="padding-left: 30px;">The particular Car is the same car&nbsp;in the real world. &nbsp;Just look at the registration number. &nbsp;However, the context within which it is used changes. &nbsp;It is, in OO terms, a specialization of a Car class to incorporate the context. &nbsp;This inevitably leads to data duplication of sorts since the data for each BC will probably be stored in different databases.</p>
<p>Assuming we view this as a problem, how could we solve this? &nbsp;As proposed by the GoF we could try composition. &nbsp;In DCI terms the context can aggregate the different entities.&nbsp; I previously blogged about an <a href="http://www.ebenroux.co.za/post/2009/11/24/DDD-!3d-AR.aspx">aneamic use-case model</a> and the context looks an awful lot like a use-case.&nbsp; I have not played around with how to get these concepts into code but we'll get there.</p>
<h3>Repositories return Aggregate Roots?</h3>
<p>Now this one is rather weird.&nbsp; I have no idea where this comes from.&nbsp; For those that have the Domain-Driven Design book by Eric Evans it is a simple case of opening the front cover and having a look at the diagram printed on the inside where it clearly shows two arrows that point to <strong>Repositories</strong>.&nbsp; One comes from <strong>Entities</strong> and the other from <strong>Aggregates</strong> (note: not Aggregate Root), like so:</p>
<ul>
<li>[Entities] --- <em>access with</em> --&gt; [Repositories]</li>
<li>[Aggregates] --- <em>access with</em> --&gt; [Repositories]</li>
</ul>
<p>The fact is that a repository <strong><em>always</em></strong> returns an entity whether or not that entity is an aggregate. So if folks want to call it an AR when there is no aggregation it probably will not restrict the design.</p>