# T1097 - Pass the Ticket

## Attributes

- **Tactic**: Lateral Movement
- **Effective Permissions**: Not Specified
- **Data Sources**: Authentication logs

## Description

Pass the Ticket is an attack method that can allow an attacker to authenticate to a system using Kerberos tickets without knowledge of a users password.

This attack falls into two categories, Golden Ticket and Silver Ticket.

Golden Tickets are obtained on a Domain Controller from the Key Distribution Service account NTLM hash, this account is named KRBTGT. A Golden Ticket will enable an attacker to generate a TGT for any account in Active Directory.

Silver Tickets attacks are performed by crafting a TGS for a service account. A service account is a special account that is used by an application, such as SQL Server. In order to create a forged TGS the password or password hash must be known by an attacker.

## Tools to Perform Attack

- Mimikatz
  - A tool that has the capability to gather credential data from a Windows System (Passwords, NTLM hash, TGT, TGS).
  - Mimikatz exists as an executable that can be run on Windows directly.
- PowerSploit
  - Powershell post-exploitation framework
  - Mimikatz can be loaded into memory using ```Invoke-Mimikatz```
- Empire
  - Powershell and Python post-exploitation framework.
  - Can leverage its own implementation of Mimikatz to obtain and use Silver and Golden tickets.

### Examples

The following command is run using PowerSploit, it can execute Mimikatz on remote computers to gather credentials:

```powershell
Invoke-Mimikatz -DumpCreds -ComputerName @("computer1", "computer2")
```

The following is run in Mimikatz, these commands will allow an attacker to retrieve the hash of the KRBTGT account from a Domain Controller.

```shell
 privilege::debug
 lsadump::lsa /inject /name:krbtgt
```

The following command is run in Mimikatz, it can be used to create a Golden Ticket:

```shell
kerberos::golden /admin:ADMIINACCOUNTNAME /domain:DOMAINFQDN /id:ACCOUNTRID /sid:DOMAINSID /krbtgt:KRBTGTPASSWORDHASH /ptt
```

The following command is run in Mimikatz, it can be used to create a Silver Ticket for a CIFS service on the server adsmswin2k8r2.lab.adsecurity.org:

```shell
kerberos::golden /admin:LukeSkywalker /id:1106 /domain:lab.adsecurity.org /sid:S-1-5-21-1473643419-774954089-2222329127 /target:adsmswin2k8r2.lab.adsecurity.org /rc4:d7e2b80507ea074ad59f152a1ba20458 /service:cifs /ptt
```

### Other Information

#### Kerberos Protocol

The following diagram shows the steps taken to authenticate to a server using Kerberos.

![Simple Kerberos Diagram](https://www.varonis.com/blog/wp-content/uploads/2018/07/Kerberos-Graphics-1-v2-787x790.jpg)

#### Types of Tickets

The Ticket Granting Ticket (TGT) is a ticket used to prove to the Key Distribution Center (KDC), located on the Domain Controller, that a user is authenticated. The contents of the TGT are encrypted with the KRBTGT NTML hash, this ticket should only be read by the KDC.

The Ticket Granting Service (TGS) is a ticket used to prove to a service that a user is authenticated, this ticket contains data that can only be decrypted by the service.

## Detection

### Monitoring

#### Kerberos Logs

- The following Windows Event IDs are related to Kerberos:

  - Event [ID 4768](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4768) is created for every TGT request.

  - Event [ID 4769](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4769) is created for every TGS request.

  - Event [ID 4770](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4770) is created for every TGT renew.

Using these event ID the analyst must look for anything out of the ordinary. For example if a TGS request or TGT renew is seen the analyst can look back in time to check if there is TGT request that match's the computer and user.

#### Golden Ticket Detection

- Golden Ticket events may have one of these issues:
  - The Account Domain field is blank when it should be DOMAIN.
  - The Account Domain field is DOMAIN FQDN when it should be DOMAIN.

- Event ID: 4624 (Account Logon)
  - Account Domain is FQDN & should be short domain name
  - Account Domain:        LAB.ADSECURITY.ORG   [ADSECLAB]

- Event ID: 4672 (Admin Logon)
  - Account Domain is blank & should be short domain name
  - Account Domain:        _______________   [ADSECLAB]

#### Silver Ticket Detection

- Silver Ticket events may have one of these issues:
  - The Account Domain field is blank when it should be DOMAIN
  - The Account Domain field is DOMAIN FQDN when it should be DOMAIN.

- Event ID: 4624 (Account Logon)
  - Account Domain is FQDN & should be short domain name
  - Account Domain:        LAB.ADSECURITY.ORG   [ADSECLAB]

- Event ID: 4634 (Account Logoff)
  - Account Domain is blank & should be short domain name
  - Account Domain:        _______________   [ADSECLAB]

- Event ID: 4672 (Admin Logon)
  - Account Domain is blank & should be short domain name
  - Account Domain:        _______________   [ADSECLAB]

### Detecting on Endpoint

A blog [post](https://blog.stealthbits.com/detect-pass-the-ticket-attacks) writes about a useful way to detect pass the ticket attacks on an endpoint machine.

The steps layed out in the blog post are as follows:

1. Look at the current logon sessions on that system
    - A Powershell [Get-LoggedOnUsers](https://github.com/tmmtsmith/Powershell/blob/master/Get-LoggedOnUsers.ps1) function can be used to view current logged on users.
2. Use the klist command to inspect the Kerberos tickets associated with that session
    - ```klist -li NAME``` command can be used to get associated tickets.
3. Look for Kerberos tickets that do not match the user associated with the session.  If found, that means those were injected into memory and a pass-the-ticket attack is afoot.  
  
#### Example

The following shows at example. The first red rectangle is a session for a user Micheal. However, the second rectangle shows from the ```klist``` command that the TGT is for the user Gene.
![Detect Pass The Ticket Example](https://blog.stealthbits.com/wp-content/uploads/2019/02/image-28.png)

## Mitigation

### Logon Rights

There should be a minimum amount of Domain Administrators that have the right to logon to a DC.

### Golden Ticket Recovery

To recover from a Golden Ticket attack the KRBTGT user account password will have to be reset twice.

### Silver Ticket Recovery

To recover from a Silver Ticket attack the service account password will have to be reset.

## References

- [T1097](https://attack.mitre.org/techniques/T1097/)
- [How does Kerberos work? – Theory](https://www.tarlogic.com/en/blog/how-kerberos-works/)
- [How to attack Kerberos?](https://www.tarlogic.com/en/blog/how-to-attack-kerberos/)
- [Silver Ticket Attack](https://adsecurity.org/?p=2011)
- [Good Video on Kerberos Protocol](https://www.youtube.com/watch?v=WXgKiiFqJbI)
- [Detecting Forged Kerberos Ticket (Golden Ticket & Silver Ticket) Use in Active Directory](https://adsecurity.org/?p=1515)
- [Detect Pass the Ticket Attacks](https://blog.stealthbits.com/detect-pass-the-ticket-attacks)
