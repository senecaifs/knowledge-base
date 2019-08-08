# T1043 – Commonly Used Ports 

## Attributes

- **Tactic**: Command and Control
- **Data Sources**: Packet capture, Netflow/Enclave netflow, Process use of network, Process monitoring

## Description

To avoid drawing attention to traffic going between the Command and Control (C2) and the compromised system, the attacker may utilize commonly used ports. Such ports include **TCP:80 (HTTP)**,  **TCP:443 (HTTPS)**, **TCP:25 (SMTP)** and **TCP/UDP:53 (DNS)**. As all of these ports generally will have a great deal of traffic on an average network, additionally traffic by the attacker will not be noted as suspicious. More importantly, most firewalls and IDS' will be configured to allow this traffic in and out. 

For connections that occur internally within an enclave, examples of common ports are: 
- **TCP/UDP:135 (RPC)**
- **TCP/UDP:22 (SSH)**
- **TCP/UDP:3389 (RDP)**

### Examples

An attacker has gained access to a server on X Company’s website. To communicate undetected by X Company, the C2 Server is set up to communicate with the compromised system over port 443. As HTTPS is now ubiquitous, a large volume of traffic through this port may not be flagged as suspicious by X Company’s network team. 

## Detection

While detection can be difficult, monitoring the network for data flows that are large, originate from network sources not previously seen, and also do not fit the protocol structure used on that port are effective means of detecting this behaviour. 

## Mitigation

- Configure internal and external firewalls to block traffic using common ports that associate to network protocols that may be unnecessary for that partuclar network segment
- Configure an IDS to look for a malware's network signature
- Whitelist common ports to be explicitly configured and monitored

## References

-	[Mitre T1043]( https://attack.mitre.org/techniques/T1043/)
