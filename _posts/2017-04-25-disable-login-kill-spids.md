---
title: Disable login & Kill SPIDS
date: 2017-04-25 08:11 -500
categories: [sql server, sql code]
tags: [tsql]
---

# Kill the SPIDs

Recently came across a situation where reporting logins were interfering with nightly jobs due to blocking. After a number of attempts of trying to resolve the blocking, it was decided that a stored procedure that disabled the login and killed the user sessions was the most pragmatic solution. This is the code I came up with to resolve the issue.

On the other side, I scripted out a job to re-enable that could be called from a separate stored procedure at the conclusion of the job.

```SQL
SET NOCOUNT ON DECLARE @rowsToProcess INT; 
DECLARE @CurrentRow INT;
DECLARE @SelectSpid INT;
DECLARE @user VARCHAR(30);
DECLARE @killString varchar(50);
DECLARE @DisableLogin VARCHAR(100);

SET @user = ''; -- add user

--disable login of user to prevent more logins 
SET @DisableLogin = 'Alter Login ' + @user + ' Disable;';
EXEC(@DisableLogin);

--Create Table variable to store the spid info
Declare @BusinessObjectsTable Table 
  (
     rowid INT NOT NULL PRIMARY KEY IDENTITY(1,1), spid INT,
     loginname VARCHAR(30),
     spid_status VARCHAR(15)
  );

--query for the SPID based on user and store it in the table variable
INSERT INTO @ReportObjectsTable (spid, loginname, spid_status)
SELECT spid, loginame, status FROM sys.sysprocesses 
        WHERE loginame = @user;

-- set rows to process
SET @rowsToProcess = @@ROWCOUNT;

-- loop through SPIDs to kill 
SET @CurrentRow = 0;
WHILE @CurrentRow < @rowsToProcess
  BEGIN
    SET @CurrentRow = @CurrentRow + 1;
    SELECT @SelectSpid = spid FROM     @ReportObjectsTable 
       WHERE rowid = @CurrentRow 
       
    SET @killString = 'Kill ' + CONVERT(VARCHAR, @SelectSpid); 
    
    -- verify spid still exits and belongs to correct user before killing 
    IF EXISTS(SELECT spid, loginame FROM sys.sysprocesses 
        WHERE spid = @SelectSpid AND loginame = @user) 
    BEGIN
     PRINT @killString; -- verificatioin step I used to test against 
     EXEC (@killString)
    END;
    ELSE
     PRINT 'No spid to kill' 
     WAITFOR DELAY '00:00:05'; -- add delay if needed 
END;
```