# T1088 – Bypass User Account Control 

## Attributes

Tactic: Defense Evasion, Privilege Escalation

Effective Permissions: Administrator

Data Sources: System calls, Process monitoring, Authentication logs, Process command-line parameters

## Description

User Account Control (UAC) is a feature on the Windows OS that works to prevent changes to the system without authorization. A pop-up will be triggered on the users’ screen when a program tries to make system changes, which requires interaction from the user, and, depending on the program/user account type, an Administrator password, to continue with the changes. 

While there are many types of attacks to bypass UAC, one of the most common types involves attempting to bypass UAC by launching an application that has auto-elevated privileges after replacing the registry key content with the path to an executable that the attacker wants to run. This will allow the executable to be run without triggering the interactive pop up. This attack is most successful on a user who is part of the Local Administrators group. 

## Tools to Perform Attack

PowerShell, Command Line

### Examples
Some processes will be called automatically with high privileges, such as `sdclt.exe`, which looks for the path to `control.exe`, and then runs it with elevated privilege. The attacker will change the path of the executable to one that they want to run elevated. An example PowerShell script to perform this attack can be found [here](https://raw.githubusercontent.com/enigma0x3/Misc-PowerShell-Stuff/master/Invoke-AppPathBypass.ps1)


This method is also used with `eventvwr.exe` as it also automatically runs the application path specified elevated. Both of these techniques can be used as “fileless”, meaning that they are used to run PowerShell or another application on the victim’s system without needing to drop an executable. However, they may also be used with an executable that the attacker has first placed in the system – this attack will use a similar method. 


## Detection

To detect attacks where the registry keys for `eventvwr.exe` have been changed, monitor `[HKEY_CURRENT_USER]\Software\Classes\mscfile\shell\open\command`

For changes to the registry keys for `sdclt.exe`, monitor any changes to the following:
`[HKEY_CURRENT_USER]\Software\Microsoft\Windows\CurrentVersion\App Paths\control.exe` and `[HKEY_CURRENT_USER]\Software\Classes\exefile\shell\runas\command\isolatedCommand`

Additionally, monitor the processes being run by the applications commonly targeted in these attacks for unusual activity. 

### Sysmon Event IDs 

**12** – Registry object added or deleted

**13** – Registry value set

**1** – Process Create


## Mitigation

Ensure that users are not local administrators and set UAC to **always notify** to help mitigate these attacks. 



## References

-	[T1088](https://attack.mitre.org/techniques/T1088/)
-	[Bypass UAC Using Registry Keys] (https://attackiq.com/blog/2018/05/14/bypassing-uac-using-registry-keys/)
-	[UACMe](https://github.com/hfiref0x/UACME)
-	[Try It](http://blog.sevagas.com/?Yet-another-sdclt-UAC-bypass)
-	[Bypass UAC with App Paths]( https://enigma0x3.net/2017/03/14/bypassing-uac-using-app-paths/)

