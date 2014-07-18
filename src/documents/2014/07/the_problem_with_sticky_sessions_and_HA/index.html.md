---
layout: post
title: The Problem With Sticky Sessions and High Scale
date: 2014-07-18
---
I've had this discussion a few times, so my colleague [Marc Kuperstein](http://blogs.msdn.com/b/marckuperstein/) thought it might be a good idea if I wrote a blog post about it. Thanks for the encouragement Marc, here goes...

Let me just state my premise upfront:

> If an application requires any form of sticky sessions, it cannot achieve high scale OR high availability.

There. I said it, deal with it. I have heard so many different flavors of this question, but the answer always boils down to the simple fact that if you are routing traffic to a specific server, the risk of that server going down is the weak link preventing your app from being highly available or scaling to a massive level.

For me, this conversation usually comes in the context of someone migrating an existing application to Azure, but this concept is also true on any cloud platform, and even on-premises for that matter.

### A common scenario ###
There are many more, but let's stick to this one for now:

<i>Customer metadata is retained in-memory on a certain server. In order to process their request, all requests for that customer must be routed to this server in order to access the in-memory state.</i>

Under this context, let's break down the two areas of discussion, availability and scalability.

### High Availability ###

So, a typical architecture would use some form of sticky load balancer to analyze the header or look for a cookie, and send the request to a pre-determined server. 

So, what happens when that server goes down? (Remember - cloud hardware is commodity hardware!) That's right - for that customer, your application is no longer available! (cue dramatic music and images of angry customers, support calls, etc...)

Reboot, few minutes... restore metadata state, maybe a few minutes... (I mean, if it were quick, you wouldn't need to store it in-memory, right?)

I like to differentiate between two ways of thinking about availability. First, there's the SLA-based concept of being able to connect to the application as a definition of availability. This means you can run in a degraded mode, or it's available to some customers and not others, you're considered still highly available. As a legal SLA concept, you can certainly achieve high availability by this definition. However, are you really doing right by your customers?

Second is a more practical concept that high availability means that ALL of your customers are able to use their application. This is the one to which I strive when I architect applications. In order to achieve this, every piece of your architecture must be completely redundant

Coming back to our scenario - yes, you likely met your SLA, because your  app overall wasn't down. But if I'm that customer, I'm NOT very happy with you right now!

<B>Availability lesson:</B> Sticky-based routing causes a single point of failure. This is at odds with high availability.

### High Scalability ###

So, considering the scenario above, let's say you're a weather app who has decided to store data (i.e., graphics, forecast data, etc.) in memory to increase performance. You have decided to have 50 servers, each holding the data for a specific U.S. state.

What happens when there's severe weather, such as a tornado warning across 6 counties (which is common in the midwest)? EVERYONE goes to that server to get their information. Your server for that state becomes quickly overwhelmed and bogs down, while 49 other servers sit idly by. So much for scalability... 

<b>Scalability lesson:</b> Sticky-based routing can cause an unbalanced load to overwhelm a single server. This limits your scalability for that load to ONE server.

## Sticky Routing and/or in-memory state means scale and availability limitations, regardless of platform  ##
I often get asked "Does Azure offer anything like AWS Elastic IP?" My next question is always to ask about their scalability plans. If they indicate they plan to scale big, then I point out that they're asking the wrong question. The right question is "How do I get all of this state out of my app so I can scale?" 

Sometimes they choose the fastest path to migration by implementing some form of sticky routing, but at least they're now aware of the limitations.