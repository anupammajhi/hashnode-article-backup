---
title: "WINDOWS: BATCH files – What, Why and How?"
datePublished: Sat Nov 02 2013 11:41:21 GMT+0000 (Coordinated Universal Time)
cuid: cllno4g2o000a0ajv4cjv9wah
slug: windows-batch-files-what-why-and-how
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1692792367945/534ffefd-a43a-478a-8b0e-7d0082164832.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1692792360196/18fde2e5-7ec4-4623-8c1b-eaa62e417fdb.png
tags: windows, hello-world, cmd, bat, batch

---

Well, you must know or have heard about “Batch files” or “dot bat (.bat)” files if you have anything to do with computer stuff. If you don’t know what it exactly is, then maybe this is a pretty good place to start with. If you are already an expert programmer and know all about it then you are most welcome to share your knowledge here.

**What is a BATCH file?**

DOS, OS/2, and even Microsoft Windows use batch files as script files. It has a reason it is called a **batch** file because a batch file basically contains multiple DOS commands or a group of DOS commands to be executed sequentially by the command interpreter (Command prompt precisely). A batch file is run by [COMMAND.com](http://COMMAND.com) or cmd.exe which is similar to running shell scripts. Just so you know, [COMMAND.com](http://COMMAND.com) is not a website but a command interpreter in Microsoft Windows.

**Why use a BATCH file?**

BATCH files are generally used to schedule or automate a task. For example, if you want to copy certain files from one directory to another every week, you can just create a text file with the DOS commands written in a sequence and save it with a .bat extension to make it a batch file. Execute it and your job is done. No need to do the task repeatedly.

So, basically YES, it is indeed useful sometimes.

**How to use a BATCH file?**

Batch files are pretty easy to create and execute. Just open any text editor (maybe Notepad) and write the DOS commands in each line and save it as a .bat file (anyname.bat).

Here are a few tips when you are creating a batch file of your own.

You may consider writing “@echo off” at the start or at some point of the file where you don’t want the command prompt to display the processing steps of the commands.

example:

```powershell
@echo off
time
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692790617468/07d0e1f3-5ab4-4299-8734-3af7cb67749e.png align="center")

Another keyword is “echo” which is used to print or display any text in the command prompt. This example batch file displays “Hello World!”, prompts and waits for the user to press a key, and then terminates.

```powershell
@echo off
echo Hello World
pause
```

You can try a few more DOS commands together in a single batch file. Here is a simple batch file to copy a folder’s content to another destination.

```powershell
@echo off title Just Copying Few Stuffs to D Drive
echo Copying started..... 
xcopy /q /h /c /s "C:\My Folder" "D:\My New Folder"
echo Copying is done my friend 
msg * Copying is done. 
pause
```

To learn more about DOS commands you can just type “help” in the command prompt and to know details about any particular command write the command name followed by /? e.g. “xcopy /?”

To learn "keep experimenting".

Happy Coding ….