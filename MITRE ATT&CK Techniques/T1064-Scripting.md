# T1064- Scripting

## Attributes
Tactic: Defense Evasion, Execution
Effective Permissions: User
Data Sources: Process monitoring, file monitoring, process command line parameters

## Description

Scripting is used to aid in operations and perform multiple actions in quick succession automatically. It is useful for speeding up manual tasks as well as reducing time required to gain access to critical resources where time may be of the essence. Some scripting languages may be used to bypass process monitoring mechanisms by directly interacting with the OS at an API level instead of calling upon other programs. Some common scripting languages for Windows include VB Script and PowerShell but could also include cmd batch scripts.

## Tools to Perform Attack

Powershell, VBScript, Metaspoit, Veil, batch scripts, Python, Java script, shell script, MS Word Scripts, etc.

### Examples
An exploit in MS word allows one to run a script simply by opening the .docx file.
>In depth example on how this works:
https://null-byte.wonderhowto.com/how-to/execute-code-microsoft-word-document-without-security-warnings-0180495/

## Detection
Scripts are commonly used on admin/developer systems. If scripting is disabled for regular users, any attempts to run or enable scripting should be considered suspicious. If scripts are not commonly used on a particular system but enabled, any running out of cycle or with admin functionality should be considered suspicious. Scrips like these should be found within the file system and analyzed to determine actions and intent. 
Often, the effects of the scripts running on the particular system will generate event logs. Processes and command line arguments for script execution should be monitored closely. Actions may be related to network and system information discovery, collections, and/or other scriptable post-compromise behaviors could be used as indicators that lead back to the source script.
### Example
Office file attachments that may contain potentially malicious macros. Execution of said macros may create suspicious process trees, such as windword.exe spawning instances of cmd.exe, wscript.exe or even Powershell may indicate suspicious activity.

## Mitigation
Disable any unused features such as VBScript or restrict access to scripting engines or scriptable admin frameworks such as PowerShell.
To mitigate against Office macros, enable protected view and block macros through Group Policy. Having a virtualized enviroment, or application microsegmentation may also aid in mitigating the impact of a compromise, however, additional exploits and weaknesses in implementation may still exist.

## References

  - [https://attack.mitre.org/techniques/T1064/](https://attack.mitre.org/techniques/T1064/)
  - [https://null-byte.wonderhowto.com/how-to/execute-code-microsoft-word-document-without-security-warnings-0180495/]
