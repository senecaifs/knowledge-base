
# T1084 - Windows Management Instrumentation Event Subscription

## Attributes

**Tactic**: Persistence
**Effective Permissions**: Administrator, SYSTEM
**Data Sources**: WMI Objects

## Description

The attacker will use the capabilities of WMI which allows you to subscribe to an event  and execute arbitrary code when that event occurs, providing persistence on a system.
Attackers may attempt to evade detection of this technique by compiling WMI scripts. Examples of events that may be subscribed to are the computers uptime or clock time. 


## Detection

Monitor WMI event subscription entries, comparing current WMI event subscriptions to known good subscriptions for each host. Tools such as Sysinternals Autoruns may also be used to detect WMI changes that could be attempts at persistence.

## Mitigation

- [Privileged Account Management](https://attack.mitre.org/mitigations/M1026)  is preventing credential overlap accross systems of administrator and privileged accounts. 

- [User Account Management](https://attack.mitre.org/mitigations/M1018) is either restricting other users from connecting remotely, or disallowing all users from connecting remotely to WMI. 

## References

- [Mitre T1084](https://attack.mitre.org/techniques/T1084/)
