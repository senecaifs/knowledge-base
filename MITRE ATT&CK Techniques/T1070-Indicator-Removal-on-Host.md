# T1070 - Indicator Removal on Host

## Attributes

**Tactic**: Defense Evasion
**Effective Permissions**: Admin Permissions
**Data Sources**: File monitoring, Process monitoring, Process command-line parameters, API monitoring, Windows event logs
**Defense Bypassed**: Log analysis, Host intrusion prevention systems, Anti-virus

## Description

Attackers may delete or edit generated artifacts on a system, including logs, and potentially captured files such as quarantined malware. Events that may be deleted include bash history `~/.bash_history` , logs `/var/log`, and Windows events. 

Actions that interfere with eventing and other notifications that can be used to detect intrusion activity may compromise the integrity of security solutions, causing events to go unreported. They may also make forensic analysis and incident response more difficult due to lack of sufficient data to determine what occurred.

## Tools to Perform Attack

- Powershell
- cmd

### Clear Windows Event Logs

Attackers may clear logs that pertain to accoutn management, account logon and directory service accesss, etc. Any log can be deleted in order to hide an attackers presence. The following commands are used to clear logs:

- `wevtutil cl system`
- `wevtutil cl application`
- `wevtutil cl security`
- `wevtutil el | Foreach-Object {wevtutil cl “$_”}`

### Clear Linux Event Logs

- `rm -r /var/log/*`
- `rm ~/bash_history`

## Detection

File system monitoring may be used to detect improper deletion or modification of indicator files. For example, deleting Windows event logs (via native binaries, API functions, or Power Shell) may generate an alterable event (Event ID 1102: "The audit log was  cleared"). Events not stored on the file system may require different  detection mechanisms.

## Mitigation

- Obfuscate/encrypt event files locally and in transit to avoid giving feedback to an attacker
- Automatically forward events to a log server or data repository to prevent conditions in which the attacker can locate and manipulate data on the local system
- Minimize time delay on event reporting to avoid prolonged storage on the local system
- Protect generated event files that are stored locally with proper permissions and authentication

## References

- [Mitre - T1070](https://attack.mitre.org/techniques/T1070/)
- [Bash History](https://attack.mitre.org/techniques/T1139/)
