---
title: Check for Corruption
date: 2017-07-16 11:16 -500
categories: [sql server, sql code]
tags: [tsql]
---

# Check multiple databases quickly

If you work or have worked, in a shop with lots of SQL servers then you either have, or will, experience an issue with storage. Like for instance, you're storage controllers go down, knocking down your SQL instances. As we all know, this is a good way to corrupt databases. That's why Sys Admins should never just hit the power button on a database server, but I digress.

I've recently encountered this problem, a couple of times in fact. While weekly DBCC checks are run, I was tasked with a way to check lots of databases for corruption quickly. It was decided that we would only do a physical check with DBCC, the databases should be under 1GB in size, and we wanted to be able to run the query across multiple servers quickly. Obviously, the size of the database can be modified.

I came up with this.

```SQL
--create table variable 
declare @dbcc_physical Table (
     name varchar(50) not null
);

-- select database names who are under 1GB in size, and store in table variable 
INSERT INTO @dbcc_physical
SELECT name from sys.master_files
WHERE type = 0 
AND size < 1024
AND name != 'modeldev';

-- create dynamic sql to execute dbcc check 
DECLARE @tableCursor CURSOR,
@databaseName VARCHAR(100); 

SET @tableCursor = CURSOR FOR SELECT * FROM @dbcc_physical; 

OPEN @tableCursor; 
FETCH NEXT FROM @tableCursor INTO @databaseName; 
WHILE(@@FETCH_STATUS = 0) 
BEGIN 
  EXECUTE ('DBCC CHECKDB(' + @databaseName + ') WITH PHYSICAL_ONLY;'); 
  
  FETCH NEXT FROM @tableCursor INTO @databaseName 
END; 
CLOSE @tableCursor; 
DEALLOCATE @tableCursor;
```