# T1060 - Registry Run Keys / Startup Folder

## Attributes
**Tactic**: Persistence
**System Requirements**: HKEY_LOCAL_MACHINE keys require administrator access to create and modify
**Effective Permissions**: User, Administrator
**Data Sources**: Windows Registry, File monitoring

## Description
Adding an entry to the "run keys" in the Registry or startup folder will cause the program referenced to be executed when a user logs in. These programs will be executed under the context of the user and will have that accounts associated permissions level. 

The following run keys are created by default on Windows:
`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`
`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce`
`HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run`
`HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce`

Registry run key entries can reference programs directly or list them as a dependency, f.e it is possible to load a DLL at logon using a "Depend" key with a run key: 
`reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnceEx\0001\Depend /v 1 /d "C:\temp\evil[.]dll"`

The following Registry keys can be used to set startup folder items for persistence:
`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders`
`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders`
`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders`
`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders`

Attackers can use these locations to execute malware, such as remote access tools, to maintain persistence through system reboots. Attacks may also use [Masquerading](https://attack.mitre.org/techniques/T1036) to make registry entries look as if they are associated with legit programs. 

## Mitigations
This type of attack technique cannot be easily mitigated with preventive controls since it is based on the abuse of system features.

## Detection
- Monitor your Registry for changes to run keys that do not correlate with known software, patch cycles, etc.
- Monitor your start folder for additions or changes

Tools such as Sysinternals Autoruns may also be used to detect system changes that could be attempts at persistence, including listing the run keys' Registry locations and startup folders. Suspicious program execution as startup programs may show up as outlier processes that have not been seen before when compared against historical data. 

## References
- [Mitre T1060](https://attack.mitre.org/techniques/T1060/)


