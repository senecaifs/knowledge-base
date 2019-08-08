
# T1045 - Software Packing

## Attributes

- **Tactic**: Defense Evasion
- **Data Sources**: Binary file metadata
- **Defense Bypassed**: Signature-based detection, Anti-Virus, Heuristic detection

## Description

This is a method of compressing or encrypting an executable. Packing an executable changes the file signature in an attempt to avoid signature-based detection (checking the source code to see if it matches with other well-known malware source code). Most decompression techniques decompress the executable code in memory.

Packers are used to perform packing. Some examples of popular packers include: MPRESS, and UPX. A list of more well-know packers can be found [here](https://en.wikipedia.org/wiki/Executable_compression). However, attackers may create their own packing techniques that do not leave the same artifacts (or evidence) as other well-known packers to evade defenses.

## Mitigation

- Make sure your Anti-virus definitions are up-to-date. Create new definitions if you observe malware that does not already have a signature. Try to use your own deduction and reasoning if you think an executable is malware.
- Identify and prevent the execution of potentially malicious software that may have been packed by using whitelisting tools like AppLocker or Software Restriction Policies.

## Detection

Use file scanning to look for known software packers or artifacts of packing techniques. If an executable is packed, it does not exactly mean that it is malware, because legit software also use packing techniques to reduce the size of executables, or protect their own code.

## References

- [Mitre T1045](https://attack.mitre.org/techniques/T1045/)
