---
title: Working with TempDb
date: 2016-05-04 11:00 -500
categories: [sql server]
tags: [sql server, tempdb]
---

# Tempdb

A while back I was working on setting up a new Availability Group (AG), which required working with tempdb. It was a bit of an advanced setup in the sense that it was a five node cluster, four nodes set up in a standard AG - two active/passive AGs. The fifth node was a participant in both AGs as a read replica. The logic behind the fifth node was it was being a read-only data warehouse environment, with the need of having all the databases in both AGs on one server. The fifth node was never to be made an active primary.

Eight tempdb files are the standard

One of the differences with the fifth node was that it had 12 CPUs vs 8 as with the other four nodes. So with that, the fifth node was now in the territory of needing more than the standard 8 tempdb files, 12 to be exact. After adding the four extra tempdb files, I noticed that after a few days the files weren't growing.

## Recommendations

One of the recommendations regarding tempdb files, pre-SQL2016, was to enable TF1118 to grow the tempdb files at the same rate. Otherwise, one or two files would become a hot spot - meaning only one or two of the tempdb files would be getting used.

To resolve the issue, I attempted to resize the other eight files to make room to grow the additional four. While trying to shrink one of the eight Tempdb files I received the following error:

```SQL
    DBCC SHRINKFILE: Page 8:16332696 could not be moved because it is a work table page
```
A little Googling turned up the cause of the error, occasionally TempdbDB files are stubborn and will not always shrink. One of the suggestions I found was to try to run [DBCC FREEPROCCACHE](https://www.brentozar.com/archive/2016/02/when-shrinking-tempdb-just-wont-shrink/).

However, that didn't quite work for all of the files. A little more Googling turned up similar conditions at [SQLSERVERCENTRAL.COM](https://www.sqlservercentral.com/forums/topic/shrink-fails-with-error-file-id-of-database-id-cannot-be-shrunk-as-it-is-either-being-shrunk-by-another-process-or-is-empty) and one of the suggestions was to bump the size of the file by a few MBs. Which ended up working for me.

Next time I have to shrink a tempdb file, I'll have a process to start with to unstick stubborn files.