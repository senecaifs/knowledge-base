
# T1086 - Powershell

## Attributes

Find these on the MITRE page

- Tactic: Execution
- Effective Permissions: User, Administrator
- Data Sources: Powershell Logs, Loaded DLLs, DLL monitoring, Windows Registry, File Monitoring, Process Monitoring, Process Command-Line Parameters

## Description

Powershell can be used to run scripts filled with arbitrary code created by the attackers, and also discover information about our system.  Powershell can also be used to download and run executables from the internet, and even run them in the memory, without being on any local disks. However, Administrative perms are required to use Powershell to connect to remote systems. Powershell scripts can also be executed without directly running the powershell.exe executable, but instead with Powershells underlying System.Management.Automation assembly exposed through the .NET framework and Windows CLI.  


Powershell testing tools include: Empire, PowerSploit, and PSAttack.

## Tools to Perform Attack

Powershell

## Detection

Attackers may try to change the execution policy either through the registry, or the command line. Check to see if this policy has been changed to detect if Powershell has been used in a malicious way. 

Monitor for loading and/or execution of artifacts associated with Powershell specific assemblies, such as System.Management.Automation.dll 

### Enable Powershell logging using GP Editor
Powershell 4.0 and 5.0 should have enhanced logging capabilities (earlier versions do not). To enable Powershell logging using GP Editor, first run `gpedit.msc` then navigate to `Computer Configuration\Administrative Templates\Windows Components\Windows PowerShell` and enable `Turn on PowerSehll Transcription`. When enabling, you can enter a prefered output directory (try to set this up in a way that will send logs to our logging server). You can also tick the `Include invocation headers:` option to include the command start time for each command.  

## Mitigation

### Execution Policy
Restrict Powershell execution policy to admins, and only signed scripts (if you're bold). To do this, open a Powershell window as an admin, and use the cmdlet `Set-ExecutionPolicy`. You can look up which execution policy to use based your needs [here.](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6)

Disable the WinRM service to help prevent uses of Powershell for remote execution. 