# T1053 - Scheduled Task

## Attributes
**Tactic**: Execution, Persistence, Privilege Escalation
**Effective Permissions**: Admin, SYSTEM, User
**Data Sources**: File Monitoring, Process monitoring, process command-line parameters, Windows Event logs

## Description
Utilities such as Windows Task Scheduler, or schtasks are used to schedule programs or scripts to be run at a specific date/time. Tasks can also be scheduled remotely provided that proper authentication is met to use RPC (remote procedure call) and file/printer sharing is turned on. Typically this requires admin permissions on the remote system.
Task scheduling may be used in many ways, including but not limited to:  execute programs on startup or on scheduled basis, to gain SYSTEM privileges, or run a process under the context of a specified account.

## Tools to Perform Attack
- at
- schtasks
- Windows Task Scheduler
- cron
- Powershell

### Examples
Malware may add scheduled task to establish persistence.
Malformed task files (.job, from older versions of Windows) may also be imported.
Example on how this works [here](https://www.bleepingcomputer.com/news/security/new-zero-day-exploit-for-bug-in-windows-10-task-scheduler/).
[More examples](https://kb.cert.org/vuls/id/119704/)

## Detection
Monitor scheduled task creation from common utilities using command-line invocation. Legitimate scheduled tasks are often created during installation of software or sysadmin functions. Process execution can be monitored through `svchost.exe` on Windows 10 (or `taskeng.exe` for older versions of windows). If scheduled tasks are not used for persistence then they are most likely removed when the action is completed. Change logs are located in `%systemroot%\System32\Tasks`,  and should be monitored for entries that do not correlate with known software or patch cycles. Data and events should be viewed as part of a behavior chain that may lead to other activities such as network connections made for Command and Control, details about the environment through Lateral Movement, and Discovery.
Configure event logging for scheduled task creation and changes by enabling the **Microsoft-Windows-TaskScheduler/Operational** setting within the event logger.
Several events will be logged onto the scheduled task activity from then on include:

- Event ID 106 - Scheduled task registered
- Event ID 140 - Scheduled task updated
- Event ID 141 - Scheduled task removed

Other tools such as Sysinternals Autoruns may be used to detect system changes that could be attempts at persistence, including listing current scheduled tasks. Look for changes to scheduled tasks like outlier processes that have not shown up before, and compare it against historical data.
Remote access tools with built-in features may interact directly with the Windows API to perform these functions outside of typical system utilities. Tasks may also be created through Windows system management tools such as [Windows Management Instrumentation](https://attack.mitre.org/techniques/T1047) and PowerShell, so additional logging may need to be configured to gather the appropriate data.
Remote access tools may also have features that may interact directly with Windows API to perform functions outside typical system utilities. Tasks may also be created through Windows Management Instrumentation or Powershell, so additional logging may be needed to gather appropriate information.

## Mitigation
- Privileges of user accounts should be limited; only authorized admins should be able to create scheduled tasks on any system
- Usage of Powershell and cmd should also be limited to admins. Pen testing tools such as Powersploit  framework contain modules that can be used to search a system for permission weaknesses in scheduled tasks that could be used to escalate privileges.

### Techniques/Examples
Regular users should be prevented from running or stopping Task Scheduler by modifying the registry, or by changing the group policy through `gpedit.msc`. More on that [here](https://support.microsoft.com/en-ca/help/305612/how-to-prevent-a-user-from-running-task-scheduler-in-windows).

Configure settings for scheduled tasks to force tasks to run under the context of the authenticated account instead of allowing them to run in default as SYSTEM. The associated Registry key is located at `HKLM\SYSTEM\CurrentControlSet\Control\Lsa\SubmitControl`. The setting can be configured through GPO: `Computer Configuration > Windows Settings > Security Settings > Local Policies > Security Options > Domain Controller`: disabled.

Configure the Increase Scheduling Priority option to only allow the Administrators group the rights to schedule a priority process. This can be can be configured through GPO: `Computer Configuration  > Windows Settings > Security Settings > Local Policies > User Rights Assignment`: Increase scheduling priority.

Identify and block unnecessary system utilities or potentially malicious software that may be used to schedule tasks using whitelisting tools, such as Software Restriction Policies.

## References
- [Mitre T1053](https://attack.mitre.org/techniques/T1053/)
- [How to prevent a user from running Task Scheduler in Windows](https://support.microsoft.com/en-ca/help/305612/how-to-prevent-a-user-from-running-task-scheduler-in-windows)
