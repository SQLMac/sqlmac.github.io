---
title: Postgres Failovers
date: 2018-05-24 14:30 -500
categories: [databases]
tags: [postgres]
---

# Introduction to Postgres Failovers

This week I got asked to help a developer figure out why a failover wasn't working on a Postgres cluster. Interesting enough I guess. Especially because I don't know anything about Postgres. Good time to learn I guess? I don't know. Anyway, I accepted the directive and started trying to get familiar with the new RDBMS system. The dev sent me a username and password to work with, so I got to work trying to figure out the issue.

## The Work

Starting off, was a failure in communications. I was sent a username and password, and naturally assumed this was as postgres user. So I loaded up PgAdmin and tried to go to work. Ya, no that wasn't happening. It turns out, the username and password given to me was for a Linux account. A Linux account without the ability to sudo to root (or anyone else). So after going in circles for the morning, I finally got my access straightened out.

Moving on, I setup a user for me in Postgres, modified the necessary conf files to give myself to login into Postgres with PgAdmin from my laptop. Sadly, between the Googling and continued back and forth over my access, this took the better part of the day. Once in though, it didn't take me to long to figure out the basic lay of the land. I guess when you get down to it, an RDBMS is a RDBMS is an RDBMS.

## Failover

This is where the process takes a turn. In SQL, when you think of failover replication with Availability Groups you know you can bounce the nodes pretty easily. You can make any member node of the AG the primary replica without much work. Makes sense. No so in Postgres/

In Postgres native replication, to failover you have to shut the primary node down and then manually (or through a trigger file....*still not sure about this) promote the standby server to master. Then, when you bring the previous master back online, it has to be brought up and configured as a standby server. At least, that is my understanding of the process so far. It seems a little jerky to me to have to jump through these kinds of hoops to have a failover configured. Then again, there is probably some framework or script that handles failovers better. I'm still working on understanding if this is actually how Postgres handles the failover.

Still learning....