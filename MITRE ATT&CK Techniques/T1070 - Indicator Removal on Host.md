# T1070 - Indicator Removal on Host

## Attributes

- Tactic: Defense Evasion
- Effective Permissions: Admin Permission
- Data Sources: File monitoring, Process monitoring, Process command-line parameters, API monitoring, Windows event logs

## Description

Attacker may delete or edit system logs or captured files such as quarantined malware. location of these files may vary depending on the OS that's under attack.

Windows Events might get deleted !

Linux Bash History `~/.bash_history` and logs in `/var/log` 

## Tools to Perform Attack

### WINDOWS

- `wevtutil cl system`
- `wevtutil cl application`
- `wevtutil cl security`
- `wevtutil el | Foreach-Object {wevtutil cl “$_”}`

### LINUX

- `rm -r /var/log/*`
- `rm ~/bash_history`

## Detection

File system monitoring may be used to detect improper deletion or modification of indicator files. For example, deleting Windows event logs (via native binaries [[28\]](https://docs.microsoft.com/windows-server/administration/windows-commands/wevtutil), API functions [[29\]](https://msdn.microsoft.com/library/system.diagnostics.eventlog.clear.aspx), or [Power hell] https://attack.mitre.org/techniques/T1086) [[30\]](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/clear-eventlog)) may generate an alterable event (Event ID 1102: "The audit log was 
cleared"). Events not stored on the file system may require different  detection mechanisms.

## Mitigation

Automatically forward events to a log server or data repository to prevent conditions in which the adversary can locate and manipulate data on the local system. When possible, minimize time delay on event reporting to avoid prolonged storage on the local system. Protect generated event files that are stored locally with proper permissions  and authentication and limit opportunities for adversaries to increase  privileges by preventing Privilege Escalation opportunities. Obfuscate/encrypt event files locally and in transit to avoid giving feedback to an adversary.

## References

References to all articles go here

- **T1070**
  - [T1070](https://attack.mitre.org/techniques/T1070/)
  - [Bash History](https://attack.mitre.org/techniques/T1139/)
