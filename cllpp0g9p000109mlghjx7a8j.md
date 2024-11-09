---
title: "POWERSHELL: Why the hell “Powershell”?"
datePublished: Tue Jun 17 2014 06:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cllpp0g9p000109mlghjx7a8j
slug: powershell-why-the-hell-powershell
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1692913150601/8b3b954c-ce2c-42c3-a11e-02d37458c437.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1692913483561/e1c1d0b0-7203-48dd-87f0-1e496aff38b6.png
tags: server, command-line, windows, powershell, scripting

---

Well, while we are starting to learn Powershell, just like me many people would have come across the thought  ***“OK, the name sounds cool, it has a fancy logo, seems to be POWERful, I have heard the nerds talk about it, but ….. What the hell is it?”.***

This is my first blog post about Powershell and I will try to answer the obvious question “Why the hell do we even need Powershell?”.

Powershell is basically a scripting language provided by Microsoft. It is a task automation and configuration management framework consisting of a classic command-line shell and associated scripting language built on the .NET framework. We all know about VBScript which is quite popular in the Windows scripting world, Powershell is similar, just with a lot more muscle than VBScript could afford to have.

### A few things that make Powershell beautiful are:

* It has full access to COM and WMI which makes Windows administration easier.
    
* It provides you access to both local and remote systems with WSMan and CIM making it easier to communicate with devices.
    
* Along with command-line, it also comes with a GUI-based ISE (Integrated Scripting Environment) where we can write, test, and debug scripts.
    
* It gives you full access to the .NET classes making it easier for C#, C++ programmers, and VBScripters to learn Powershell.
    
* It uses commands called *cmdlets* (pronounced *command-lets*), which use a “Verb-Noun” format, hence recognizing what cmdlet to use next becomes obvious. For example, if you want to write something on the host’s screen, the cmdlet for it is “Write-Host”. Want help then just type “Get-Help”.
    
* Powershell has a very elaborate manual/help document integrated with it.
    
* The most powerful thing about Powershell is that, from Windows 2012 onwards, all Microsoft tools like Exchange, SQL, etc. can be managed completely with the help of Powershell.
    
* Unlike VBScript, Powershell makes it difficult to be used for malicious tasks like creating viruses, hence making it secure.
    
* Powershell uses something called “aliases” to retain compatibility with other scripting tools like MS-DOS, Linux Shell, etc. So if you type in Powershell “dir”(DOS) or “ls”(Linux), you still get the results.
    
* And the list goes on ….
    

So, ultimately answering the question, Powershell is what we can consider to be the best friend for a System-Administrator and other people who want to automate tasks, develop scripts to generate data and reports, and manage Windows applications.

I hope I was able to explain the essence of Powershell and how powerful a tool it is.

Do leave comments and keep visiting this blog for future posts.