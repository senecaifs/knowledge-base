# T1084- #Windows Management Instrumentation Event Subscription

## Attributes

Tactic: Persistence
Effective Permissions:Administrator, SYSTEM
Data Sources: WMI Objects

## Description

The attacker will use the capabilities of WMI which allows you to subscribe to an event  and execute arbitrary code when that event occurs, providing persistence on a system.
Examples of events that may be subscribed to are the wall clock time or the computer's uptime.

## Tools To Attack

 - WMI

## Detection

Monitor WMI event subscription entries, comparing current WMI event subscriptions to known good subscriptions for each host. Tools such as Sysinternals Autoruns may also be used to detect WMI changes that could be attempts at persistence.

## Mitigation

By default, only administrators are allowed to connect remotely using WMI; restrict other users that are allowed to connect, or disallow all users from connecting remotely to WMI. Prevent credential overlap across systems of administrator and privileged accounts.

## References

- **Mitre article**
- [https://attack.mitre.org/techniques/T1084/](https://attack.mitre.org/techniques/T1084/)
