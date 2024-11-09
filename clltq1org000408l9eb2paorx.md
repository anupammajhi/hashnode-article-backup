---
title: "SCORCH: Useful Orchestrator SQL Queries"
datePublished: Mon Mar 26 2018 21:11:27 GMT+0000 (Coordinated Universal Time)
cuid: clltq1org000408l9eb2paorx
slug: scorch-useful-orchestrator-sql-queries
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693330162097/cf562f3c-ff30-4473-bcdd-4eaabf2b840d.png
tags: microsoft, sql, orchestrator, scorch

---

Microsoft’s System Center Orchestrator is quite a useful tool but with limited development and support from Microsoft. Sometimes it becomes quite tough to manage the tool without much ability to fetch information like list of running Runbooks or maybe simply to find the location of a runbook by name.

Since there are not many options or tools available in Orchestrator which could help us troubleshoot or analyze the Orchestrator environment, few SQL queries come in handy. It will also be useful if you are trying to generate some kind of report of the environment.

Here are some of those useful queries.

**Get Runbook (with date) and Folder Path**

This will list all the runbooks Orchestrator along with its location path (folder structure). This also gives you the ‘Last Modified’ date and time for each runbook, just in case it’s important for you to filter out older runbooks.

```sql
use orchestrator;
 
with RunbookPath as
(
select 'Policies\' + cast(name as varchar(max)) as [path], uniqueid from dbo.folders b
where b.ParentID='00000000-0000-0000-0000-000000000000' and disabled = 0 and deleted= 0
union all
select cast(c.[path] + '\' + cast(b.name as varchar(max)) as varchar(max)), b.uniqueid from dbo.FOLDERS b
inner join
RunbookPath c on b.ParentID = c.UniqueID
where b.Disabled = 0 and b.Deleted = 0
)
 
SELECT f.path AS RunbookPath,r.name AS RunbookName, r.LastModified AS 'Runbook Last Modified' from POLICIES as r 
INNER JOIN RunbookPath as f on r.ParentID = f.UniqueID
WHERE r.Deleted = 0
```

**Get Running Runbooks**

This query will help you fetch all the runbooks (along with path as previous) that’s running and has not yet completed. You may use it to understand if any runbook is running that actually should not be running. There is indeed a tool available to do this already, but it does not provide the path of runbook, in such a situation if we have multiple runbooks with same name, it becomes difficult to search.

```sql
use orchestrator;
 
with RunbookPath as
(
select 'Policies\' + cast(name as varchar(max)) as [path], uniqueid from dbo.folders b
where b.ParentID='00000000-0000-0000-0000-000000000000' and disabled = 0 and deleted= 0
union all
select cast(c.[path] + '\' + cast(b.name as varchar(max)) as varchar(max)), b.uniqueid from dbo.FOLDERS b
inner join
RunbookPath c on b.ParentID = c.UniqueID
where b.Disabled = 0 and b.Deleted = 0
)
 
select f.path AS RunbookPath,r.name AS RunbookName,A.[Computer] as RunbookServer from POLICIES as r 
inner join RunbookPath as f on r.ParentID = f.UniqueID
inner join [Microsoft.SystemCenter.Orchestrator.Runtime.Internal].[Jobs] as J on J.RunbookId = r.UniqueID
INNER JOIN [dbo].[ACTIONSERVERS] A ON J.RunbookServerId = A.UniqueID
WHERE r.Deleted = 0 AND
J.[StatusId] = 1
```

**Get Runbooks with ‘Common-Data Log’ and ‘Activity-Specific Log’ Enabled**

Ever wondered how to find which Runbooks have the ‘Common-Data Logging’ and ‘Activity-Specific Logging’ enabled under runbook properties?

The following query will help you find that out. Here **‘1’** means enabled/checked and **‘0’** means disabled/unchecked

```sql
use orchestrator;
 
with RunbookPath as
(
select 'Policies\' + cast(name as varchar(max)) as [path], uniqueid from dbo.folders b
where b.ParentID='00000000-0000-0000-0000-000000000000' and disabled = 0 and deleted= 0
union all
select cast(c.[path] + '\' + cast(b.name as varchar(max)) as varchar(max)), b.uniqueid from dbo.FOLDERS b
inner join
RunbookPath c on b.ParentID = c.UniqueID
where b.Disabled = 0 and b.Deleted = 0
)
 
select f.path AS RunbookPath,r.name AS RunbookName,r.LogCommonData, r.LogSpecificData from POLICIES as r 
inner join
RunbookPath as f on r.ParentID = f.UniqueID
where r.Deleted = 0 AND (r.LogCommonData = 1 OR r.LogSpecificData = 1)
```

**Get All ‘Powershell / C# /** [**VB.Net**](http://VB.Net)**’ Scripts**

This query will list down all the scripts that use the inbuilt activity ***‘Run .Net Script’***.

You may use generate a report of all the scripts. In my work, I have used this query (with small modification) to list down all scripts that use a particular password hard-coded, so that I could fix those to avoid security risk.

> *Note: This query might take a long time to execute depending on how many scripts you have in the Orchestrator.*

```sql
use orchestrator;
  
with RunbookPath as
(
select 'Policies\' + cast(name as varchar(max)) as [path], uniqueid from dbo.folders b
where b.ParentID='00000000-0000-0000-0000-000000000000' and disabled = 0 and deleted= 0
union all
select cast(c.[path] + '\' + cast(b.name as varchar(max)) as varchar(max)), b.uniqueid from dbo.FOLDERS b
inner join
RunbookPath c on b.ParentID = c.UniqueID
where b.Disabled = 0 and b.Deleted = 0
)
  
SELECT F.Path as RunbookPath,RB.Name AS RunBook,ACT.Name AS Activity,PS.ScriptType,PS.ScriptBody,PS.PublishedData,Act.Enabled as ActivityEnabled,RB.LastModified  FROM [Orchestrator].[dbo].[RUNDOTNETSCRIPT] AS PS
INNER JOIN [Orchestrator].[dbo].[OBJECTS] AS Act ON PS.UniqueID = Act.UniqueID
INNER JOIN [Orchestrator].[dbo].[POLICIES] AS RB ON RB.UniqueID = Act.ParentID 
INNER JOIN RunbookPath AS F ON F.UniqueID = RB.ParentID
where PS.Scriptbody is not Null
AND Act.Deleted = 0 AND RB.Deleted = 0
ORDER BY RB.LastModified,F.Path 
```

**Get All Activities Objects**

If you want to list down all the runbook activities, this query will be very useful.

> *Pro Tip: You can use the ‘ObjectType’ column to filter the activities by type of activity.*

```sql
use orchestrator;
  
with RunbookPath as
(
select 'Policies\' + cast(name as varchar(max)) as [path], uniqueid from dbo.folders b
where b.ParentID='00000000-0000-0000-0000-000000000000' and disabled = 0 and deleted= 0
union all
select cast(c.[path] + '\' + cast(b.name as varchar(max)) as varchar(max)), b.uniqueid from dbo.FOLDERS b
inner join
RunbookPath c on b.ParentID = c.UniqueID
where b.Disabled = 0 and b.Deleted = 0
)
 
 
/** 7A65BD17-9532-4D07-A6DA-E0F89FA0203E is the code for Link Objects, Remove the condition (AND Act.ObjectType <> '7A65BD17-9532-4D07-A6DA-E0F89FA0203E') if you want LINK objects too **/
 
SELECT F.Path,RB.Name AS RunBook,ACT.Name AS Activity, RB.LastModified AS 'Runbook Last Modified'  
FROM [Orchestrator].[dbo].[OBJECTS] AS Act
INNER JOIN [Orchestrator].[dbo].[POLICIES] AS RB ON RB.UniqueID = Act.ParentID 
INNER JOIN RunbookPath AS F ON F.UniqueID = RB.ParentID
where Act.Deleted = 0 AND rb.Deleted = 0 AND Act.ObjectType <> '7A65BD17-9532-4D07-A6DA-E0F89FA0203E'
ORDER BY F.Path, RB.Name , Act.Name
```

**Get Checked Out Runbooks**

Sometimes we check-out the runbooks to edit or maybe for some other reason and forget to check-in; Or maybe someone else has checked-out to work on it. And when we run the runbook it either fails or uses the older version of the runbook.

So, it becomes essential to find out if there is any runbook that has been checked out and who has checked it out. The following query might be useful for you in such scenarios.

```sql
use orchestrator;
  
with RunbookPath as
(
select 'Policies\' + cast(name as varchar(max)) as [path], uniqueid from dbo.folders b
where b.ParentID='00000000-0000-0000-0000-000000000000' and disabled = 0 and deleted= 0
union all
select cast(c.[path] + '\' + cast(b.name as varchar(max)) as varchar(max)), b.uniqueid from dbo.FOLDERS b
inner join
RunbookPath c on b.ParentID = c.UniqueID
where b.Disabled = 0 and b.Deleted = 0
)
 
 
/** 7A65BD17-9532-4D07-A6DA-E0F89FA0203E is the code for Link Objects, Remove the condition (AND Act.ObjectType <> '7A65BD17-9532-4D07-A6DA-E0F89FA0203E') if you want LINK objects too **/
 
SELECT F.Path,RB.Name AS RunBook,ACT.Name AS Activity, RB.LastModified AS 'Runbook Last Modified'  
FROM [Orchestrator].[dbo].[OBJECTS] AS Act
INNER JOIN [Orchestrator].[dbo].[POLICIES] AS RB ON RB.UniqueID = Act.ParentID 
INNER JOIN RunbookPath AS F ON F.UniqueID = RB.ParentID
where Act.Deleted = 0 AND rb.Deleted = 0 AND Act.ObjectType <> '7A65BD17-9532-4D07-A6DA-E0F89FA0203E'
ORDER BY F.Path, RB.Name , Act.Name
```

**Get SQL Query Passwords (Encrypted)**

This query will get the ‘Query Database’ activities along with the encrypted passwords that is used in the activity.

Note that the password is encrypted and has a specific 3-part (actually 4-part) format. I will leave that to you to analyze. You can use the queries coming-up later in this article to even de-crypt those passwords.

```sql
use orchestrator;
 
with RunbookPath as
(
select 'Policies\' + cast(name as varchar(max)) as [path], uniqueid from dbo.folders b
where b.ParentID='00000000-0000-0000-0000-000000000000' and disabled = 0 and deleted= 0
union all
select cast(c.[path] + '\' + cast(b.name as varchar(max)) as varchar(max)), b.uniqueid from dbo.FOLDERS b
inner join
RunbookPath c on b.ParentID = c.UniqueID
where b.Disabled = 0 and b.Deleted = 0
)
 
select f.path,r.name,Act.Name as Activity, s.password from POLICIES as r 
inner join RunbookPath as f on r.ParentID = f.UniqueID
INNER JOIN [Orchestrator].[dbo].[OBJECTS] AS Act ON Act.ParentID = r.UniqueID
INNER JOIN [Orchestrator].[dbo].[TASK_QUERYDATABASE] as S on s.UniqueID = Act.UniqueID
where r.Deleted = 0 AND Act.Deleted = 0
-- AND s.Password <> '\`d.T.~Ec/\`d.T.~De/\`d.T.~De/' /** Use this condition if you want filter-out the ones that use system password **/
```

**Decrypt Single Orchestrator Encrypted Value**

In case you come around an encrypted variable or value that you want to decrypt and find out, here is the query for that.

```sql
use orchestrator;
 
DECLARE @EncryptedPass nvarchar(255);
/** Set the value of @EncryptedPass as the encrypted value that you want to decrypt. Note that it is the Alphanumeric part of the Encrypted text. **/
SET @EncryptedPass = '00224EAFD0B0FC41DA40650F2F11F68801000000B60FCDF1618F1A37701C4B6D292DB444F1B1507B7EE82D5DAB8107CE1C54DD43E19DF7CC9AD6DDE6AFA0E4E834256955';
 
OPEN SYMMETRIC KEY ORCHESTRATOR_SYM_KEY DECRYPTION BY ASYMMETRIC KEY ORCHESTRATOR_ASYM_KEY;
 
with EncryptedPass AS
(
SELECT CONVERT(VARBINARY(MAX), @EncryptedPass , 2) as TheHexPass
)
 
SELECT convert(nvarchar, decryptbykey(TheHexPass)) AS Password FROM EncryptedPass;
```

**Decrypt All Stored SCO Variables**

It might happen that you or someone in your team has stored some variable under Orchestrator Variables as an encrypted variable. And for some reason you want to see the value stored in it now. Once it is encrypted, you cannot remove the check and see the original password. It just shows blank.

In such scenarios the following query might help you find out the decrypted values from the Orchestrator Variables.

```sql
use orchestrator;
 
OPEN SYMMETRIC KEY ORCHESTRATOR_SYM_KEY DECRYPTION BY ASYMMETRIC KEY ORCHESTRATOR_ASYM_KEY;
 
WITH TheVariables AS
(
SELECT Act.Name, 
        v.Value AS Encrypted, 
        CASE WHEN v.Value like '\`d.%' THEN
        convert(nvarchar, decryptbykey(CONVERT(VARBINARY(MAX), SUBSTRING(v.Value, CHARINDEX('/',v.Value) + 1, LEN(v.Value) - (2 * CHARINDEX('/',v.Value))) , 2))) 
        ELSE
        v.Value
        END AS Decrypted, Act.UniqueID
        FROM [Orchestrator].[dbo].[VARIABLES] v
INNER JOIN [Orchestrator].[dbo].[OBJECTS] AS Act ON Act.UniqueID = v.UniqueID
where Act.Deleted = 0
),
 RunbookPath as
(
select 'Variables\' + cast(name as varchar(max)) as [path], uniqueid from dbo.folders b
where b.ParentID='00000000-0000-0000-0000-000000000005' and disabled = 0 and deleted= 0
union all
select cast(c.[path] + '\' + cast(b.name as varchar(max)) as varchar(max)), b.uniqueid from dbo.FOLDERS b
inner join
RunbookPath c on b.ParentID = c.UniqueID
where b.Disabled = 0 and b.Deleted = 0
)
 
SELECT F.Path, V.Name as VariableName, V.Encrypted, V.Decrypted  
FROM [Orchestrator].[dbo].[OBJECTS] AS Act
INNER JOIN RunbookPath AS F ON F.UniqueID = Act.ParentID
INNER JOIN TheVariables AS V ON Act.UniqueID = V.UniqueID
where Act.Deleted = 0
ORDER BY F.Path, V.Name
```