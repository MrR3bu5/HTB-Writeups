# HackTheBox Machine: DarkZero
## Windows Active Directory Offensive Security Writeup

---

## Overview

DarkZero presented an Active Directory environment centered around a Windows Server domain controller. Enumeration revealed LDAP, SMB, Kerberos, and MSSQL services. The attack path leveraged domain credentials, SQL Server abuse, remote command execution, and local privilege escalation to achieve SYSTEM access.

Sensitive values and flags are intentionally removed.

---

## Initial Reconnaissance

Port scanning revealed a full Active Directory stack including:

- LDAP and LDAPS services
- Kerberos authentication
- SMB shares
- Microsoft SQL Server

The presence of SQL Server alongside domain services suggested potential lateral movement through database functionality. :contentReference[oaicite:4]{index=4}  

---

## Domain Enumeration

Authenticated enumeration began using known credentials.

Key actions included:

- SMB share enumeration
- SYSVOL inspection
- BloodHound data collection

Directory analysis revealed standard domain structure and potential privilege relationships within the environment. :contentReference[oaicite:5]{index=5}  

---

## SQL Server Exploitation

Access to the SQL service enabled command execution testing.

Indicators of compromise path:

- Linked server functionality allowed remote execution context.
- xp_cmdshell functionality exposed system level command execution.
- Execution context revealed service account privileges.

This phase transformed domain user access into system-level interaction on a secondary host.

---

## Remote Access and Shell Acquisition

Command execution confirmed network reachability to attacker infrastructure.

A reverse payload was introduced to establish a stable session.

Once active, the session confirmed access under a service account context.

---

## Privilege Escalation

Local enumeration identified a vulnerable Windows Server configuration.

A local privilege escalation technique allowed token manipulation and SYSTEM access.

After successful escalation:

- Administrative directories became accessible.
- Full system control was achieved.

This phase demonstrated exploitation of post-compromise Windows weaknesses rather than initial access flaws. :contentReference[oaicite:6]{index=6}  

---

## Attack Path Summary

1. Enumerated Active Directory services.
2. Authenticated to domain resources.
3. Leveraged MSSQL linked server execution.
4. Achieved remote shell through SQL execution.
5. Performed local privilege escalation.
6. Obtained SYSTEM-level access.

---

## Key Vulnerabilities Identified

- Overly permissive SQL Server execution context
- Exposure of linked server functionality
- Insufficient service account restrictions
- Local privilege escalation opportunity

---

## Defensive Recommendations

- Disable xp_cmdshell where not required.
- Restrict SQL linked server execution privileges.
- Implement least privilege service accounts.
- Apply timely patches to prevent local escalation exploits.
- Monitor unusual SQL execution patterns.

---

## Skills Demonstrated

- Active Directory reconnaissance
- SMB and LDAP enumeration
- SQL Server exploitation
- Reverse shell deployment
- Windows privilege escalation workflows

