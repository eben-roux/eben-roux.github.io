---
layout: post
title: "Why *I* think software fails"
date: 2010-02-11 22:56:00
comments: true
categories:
- opinion
---

Since the software industry has now been around for quite some time it is possible to look at the statistics around software failure. Although a great deal has been written and said about software development failure there is probably not too much in the line of anything that can be done about it. There should be.

Looking at the history of software development it is quite easy to blame the process or method used to manage the development. Software development is a broad discipline encompassing quite a number of spheres so singling out any particular cause is quite difficult. That is why there are so many lists specifying the reasons for software failure, e.g.:


- [Why Software Projects Fail and How to Make Them Succeed](http://www.projectsmart.co.uk/why-software-projects-fail.html)
- [Top 10 Reasons Why Systems Projects Fail](http://www.hks.harvard.edu/m-rcbg/ethiopia/Publications/Top%2010%20Reasons%20Why%20Systems%20Projects%20Fail.pdf)
- [10 Reasons why Enterprise Initiatives fail](http://it.toolbox.com/blogs/madgreek/10-reasons-why-enterprise-initiatives-fail-24082)
- [Five reasons why software projects fail](http://www.computerworld.com/s/article/71209/Why_Projects_Fail)

There are even good suggestions in there as to what can be done.

### Software fails because it sucks

Now that was simple. But why does it suck? *What* makes it suck? I was watching a podcast by [David Platt](http://www.suckbusters.com/) about his book on the subject and he said a very interesting thing:

> ...that's using the computer to do what the computer is good at so the human can do what the human is good at.

This immediately stood out because it has been my experience that as soon as one tries to implement functionality in any system that is *flexible* it becomes extremely *complex*. Anything flexible is not only easy for humans to grasp and do, but also very obvious. Unfortunately, the geniuses in the chip-making industry have yet to come up with the '*obvious* chip'. That is why one would, as a software developer, explain a particularly complex problem to someone (like a business analyst or a domain expert) and get a response of "So what's the problem?". They typically don't realize that the human brain is OK with fuzzy logic but that fuzzy logic cannot be represented very well by a simple algorithm.

### Defining complexity

When I think about software complexity [Roger Sessions](http://www.objectwatch.com/)  comes to mind. From what I loosely recall he has mentioned that complexity in a system increases as the number of states increase and complexity between systems increases drastically as the number of connections between them increases. And then, of course, within a system there is also the coupling aspect in terms of class interaction.

So the following are the main culprits in the complexity game:


- Heuristic level required
- Number of states
- Coupling (intra- and inter-system)

### How could we prevent the failures
I read [Re-imagine!](http://www.tompeters.com/reimagine/) by <a href="http://www.tompeters.com/">Tom Peters</a> a few years back and if memory serves that is where I read:

> "Fail faster, succeed sooner."

Sounds very agile to me. So the quicker we can prove that a specific technology, product, or development project is *not* going to work, the better. Another approach would be to burn $10,000,000 of your client's money and keep on keeping on until your <a href="http://www.artima.com/weblogs/viewpost.jsp?thread=51769">momentum runs out</a>. At *that* point there will be anger, finger-pointing, tears, blows, and in all probably a good measure of litigation.

Granted, there are definitely systems that require heuristics and it  is a rather specialised area that may require expertise in areas that  the normal run-of-the-mill software developer simply does not have. It  is hard enough keeping abreast of changes in technology. Software  developers are not accountants, pharmacists, inventory managers,  logistics experts, statisticians, or mathematicians. Yet we find  ourselves in situations where we are simply given a vague direction to  go in and 'since we are so sharp' we'll just figure it out.  Maybe. Probably not. That is why a domain expert is so crucial to  software success. A business analyst would find themselves in the same  quandry as a programmer. They document things but may not have the  required extertise either (although some may). This is why management commitment is also listed quite often in the lists of reasons for software failure. Management needs to ensure that the domain experts are indeed available.

#### Technology to the rescue! Not.
Many companies are lead to believe that their problems are related to technology. Now sometimes the latest technology would help and sometimes old technology simply becomes obsolete. But more often than not it is a case of a vendor telling the client that they simply need to use a single platform along with all the merry products that are built there-on. But a quick visit to the common sense center of the brain will tell the client that it *may* just be that they merge with another company in future and then there *may* be different technologies at play or they *may* purchase a product that relies on different technology altogether.

A homogeneous technology environment will definitely not solve anything and is somewhat of a pipe dream in any but the simplest environments.

#### Products to the rescue!
OK, so we'll throw in a rules engine, workflow engine, CRM, CMS or any other COTS product and *voilá*! We'll simply build on top of that. The thing is: each product satisfies a specific need. By the time most folks realise that there is no one ring to rule them all it has all gone south.

#### Heuristic level / Flexibility
Too many systems try to do too many things. I once worked on a system that was, essentially, a loan management system. It has been in development for over three years with a constant team of around twenty folks and I don't think it will see the light of day. One thing the client does in their daily life is to authorise loans or extensions on loans based on a motivation. Now this motivation document has traditionally been typed up in a word processor. In one instance a loan of say $10,000 needs to be authorised to a small company. Now, not much motivation required in this case since it is a small risk. Half a page did the trick. In another case a $20,000,000 loan needs to be scrutinized. Here a decent document is required that contains the company structure along with directors and lists of securities, and so on and so forth. This was put into the system along with all the input required to make this authorisation document work. It is total unnecessary to do so. Absolutely no value is gained by doing so. Just link to the original document or scan it in. Extreme complexity and cost was added for very little value.

#### Number of states
This part is actually slightly related to coupling anyway. One of the examples used by Roger Sessions is that of a coin. A coin has two sides: heads or tails. So there are two states that one needs to test for in your system. But how about two coins: heads-heads or heads-tails or tails-heads or tails-tails. OK, that was only four. But how about 3 coins. Well, that would be eight combinations since it is 2 ^ 3.

Now to be honest that wasn't too bad. But let's take a dice. Six sides there, so six states. Two dice would be 6 ^ 2 = 36 and 3 dice would be 6 ^ 3 = 216.

The only way to reduce this complexity is to reduce the states. At least, we need to reduce the coupling of the states so that, in our dice example, we have 3 independent dice so the number of states is 6 + 6 + 6 = 18 ( a *lot* less that 216) if we could find a way to separate the states from each other.

#### Coupling
I have blogged about coupling before. As software developers we need to identify different bounded contexts (as per Eric Evan's Domain-Driven Design) and implement them independently of each other and I have a sneaky suspicion that this is more-or-less what Roger Session does with his *simple* iterative partitions. The problem here is: how?

Quite a few of the lists of reasons for software failure include skill level as a factor. However, even highly skilled developers may get this wrong. So the software will still fail. A highly coupled system or enterprise solution is very, very fragile.

### Service-Orientation and Tacit Knowledge
The key to decoupling bounded contexts or ending up with simple partitions is service-orientation. But it has to be done right. A service bus is all good-and-well but that other technology we need to talk to in our hetergenous environment is problematic. A possible solution to that interaction is bridging between the various service bus implementations so that the systems remain unaware of the other systems. Of course, most companies probably will not find themselves in such a position but it is a way to handle different technologies.

How would one actually go about stringing all these bits together? The answer is obviously not that simple since all the knowledge and experience required cannot really be measured. I think the reason for this is that it falls into the realm of [tacit knowledge](http://en.wikipedia.org/wiki/Tacit_knowledge).

The best thing to do for companies is to keep trying new ideas on a small or experimental projects and then to see whether something useful can be produced. Take the ideas developers and others have and give it a bash. The Big-Bang kind of approach has proven itself to be a mistake time-and-time again so growing something in an organic way may be best. There is also nothing wrong with 'shedding some pounds' as we move along.

Our software needs to be simple to use.
