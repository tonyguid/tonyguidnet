---
layout: post
title: Using Azure Automation to Provision HDInsight and Hive
date: 2014-08-27
---
I've been spending a lot of time working with big data platforms and partners in this corner of the world lately. I'm planning on doing a post to try to break down the myriad of providers in the high-scale database area, which includes NoSQL, NewSQL, etc... 

But recently I gave a talk at [ThatConference](https://www.thatconference.com/) in the Wisconsin Dells on using HDInsight in combination with an Internet of Things scenario. [Brent Stineman](http://brentdacodemonkey.wordpress.com/) and [Jason Young](http://www.ytechie.com) collaborated with me, each building a different part of the overall scenario and turning int into a presentation and demo. Brent built the ingestion service using Event Hub, Jason built real-time analytics at scale using Orleans, and I built the analytics solution using HDInsight, Hive and Excel. Wow, more blog post ideas, they're really popping now!  

First thing I needed to do was provision an HDInsight instance. Sure, this is easy enough to do using the portal, but I wanted to be able to provision/deprovision as necessary in order to save on the number of cores being used in my subscription. So, I needed to automate the process so that I am not having to do this manually all the time in the portal. Azure Automation to the rescue!

First, I have to give credit to [Benjamin Guinebertiere](http://blogs.msdn.com/b/benjguin/) for the initial script. Benjamin also has other posts related to automation that are useful, so check out his blog.

<MORE TO COME>

