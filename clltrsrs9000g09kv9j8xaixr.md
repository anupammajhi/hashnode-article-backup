---
title: "POWERSHELL: Get-PrinterDetails from Print Servers"
datePublished: Thu Jul 24 2014 15:49:42 GMT+0000 (Coordinated Universal Time)
cuid: clltrsrs9000g09kv9j8xaixr
slug: powershell-get-printerdetails-from-print-servers
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693088495591/0a3c0191-5508-4a3e-a4b1-48e4639d4629.png
tags: windows, script, powershell, printer

---

In your server environments sometimes to get the details of printers on a print server people need to login to the print server, open the MMC console, go to print management, add servers, and then get to see the printer details.

This is a Powershell script that is invoked by the batch file in the main folder does that all for you. Running the batch file would give a prompt asking for the server name and credentials for the server.

The output produced is an HTML file that opens right after the execution completes.

[**View Code**](https://github.com/anupammajhi/PowershellScripts/tree/main/Get-PrinterDetails)

**How To Use**

1\. Download the zip file

2\. Extract the contents.

3\. Run the .bat file “ **PrintServerDetails.bat** “

4\. Enter the Print Server name

5\. Enter Credentials (You must have administrative access)

6\. Wait for the script to do its magic.