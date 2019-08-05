# T1027 - Obfuscated Files or Information

## Attributes
**Tactic**: Defense Evasion  
**Effective Permissions**: System  
**Data Sources**: Network protocol analysis, Process use of network, File monitoring, Malware reverse engineering, Binary file metadata, Process command-line parameters, Environment variable, Process monitoring, Windows event logs, Network intrusion detection system, Email gateway, SSL/TLS inspection
**Defense Bypassed**: Host forensic analysis, Signature-based detection, Host intrusion prevention systems, Application whitelisting, Process whitelisting, Log analysis, Whitelisting by file name or path

## Description
Attacker may attempt to evade discovery/analysis by encoding, encrypting or encrytping the payload or binaries. Additionally portions of file maybe encoded to hide strings or payload be split into separate files.

Payloads may be compressed, archived, or encrypted in order to avoid detection. Sometimes a user's action may be required to open and Deobfuscate/Decode Files or Informationfor User Execution. The user may also be required to input a password to open a password protected compressed/encrypted file that was provided by the attacker. Attackers may also used compressed or archived scripts, such as Javascript.

Payloads may also be split into separate, seemingly benign files that only reveal malicious functionality when reassembled.

Attackers may obfuscate commands from payloads or directly via cmd. Environment variables, aliases, characters, and other platform/language specific semantics can be used to evade signature based detections and whitelisting mechanisms

## Tools to Perform Attack
Windows command line using parameters like "-encoded", "-bxor" decoding commands like `[System.Text.Encoding]::Unicode.GetString([convert]::FromBase64String($code))`

### Examples
```
$Command = 'Get-Service BITS' 

# converting to Base64 encoded string
$Encoded = [convert]::ToBase64String([System.Text.encoding]::Unicode.GetBytes($command)) 

# running the encoded command with powershell
powershell.exe -encoded $Encoded
```

## Detection
Detection can be performed by Flagging and analyzing commands containing indicators of obfuscation and known suspicious syntax such as uninterpreted escape characters like '''^''' and '''"'''. Windows' Sysmon and Event ID 4688 displays command-line arguments for processes.  

Network analysis and email gateway can help detecting compressed, encoded files.  
Tip: MZ header of PE files commonly appears to be `TV(oA|pB|pQ|qA|qQ|ro)` in case of encoding.

## Mitigation
- Ensure logging and detection mechanisms analyze commands after being processed/interpreted, rather than the raw input. 
- Monitor compressed and encrypted files sent over the network and through email

## References
- [Mitre T1027](https://attack.mitre.org/techniques/T1027/)
- [Windows 10 to offer application developers new malware defeses](https://www.microsoft.com/security/blog/2015/06/09/windows-10-to-offer-application-developers-new-malware-defenses/)
