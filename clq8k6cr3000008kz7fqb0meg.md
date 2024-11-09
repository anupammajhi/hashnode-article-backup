---
title: "WINDOWS: Elevating PowerShell to Admin Mode without UAC prompt"
datePublished: Wed Oct 20 2021 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clq8k6cr3000008kz7fqb0meg
slug: windows-elevating-powershell-to-admin-mode-without-uac-prompt
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1702762402950/d85ab544-fd49-4c83-8843-3e0532a44be9.webp
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1702761609718/5143f8dc-ecd9-4e2d-a59e-48dad870e208.webp
tags: windows, powershell, admin, prompt, runas

---

PowerShell is a powerful tool, allowing to perform a myriad of tasks with just a few commands. However, there are times when elevated privileges are required to execute certain commands. This is as easy as right-clicking PowerShell and "Run As Administrator" 👎. Not as easy though when you want to launch it entirely from the terminal, without any GUI popup (UAC).

Microsoft provides a not so beautiful but powerful command to do that too. Drumrolls......

### The Runas Command

The \`runas\` command in Windows allows users to run a program with different user credentials than the ones currently in use. To open PowerShell in administrator mode with a domain admin user, the following command can be used:

```powershell
runas /user:DOMAIN\adminuser "powershell.exe -NoProfile -ExecutionPolicy Bypass -Command \"Start-Process powershell -ArgumentList '-NoProfile -NoExit' -Verb RunAs\""
```

`NOTE: Remember to replace placeholders like "DOMAIN" and "adminuser" with your actual domain name and admin username.`

This command will give a command line prompt to enter password.

This command utilizes the \`-NoExit\` parameter, ensuring that the PowerShell console remains open after execution.

### Breaking Down the Command

\- `runas /user:DOMAIN\adminuser`: Specifies the domain and admin user for which elevated privileges are required.

\- `powershell.exe -NoProfile -ExecutionPolicy Bypass`: Launches PowerShell with specific settings. The \`-NoProfile\` flag ensures a minimal profile is used, and \`-ExecutionPolicy Bypass\` allows the execution of scripts.

\- `Start-Process powershell`: Initiates a new PowerShell process.

\- `-ArgumentList '-NoProfile -NoExit'`: Passes additional arguments to the new PowerShell process. The \`-NoExit\` parameter prevents the console from closing immediately.

\- `-Verb RunAs`: Requests to run the process with elevated permissions.

### Use Cases

\- **Script Execution**: Running scripts that require administrative privileges.

\- **System Configurations**: Making changes to system settings that necessitate elevated access.

\- **Software Installations**: Installing software that requires administrative rights.