---
title: Windows Firewall
date: 2020-05-12 15:48 -500
categories: [dba]
tags: [windows,firewall,availability group]
---

# Damn the windows firewall

Automatic seeding is to be the lynchpin on PowerShell script I'm working on and will post about later. The idea will be to check for new databases added during the day, and then programmatically added them to the secondary later in the day. But that post is for another day.

After the upgrade, I performed a failover test, verified everything was working okay. After failing back over to the original primary, I added a database to the availability group via the wizard and chose automatic seeding, and while that was running I went off to do something else. When I got back, I saw the wizard had completed successfully and the database had been added. Just to make sure though, I checked the secondary and found no database. Say what?

After spending a day looking for failover cluster errors, testing failovers, adding databases to both nodes of the AG to test database creation, and assorted other tasks, I was still no closer to figuring out why the secondaries wouldn't sync up or why automatic seeding wasn't working. Keep in mind, all of this worked just fine when the AG was still running as a SQL 2014 environment. I mentioned this to one of our IT folks because I was at loss.

## The fix

One of our IT admins found the problem, although it did take them a couple of hours. Turns out, port 5022 was blocked on the secondary replica but why was it blocked when it worked before? I'll tell you why. When, and this very well could have been me when I built the server originally, the server was built and everything was loaded, a rule was created in the Windows Firewall for inbound connections. Only, instead of creating the rule to use the port number, the rule was created to allow access to the SSMS executable. Do you see what happened?

When I upgraded SQL to 2019, the 2019 executable took over running SQL. The path changed from 
```shell
<disk>:\<install location>\MSSQL12.MSSQLSERVER\blah blah blah
``` 
to 
```shell
<disk>:\<install location>\MSSQL15.MSSQLSERVER\blah blah blah
```

The fix was simple, we changed the firewall to allow connections on the AG port. I mean I guess we could have repointed the rule, but then if we ever upgraded that server again...I promise we would forget about this issue and had to repeat the troubleshooting work figuring it out again.

Be humble folks, every so often we manage to shoot ourselves in the foot.