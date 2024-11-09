---
title: "POWERSHELL: Get List Of Scheduled Tasks In Properly Formatted CSV"
datePublished: Thu Aug 27 2015 09:57:02 GMT+0000 (Coordinated Universal Time)
cuid: clltrmivu00060amiazg0da8c
slug: powershell-get-list-of-scheduled-tasks-in-properly-formatted-csv
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693088518577/debc656b-8eb7-458c-8b4d-1901fa0583c8.png
tags: command-line, windows, script, powershell, scheduled-task

---

Just when the Powershell Kid thought that CSV was a great way of getting outputs, CSV betrays him.

He realizes this when he tries to create a CSV file that lists all the Scheduled tasks on his computer. He runs this small command which is supposed to get him an output in CSV without any hustle, and it does get him an output in CSV file.

```powershell
schtasks /query /FO CSV > "C:\Users\PK\TaskSchedList.csv"
```

But wait!!. It opens in MS Excel fine, but everything in the file is in a “single” column. This is not how he wanted it. Well, of course, it has a way around, by doing some MS Excel Mumbo Jumbo, but he doesn’t like this way of doing it. So he tries this new hack. Powershell does have its ways to get things done.

```powershell
#Save this as a file named "TaskSchedList.ps1"
 
$Scriptpath = split-path $SCRIPT:MyInvocation.MyCommand.Path -parent
 
$tempfile = "$($env:temp)TempTaskSchedList.csv"
 
$outputFile = "$ScriptpathTaskSchedList.csv" schtasks /query /FO CSV > $tempfile
 
Import-csv -Path $tempfile | ?{
                               $_.Taskname -notmatch "TaskName"
                              } | Export-csv -Path $outputFile -NoTypeInformation
```

And great!! It does wonders!