---
layout: post
title: "Windows 7 Enterprise 90-day Trial cannot rearm"
date: 2011-08-12 13:31:00
comments: true
categories: 
---

<p>I have been using the Windows 7 Enterprise 90-day Trial edition and as some may know one can 'rearm' the trial something like 3 times after it expires.</p>
<p>In order to do so you simply run the following from a command prompt with administrator rights (right click the <strong>cmd</strong> link and click on '<strong>Run as administrator</strong>'):</p>
<p style="padding-left: 30px;"><strong>slmgr -rearm</strong></p>
<p>Well.&nbsp; It didn't work.&nbsp; It wouldn't rearm.&nbsp; Some folks reckon after the trial expires you are shafted and some reckon you need to reboot a couple of times.&nbsp; Others go with a crack approach.&nbsp; Then I came across this:</p>
<p style="padding-left: 30px;"><strong>slmgr -ato</strong></p>
<p>And <em>that</em> did the trick.&nbsp; So you need to run both.&nbsp; The first seems to do the 'rearm' bit and the second the activation.</p>
<p>Man am I happy.&nbsp; I hope this helps someone else out there.</p>
<p>&nbsp;</p>