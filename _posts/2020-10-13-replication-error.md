---
title: Error - SQL Replication - Remote Server does not exist...
date: 2020-10-13 13:44 -500
categories: [sql server, dba]
tags: [sql server, replication]
---

# Don't forget to check permissions

Here’s a quick post to remind myself and others, to check ALL of the permissions when working with replication. 

For the past couple of days, I’ve been working on getting transactional replication set up between a couple of servers in between other projects I’ve been working on. For the last day I kept running into the following error:

```shell
“The remoter server <“server name”> does not exist, or has not been designated as a valid Publisher, or you may not have permissions to see available Publishers. ” 
```

Make sure the account for the log_reader has dbo access to the distribution database. I missed that, given that the other system we use for replication is a designated Distribution server