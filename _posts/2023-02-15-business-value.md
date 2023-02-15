---
title: Business Value?
date: 2023-02-15 11:00 -500
categories: [technology,databases]
tags: [development,rant,devops]
---

![rant ahead](/assets/images/blog-caution-rant-ahead.jpg)

# More DevOps

Since I attended [re:Invent](https://reinvent.awsevents.com/) in November, my interest in DevOps has deepened. So to that end, I've been trying to apply more of the concepts and ideals to my everyday work. 

In early 2022, I got together with the DevOps lead and started working on installing SQL with Puppet. I can't tell you how nice it is not to have go clicking though those damn install windows for the (X)-thousandth time. In fact, I'm going to go a bit further in the dev environments and doing more with puppet such as login and permissions for example. 

## Standardization

One of the other major pushes I worked on in 2022 was standardizing our SQL builds, I mean you kind of have to when Puppet does the install. This has made a huge difference when troubleshooting issues.

All this work has made me realize, not all teams are vested in this. Yesterday I received a ticket asking for a dev to prod restore. So I asked if there was a better alternative to do this, like setting up a pipeline. The response I got in return was mind blowing:

> "There is no business value in changing a process we do once a year"

## Kind of pissed me off

What do you mean there is no business value? Screw the business value, what about security? Best practices? Tracking changes? 

How about, you shouldn't be directly restoring dev databases to prod and call it "Development"! 

I don't know if it's laziness or just apathy. What I fear the reason is, is some Product Manager is calling the shots for the Developers, to which I sit here thinking why? They have the idea/request, developers figure out how to best implement it. Not Product dictating how this will be done. 

Ok, rant over. Back to my lane....