---
title: Getting overly frustrated
date: 2021-12-31 10:18 -500
categories: [sql server,dba]
tags: [tempdb]
---

# Add files to tempdb

![tempdb files](/assets/images/tempdb/addFile-1024x309.png)

The number of files TempDb has will affect the performance of your SQL server. Often times, and prior to SQL 2016, adding files to TempDb is the proper course of action. In SQL 2016+, the SQL installer correctly adds the number of files to core ratio - at least up to 8. 

## Background

In SQL Server, TempDB is a system database used by the SQL engine. TempDb's primary function job is to store internal objects used by SQL like sorts, spools, and other temporary work. Think of it more a like a system scratch pad than a regular database. TempDb is not just for internal system use, it can also be used for scratch work by any database. query. or stored procedure. Another way to look at TempDb is that is SQL's version of swap space. When queries are too large to run in memory and they spill to disk, TempDb is where the queries will spill over to. One other use of TempDb is it will be used to hold version stores, which is a method to allow read queries to operate on tables without causing blocking.

## Number of Files needed for TempDb

Ideally, and per Microsoft guidance, TempDb should have one file for each processing core of the instance. When you license SQL per core, you have to pay for a minium of 4 cores, there should 4 files in TempDb. This one to one ratio holds true up to 8 cores, and then the ratio between numbers of cores to TempDb files gets murky, which is a separate topic.

To Add Files to TempDb


add pics in here


images are in images/tempdb



Tips
When you size the files, make sure to match the size of the existing files
If there isn't enough room on the disk, you may need to place the new files in a different location or shrink the existing files
Always use .NDF as the file extension for the new files
Make sure these files don't get backed by an OS backup. They have no value as TempDb gets emptied on SQL shutdown
I personally prefer to have TempDb data files on their own disk and presized to within 90% of the available disk space
I also prefer the TempDb log file on it's own disk
Prior to SQL 2016 

For SQL server 2005 - 2014, trace flags need to be added to the start up options to ensure symmetrical file growth and to use full file extents. The trace flags needed are 1117 and 1118. Brent Ozar has a great article on the trace flags.