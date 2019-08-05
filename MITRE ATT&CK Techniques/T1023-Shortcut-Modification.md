
# T1023 - Shortcut Modification

## Attributes
**Tactic**: Persistence  
**Platform**: Windows  
**Permissions Required**: User, Administrator  
**Data Sources**: File monitoring, Process monitoring, Process command-line parameters

## Description
Shortcut files are .lnk files which launch executables in windows. When a shortcut file is opened or executed, referenced files or programs that will be opened or executed along with it by a system startup process. Attackers could use shortcuts to execute their tools for persistence. They may create a new shortcut as a means of indirection that may use masquerading to look like a legitimate program. Attackers could also edit the target path or entirely replace an existing shortcut so their tools will be executed instead of the intended legitimate program.

## Tools to Perform Attack
Windows command line using parameters like "-encoded", "-bxor" decoding commands like [System.Text.Encoding]::Unicode.GetString([convert]::FromBase64String($code))

### Examples
Shortcut file can be crafted to execute a PowerShell command that read, decoded, and executed additional code from within the shortcut file.

## Detection
Since a shortcut's target path likely will not change, modifications to shortcut files that do not correlate with known software changes, patches, removal, etc., may be suspicious.

Correlation should attempt to relate shortcut file change or creation events to other potentially suspicious events such as process launches of unknown executables that make network connections.

## Mitigation
- Limit permissions for who can create symbolic links in Windows to appropriate groups such as Administrators and necessary groups for virtualization. This can be done through GPO: `Computer Configuration  > Windows Settings > Security Settings > Local Policies > User Rights Assignment`: Create symbolic links.

- Identify and block unknown, potentially malicious software that may be executed through shortcut modification by using whitelisting tools, like AppLocker, or Software Restriction Policies where appropriate

## References
 - [Mitre T1023](https://attack.mitre.org/techniques/T1023/)
- https://www.sentinelone.com/blog/windows-shortcut-file-lnk-sneaking-malware/
- https://blog.trendmicro.com/trendlabs-security-intelligence/malicious-macro-hijacks-desktop-shortcuts-to-deliver-backdoor/
