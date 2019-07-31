# T1043 – Commonly Used Ports 

## Attributes

Tatic: Command and Control

Data Sources: Packet capture, Netflow/Enclave netflow, Process use of network, Process monitoring

## Description
To avoid drawing attention to traffic going between the Command and Control (C2) and the compromised system the attacker may utilize commonly used ports. Such ports include Port 80 (HTTP), port 443 (HTTPS), port 25 (SMTP) and port 52 (DNS). As all of these ports generally will have a great deal of traffic on an average network, additionally traffic by the attacker will not be noted as suspicious. More importantly, most firewalls and IDS will be configured to allow this traffic in and out. 

### Example

An attacker has gained access to a server on X Company’s website. To communicate undetected by X Company, the C2 Server is set up to communicate with the compromised system over port 443. As HTTPS is now ubiquitous, a large volume of traffic through this port may not be flagged as suspicious by X Company’s network team. 

## Detection

While detection can be difficult, monitoring the network for data flows that are large, originate from network sources not previously seen, and also do not fit the protocol structure used on that port are effective means of detecting this behaviour. 

## Mitigation

Most malware has a network signature. An IDS can be configured to look for these signatures to identify malicious entities. Additionally, for some ports, whitelisting can be used by the company to avoid external users sneaking traffic through on these ports. As port 443 is used by APT29, this port should be explicitly configured and monitored during this exercise. 

## References

-	[T1043]( https://attack.mitre.org/techniques/T1043/)
