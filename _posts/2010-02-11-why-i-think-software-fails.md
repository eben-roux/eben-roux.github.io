---
layout: post
title: "Why *I* think software fails"
date: 2010-02-11 22:56:00
comments: true
categories: 
---

Since the software industry has now been around for quite some time it is possible to look at the statistics around software failure. Although a great deal has been written and said about software development failure there is probably not too much in the line of anything that can be done about it. There should be.

Looking at the history of software development it is quite easy to blame the process or method used to manage the development. Software development is a broad discipline encompassing quite a number of spheres so singling out any particular cause is quite difficult. That is why there are so many lists specifying the reasons for software failure, e.g.:

<ul>
<li><a href="http://www.projectsmart.co.uk/why-software-projects-fail.html">Why Software Projects Fail and How to Make Them Succeed</a></li>
<li><a href="http://www.hks.harvard.edu/m-rcbg/ethiopia/Publications/Top%2010%20Reasons%20Why%20Systems%20Projects%20Fail.pdf">Top 10 Reasons Why Systems Projects Fail</a></li>
<li><a href="http://it.toolbox.com/blogs/madgreek/10-reasons-why-enterprise-initiatives-fail-24082">10 Reasons why Enterprise Initiatives fail</a></li>
<li><a href="http://www.computerworld.com/s/article/71209/Why_Projects_Fail">Five reasons why software projects fail</a></li>
</ul>
There are even good suggestions in there as to what can be done.

### Software fails because it sucks
Now that was simple. But why does it suck? _What makes it suck? I was watching a podcast by <a href="http://www.suckbusters.com/">David Platt</a> about his book on the subject and he said a very interesting thing:

<p style="padding-left: 30px;">"...that's using the computer to do what the computer is good at so the human can do what the human is good at."

This immediately stood out because it has been my experience that as soon as one tries to implement functionality in any system that is _flexible it becomes extremely _complex. Anything flexible is not only easy for humans to grasp and do, but also very obvious. Unfortunately, the geniuses in the chip-making industry have yet to come up with the '_obvious chip'. That is why one would, as a software developer, explain a particularly complex problem to someone (like a business analyst or a domain expert) and get a response of "So what's the problem?". They typically don't realize that the human brain is OK with fuzzy logic but that fuzzy logic cannot be represented very well by a simple algorithm.

### Defining complexity
When I think about software complexity <a href="http://www.objectwatch.com/">Roger Sessions</a> always comes to mind. From what I loosely recall he has mentioned that complexity in a system increases as the number of states increase and complexity between systems increases drastically as the number of connections between them increases. And then, of course, within a system there is also the coupling aspect in terms of class interaction.

So the following are the main culprits in the complexity game:

<ul>
<li>Heuristic level required</li>
<li>Number of states</li>
<li>Coupling (intra- and inter-system)</li>
</ul>
### How could we prevent the failures
I read <a href="http://www.tompeters.com/reimagine/">Re-imagine!</a> by <a href="http://www.tompeters.com/">Tom Peters</a> a few years back and if memory serves that is where I read:

<p style="padding-left: 30px;">"Fail faster, succeed sooner."

Sounds very agile to me. So the quicker we can prove that a specific technology, product, or development project is _not going to work, the better. Another approach would be to burn $10,000,000 of your client's money and keep on keeping on until your <a href="http://www.artima.com/weblogs/viewpost.jsp?thread=51769">momentum runs out</a>. At *that* point there will be anger, finger-pointing, tears, blows, and in all probably a good measure of litigation.

Granted, there are definitely systems that require heuristics and it  is a rather specialised area that may require expertise in areas that  the normal run-of-the-mill software developer simply does not have. It  is hard enough keeping abreast of changes in technology. Software  developers are not accountants, pharmacists, inventory managers,  logistics experts, statisticians, or mathematicians. Yet we find  ourselves in situations where we are simply given a vague direction to  go in and '_since we are so sharp' we'll just figure it out.  Maybe. Probably not. That is why a domain expert is so crucial to  software success. A business analyst would find themselves in the same  quandry as a programmer. They document things but may not have the  required extertise either (although some may). This is why management commitment is also listed quite often in the lists of reasons for software failure. Management needs to ensure that the domain experts are indeed available.

<h4>Technology to the rescue! Not.</h4>
Many companies are lead to believe that their problems are related to technology. Now sometimes the latest technology would help and sometimes old technology simply becomes obsolete. But more often than not it is a case of a vendor telling the client that they simply need to use a single platform along with all the merry products that are built there-on. But a quick visit to the common sense center of the brain will tell the client that it _may just be that they merge with another company in future and then there _may be different technologies at play or they _may purchase a product that relies on different technology altogether.

A homogenous technology environment will definitely not solve anything and is somewhat of a pipe dream in any but the simplest environments.

<h4>Products to the rescue!</h4>
OK, so we'll throw in a rules engine, workflow engine, CRM, CMS or any other COTS product and _voil&aacute;! We'll simply build on top of that. The thing is: each product satisfies a specific need. By the time most folks realise that there is no one ring to rule them all it has all gone south.

<h4>Heuristic level / Flexibility</h4>
Too many systems try to do too many things. I once worked on a system that was, essentially, a loan management system. It has been in development for over three years with a constant team of around twenty folks and I don't think it will see the light of day. One thing the client does in their daily life is to authorise loans or extensions on loans based on a motivation. Now this motivation document has traditionally been typed up in a word processor. In one instance a loan of say $10,000 needs to be authorised to a small company. Now, not much motivation required in this case since it is a small risk. Half a page did the trick. In another case a $20,000,000 loan needs to be scrutinized. Here a decent document is required that contains the company structure along with directors and lists of securities, and so on and so forth. This was put into the system along with all the input required to make this authorisation document work. It is total unnecessary to do so. Absolutely no value is gained by doing so. Just link to the original document or scan it in. Extreme complexity and cost was added for very little value.

<h4>Number of states</h4>
This part is actually slightly related to coupling anyway. One of the examples used by Roger Sessions is that of a coin. A coin has two sides: heads or tails. So there are two states that one needs to test for in your system. But how about two coins: heads-heads or heads-tails or tails-heads or tails-tails. OK, that was only four. But how about 3 coins. Well, that would be eight combinations since it is 2 ^ 3.

Now to be honest that wasn't too bad. But let's take a dice. Six sides there, so six states. Two dice would be 6 ^ 2 = 36 and 3 dice would be 6 ^ 3 = 216.

The only way to reduce this complexity is to reduce the states. At least, we need to reduce the coupling of the states so that, in our dice example, we have 3 independent dice so the number of states is 6 + 6 + 6 = 18 ( a *lot* less that 216) if we could find a way to separate the states from each other.

<h4>Coupling</h4>
I have blogged about coupling before. As software developers we need to identify different bounded contexts (as per Eric Evan's Domain-Driven Design) and implement them independently of each other and I have a sneaky suspicion that this is more-or-less what Roger Session does with his _simple iterative partitions. The problem here is: how?

Quite a few of the lists of reasons for software failure include skill level as a factor. However, even highly skilled developers may get this wrong. So the software will still fail. A highly coupled system or enterprise solution is very, very fragile.

### Service-Orientation and Tacit Knowledge
The key to decoupling bounded contexts or ending up with simple partitions is service-orientation. But it has to be done right. A service bus is all good-and-well but that other technology we need to talk to in our hetergenous environment is problematic. A possible solution to that interaction is bridging between the various service bus implementations so that the systems remain unaware of the other systems. Of course, most companies probably will not find themselves in such a position but it is a way to handle different technologies.

How would one actually go about stringing all these bits together? The answer is obviously not that simple since all the knowledge and experience required cannot really be measured. I think the reason for this is that it falls into the realm of <a href="http://en.wikipedia.org/wiki/Tacit_knowledge">tacit knowledge</a>.

The best thing to do for companies is to keep trying new ideas on a small or experimental projects and then to see whether something useful can be produced. Take the ideas developers and others have and give it a bash. The Big-Bang kind of approach has proven itself to be a mistake time-and-time again so growing something in an organic way may be best. There is also nothing wrong with 'shedding some pounds' as we move along.

### A last word on Tasks vs. Functions
Over the years I have found that we as developers tend to develop software that is a real pain to use since we are not the ones using it. Giving a user a bunch of loose ends does not help. As an example I would like to use my <a href="http://www.room2010.co.za">Room2010</a> site that I actually developed for myself and then had to tweak:

Anyone can register a free accommodation listing. At a later stage they may choose to upgrade to a premium listing. They can then send me up to 10 images that I will upload. So here is the problem: I had a function in my system where I could mark a listing as premium. I could then manage the images by browsing to the image locally on my disk and then uploading it. I had to do this for each image received. The only problem was that I received images of various sizes that all had to be scaled to 200 x 150. So before uploading I had to use a graphics package to change the sizes. Quite a tedious affair really.

So I created a small Windows application where I simply specify the listing number to process. The images I received would be stored in their original format in the relevant listing folder. Once I say _GO the system would talk to the website and set the listing to premium, the images would be resized, compressed and uploaded. As a last step the site would be instructed to send an e-mail to the owner of the listing notifying them of the changes.

So a whole host of functions were grouped together to accomplish a simple task.

Our software needs to be simple to use.
