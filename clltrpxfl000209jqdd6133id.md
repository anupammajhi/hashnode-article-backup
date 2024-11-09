---
title: "POWERSHELL: Get Path of Currently Executing Script"
datePublished: Fri Jul 25 2014 05:53:34 GMT+0000 (Coordinated Universal Time)
cuid: clltrpxfl000209jqdd6133id
slug: powershell-get-path-of-currently-executing-script
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693088515244/411ae228-681d-4636-856e-817e94b2bac6.png
tags: command-line, windows, powershell, path, scripting

---

Another interesting scripting day for Powershell Kid and suddenly he is stuck at a simple problem. He can’t keep mentioning the full directory path every time explicitly in the script. Because, next minute if he moves the script to some other location, the script fails!! Let’s look into the problem he is facing.

Powershell Kid creates an awesome script that uses a few other files for input and output. So his script is saved at the location

`D:\My Test Scripts\The Crazy Script\CrazyPS.ps1`

and the script uses external files for input and output purposes, something like

```powershell
$inputcontent = Get-Content "D:\My Test Scripts\The Crazy Script\bin\input.txt" 
....
....
....

$output >> "D:\My Test Scripts\The Crazy Script\bin\output.txt"
```

Now the script executes marvelously, and the Powershell Kid goes the EUREKA!! mode.

Now he moves the script folder “The Crazy Script” to his favorite folder

```powershell
"C:\My Fav Scripts\The Crazy Script"
```

and executes it again from the new location. Disaster! It throws errors like flying dishes. What should he do now?

### **Solution**

Hey Powershell Kid, don’t use full directory paths, rather use relative paths. At the beginning of the script mention the following line:

```powershell
$ScriptPath = split-path $SCRIPT:MyInvocation.MyCommand.Path -parent
```

Now you can refer this variable as the relative path, like:

```powershell
$ScriptPath = split-path $SCRIPT:MyInvocation.MyCommand.Path -parent
 
$inputcontent = Get-Content "$ScriptPath\bin\input.txt"
....
....
....
....
 
$output >>  "$ScriptPath\bin\output.txt"
```

Have a fun day scripting!!