# T1095 - Standard Non-Application Layer Protocol

## Attributes
- **Tactic**: Command And Control
- **Effective Permissions**: Not Specified
- **Data Sources**: Host network interface, Netflow/Enclave netflow, Network intrusion detection system, Network protocol analysis, Packet capture, Process use of network

## Description
Has to do with non-application layer protocols being used for communication between a host and C2. This can include using ICMP, UDP, or TCP. There are many other protocols that can be used.

### Other Information
APT29 uses TCP for C2 communications.

## Detection
Abnormal network traffic and data flow must be analyzed. Packet contents can be investigated in order to figure out if they follow the expected protocol behavior.

## Mitigation
- Firewalls must be configured to only allow traffic on authorized outgoing ports. Network intrusion detection systems can be used to identify undesirable traffic using signatures.

## References
-  [Mitre T1015](https://attack.mitre.org/techniques/T1015/)
  - [Mitre T1095](https://attack.mitre.org/techniques/T1095/)
