---
title: "POWERSHELL: Getting Started"
datePublished: Sun Jul 06 2014 06:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cllppzgse000108mdhby58ybq
slug: powershell-getting-started
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1692913646213/790eac0a-d7a9-41e5-8e9f-c417cba94a98.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1692915063556/0e3267d0-4062-45be-accf-a617de518700.png
tags: windows, beginners, powershell, hello-world

---

After we get an overview of what Powershell is, we are ready to get started. So let’s Get Started!!

### Installation

Installing Powershell is extremely easy. Even better, it comes pre-installed with Windows 7 (Desktop Environment) and Windows Server 2008 R2 (Server Environment) onwards. It is available on the Microsoft website to be downloaded and installed in other versions of Windows XP,2003, and Vista.

### **Launching PowerShell**

On Windows, you can launch PowerShell from the Start Menu. On non-Windows platforms, you can open PowerShell Core from the terminal.

### Interactivity Is 'The Key'

Powershell REPL. Don’t worry about the terminologies. REPL means Read-Evaluate-Print-Loop. This simply means that it reads the value that you write, evaluates it, produces the result, and moves ahead to the next loop instance. Let’s try this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692914004062/dead8e3f-dc98-4068-897d-383ea915e8c6.png align="center")

We didn’t really write any complicated code or script, instead, we just wrote what we needed and it worked. Powershell is highly interactive. Let’s try another example.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692914506750/8be5ca64-7a97-4629-92f6-e4c267df0fb1.png align="center")

![](http://anupamm.com/wp-content/uploads/2014/07/Capture.png align="left")

See !! It simply works.

If you are familiar with .NET Framework (Don’t worry if you are not), you can also try something like this:

```powershell
# Specify the path to the file you want to check
$fileToCheck = "C:\Path\To\Your\File.txt"

# Check if the file exists using the .NET System.IO.File class
if ([System.IO.File]::Exists($fileToCheck)) {
    Write-Host "The file '$fileToCheck' exists."
} else {
    Write-Host "The file '$fileToCheck' does not exist."
}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692914683501/1612a2eb-5e38-47cf-99fd-2cba18fb460e.png align="center")

Yes, it is extremely flexible with that too.

### Cmdlets: The Life Saver

The instructional commands in Powershell are known as CMDLETS (pronounced command-lets).

Each cmdlet has a VERB-NOUN format. For example Get-Help, Write-Host, Clear-Variable, Get-Content, Out-File, Get-ChildItem, and so on. And nothing to worry about cases. Powershell is not case-sensitive, not even in the case of variables.

Get-Help is the most handy cmdlet that you would like to keep under your belt.

If you need to know what are the cmdlets related to FILE. No worries, just type **Get-Help *File*** and you will get the list. The asterisk is for wildcard which means “anything”. So the ***File*** means **&lt;anything&gt;File&lt;anything&gt;,** which again essentially means “starts with any letter, word, symbol and ends with any letter, word, symbol but must contain File in-between.

```powershell
Get-Help *File*
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692914918399/f68f3613-19e3-4645-8254-706ea01258d8.png align="center")

And if you already know the cmdlet, say Out-File, you can get the information about it with this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692914992666/9269aa02-1637-46a8-9133-321305d18c3e.png align="center")

If you need examples for Out-File cmdlet, you can type

```powershell
Get-Help Out-File -Examples
```

If you need every detail about it including examples,

```powershell
Get-Help Out-File -Detailed
```

### Execution Policy

Powershell has a security feature called Execution Policy. This policy saves you from executing scripts from non-trusted sources. By default, Powershell will set itself to Restricted policy which means you cannot execute scripts.

To know what your Powershell environment Execution policy is set to, just type

```powershell
Get-ExecutionPolicy
```

The **Set-ExecutionPolicy** cmdlet enables you to determine which Windows PowerShell scripts (if any) will be allowed to run on your computer. Windows PowerShell has four different execution policies:

* **Restricted** – No scripts can be run. Windows PowerShell can be used only in interactive mode.
    
* **AllSigned** – Only scripts signed by a trusted publisher can be run.
    
* **RemoteSigned** – Downloaded scripts must be signed by a trusted publisher before they can be run.
    
* **Unrestricted** – No restrictions; all Windows PowerShell scripts can be run.
    

We will go into detail about how to work with these in my later posts.

Happy scripting!