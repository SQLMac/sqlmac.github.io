---
title: Using GetDate()
date: 2018-03-06 09:57 -500
categories: [sql code]
tags: [tsql]
---

I have recently been working on tuning a job that purges data out of various tables that are older than 13 months using GetDate(). Pretty straight forward right? Well originally the job consisted of 6 different statements like so:

```sql
    delete from table1 where column_with_date <= DATEADD(mm,-13,GETDATE());
```

Except, the tables have become so large the generic deletes were filling up tempdb. No Bueno. So I decided to rewrite the job, using six separate stored procedures instead. While redundant, we could modify the behavior for each delete in the table independently should we ever need to. More importantly, we could batch out the deletes preventing tempdb from filing up.

So I went about creating a generic template to run a delete in batches. I think I based the code on a script on DBA Stack Exchange.

```sql
SET NOCOUNT ON;

DECLARE @rowcount INT;
SET @rowcount = 1;

WHILE @rowcount > 0
 BEGIN
    BEGIN TRANSACTION;
    
    DELETE TOP (500000) database.dbo.table(x)
        WHERE column <= DATEADD(mm,-13,GETDATE()) 
    OPTION (MAXDOP 1);
    
    SET @rowcount = @@ROWCOUNT;
    
    COMMIT TRANSACTION; 
END
```
One of the stored procedures began using parallelism and causing high CPU usage, so I added the MAXDOP query hint.  All of the tables have a timestamp column, so the GetDate() function works great to use as a filter. Except not all of the timestamp columns were formatted the same way.

All of the tables except one used the following format:

![format1](/assets/images/format1.png)

Whereas the one table used the following format:

![format2](/assets/images/format2.png)

The difference is the one table uses date and time, not just date. How is this important? Well, when the one job step executed on this table it got stuck in kind of a loop. The code was written to grab 500k records based on the date and TIME 13 months ago. So with every iteration, the time would change ever so slightly and the job would run forever (well, until midnight anyway).

Disclaimer, it took me about a week to finally realize what was happening. So to correct the behavior, I changed the formatting of the where clause so the query would get all of the records for the day instead of the slow cycle it was on.

```sql
SELECT * FROM table(x)
WHERE CONVERT(VARCHAR(10), column_timestamp,110) <= DATEADD(mm,-13,GETDATE));
```
I feel pretty dumb for not catching this sooner. Although, I doubt it ever takes me this long to catch it again.