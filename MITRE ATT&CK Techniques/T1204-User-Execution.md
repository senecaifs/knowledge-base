
# T1204 - User Execution 

## Attributes

- **Tactic**: Execution
- **Permissions**: User

### Data Sources

- Anti-virus
- Process command-line parameters
- Process monitoring

## Description

This relies on a user interaction with communications sent by an attacker. This is often in the form of spear-fishing, where the user will open a link or a document that is used to launch malware, or exploit a vulnerability in a web application running on the target system. APT29 has used spear-fishing emails containing both links and attachments.

## Detection

Antivirus can be used to detect malicious files that have been downloaded. Additionally, watching command line arguments being executed within the network, specifically for command directed at compression applications, as these may be used to hide the files.

## Migation

- Proper training of end users is the key to mitigating this - basic training to not open links or download documents from unknown sources will help to avoid successful attacts using this method. 
- Certain file formats should be blocked for download by users that are not coming from a whitelisted site/sender and all downloaded files scanned prior to opening. 
- Ensuring that a good antivirus is installed on all devices and is actively scanning all downloads prior to allowing them to be opened will also help avoid falling victim to this type of attack. 
- Using whitelisting for applications such as Applocker will also help prevent malicious executables from successfully running.

## References

- [Mitre T1024](https://attack.mitre.org/techniques/T1204/)
