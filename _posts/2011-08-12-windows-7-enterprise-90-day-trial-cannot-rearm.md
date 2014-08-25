---
layout: post
title: "Windows 7 Enterprise 90-day Trial cannot rearm"
date: 2011-08-12 13:31:00
comments: true
categories: 
---

I have been using the Windows 7 Enterprise 90-day Trial edition and as some may know one can 'rearm' the trial something like 3 times after it expires.

In order to do so you simply run the following from a command prompt with administrator rights (right click the **cmd** link and click on '**Run as administrator**'):

> slmgr -rearm

Well. It didn't work. It wouldn't rearm. Some folks reckon after the trial expires you are shafted and some reckon you need to reboot a couple of times. Others go with a crack approach. Then I came across this:

> slmgr -ato

And _that did the trick. So you need to run both. The first seems to do the 'rearm' bit and the second the activation.

Man am I happy. I hope this helps someone else out there.


