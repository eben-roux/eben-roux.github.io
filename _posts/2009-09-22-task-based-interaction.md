---
layout: post
title: "Domain Task-Based Interaction"
date: 2009-09-22 00:10:00
comments: true
categories: 
- design
tags:
- task based interaction
---

After some interesting to-and-froing on the <a href="http://tech.groups.yahoo.com/group/domaindrivendesign/">Domain-Driven Design group</a> the penny dropped on task-based interaction. Now this is something <a href="http://www.udidahan.com">Udi Dahan</a> seems to have been doing a while and it is actually *very* nifty. Now I must admit that I thought I actually understood task-based vs. entity based interaction but it turns out that even having something like 'Activate Account' is not necessarily task-based. Well, the UI would appear to be task-based to the user but how we implement this 'task' in the domain is where it gets interesting.

For our scenario we will use a disconnected environment as is the case with any real-world software solution.

### Entity-based implementation
Now the 'typical' way database interaction takes place, and the way I for one have been doing this, is as follows: The user invokes the 'Activate Account' function through some menu. The account is loaded from the table and all the account data is returned to the client. The user then enters, say, the activation date and clicks the 'Save' button.

Now all this data goes back to the server. An update is issued and all the account data is overwritten for the relevant row. "But wait!" come the cries. What if some other user has updated the banking details? No problem; we simply add an incrementing RowVersion on the record to indicate whether there is a concurrency violation. Now here is an interesting bit. Typically (well I used to do it like this), we simply *reload* the record and have the user do their action again. This time the last RowVersion will come along for the ride and hopefully no other user gets to submit their data before we hit the server. All is well.

### Task-based implementation
Using the task-based approach the user would invoke the 'Activate Account' function. The relevant data is retrieved from the database and displayed. Once again the user enters the activation date and clicks the 'Save' button. A command is issued containing the aggregate id and activation date and ends up in the handler for the command where a unit of work is started. The Account *aggregate root* is loaded and the command handed to the aggregate to process. Internally the status and activation date are set. So only the part that has been altered now needs to be saved. Should another user have updated the banking details it really doesn't matter since the two *tasks* do not interfere with one another.

### Applying policy rules
So why not just persist the command directly, bypassing the aggregate root?

In order to keep the aggregate valid the command is applied to the AR and it can then ensure that it is valid, or you can pass it through some validation mechanism. The act of applying the command may change the internal state to something that results in the validity being compromised. So applying the changes externally would be unwise.

### Persisting the result
Now the thing is that I don't currently fancy any ORM technology. An ORM, though, would do the change tracking for your AR and upon committing the unit of work the relevant changes would be persisted. However, when dealing directly with your own repository implementation I guess we'll require something like an ActivateAccount method on the repository to persist the relevant portions.

### Not all conflicts go away
Although focusing on tasks may achieve some manner of isolation (since business folks don't typically perform the same tasks on the same entities) it may still be possible that two tasks alter common data. In such cases a concurrency-checking scheme such as a version number may still be required. <a href="http://codebetter.com/blogs/gregyoung/">Greg Young</a> has mentioned something along the lines of saving the commands and then finding those that have been applied against the version number of the command currently being processed. So if CommandA completed against version 1 then the version number increased to 2. Now we also started CommandB against version 1 but how do we know whether we have a conflict? The answer is to find the commands issued against our aggregate with version 1 and ask each if the current command conflicts with it. Something like:

``` c#
var conflict = false;

previousCommands.ForEach(command => { conflict = thisCommand.ConflictsWith(command); }

if (!conflict)
{
	// do stuff
}
```
