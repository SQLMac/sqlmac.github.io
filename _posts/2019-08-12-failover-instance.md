---
title: Microsoft marketing is making a mess
date: 2019-08-12 11:22 -500
categories: [dba]
tags: [sql server]
---

# Confusion over MS Terms

Recently, I was working with a gentleman in our IT department and we were discussing adding redundancy to a couple of SQL Servers that were used for web apps. Turns out, VMware's disk consolidation can, and will, take a server offline. That tends to make customers unhappy, and rightly so. Anyway...I digress.

We were discussing adding redundancy to a couple of single-node environments and the gentleman kept referencing Always On, so naturally, I thought he was talking about Availability Groups. Another lesson here is to listen completely and not just for buzz words. So did he, in a fashion. He had seen what the licensing changes were with SQL2016 SP1, basic availability groups were an option on the standard license. I pointed out that basic AGs only allowed for one DB to be put into the AG per listener. In our case, that wouldn't work. The IT gentleman continued to push, pointing out that AlwaysOn Failover Cluster Instance would work under the Standard license with two nodes.

## Then it hit me

Microsoft's marketing team throws a monkey wrench in the terminology. When I looked up what [AlwaysOn Failover Cluster Instance](https://www.sqlservercentral.com/blogs/alwayson-availability-groups-vs-alwayson-failover-cluster-instances) really is, I realized it read a lot like traditional SQL clusters. You know, two nodes, shared storage, only node active at a time, with 30-90 second fail overtimes. That IS a traditional SQL cluster.

I don't know why MS decided to rename something that has been around since forever, but they did. It's probably more likely that I missed a press release or some other email. If that's the case then this post is probably redundant. However, if someone else gets caught unaware, hopefully, this comes up on Google to quickly help them.