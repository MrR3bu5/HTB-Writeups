# HackTheBox Machine: Expressway
## Linux Offensive Security Writeup

---

## Overview

Expressway focused on network level enumeration rather than traditional web exploitation. Initial recon revealed VPN related services which exposed authentication metadata. Weak pre-shared key handling enabled access expansion and eventual system level interaction.

Sensitive values and flags are intentionally removed.

---

## Reconnaissance

Service discovery exposed SSH and VPN related functionality.

IKE enumeration revealed aggressive mode responses containing user identity data.  
According to reconnaissance output, the handshake exposed an identifiable username which expanded the attack path. :contentReference[oaicite:0]{index=0}  

Key insight:

Network protocols often leak more information than web services.

---

## VPN Enumeration

IKE scanning identified:

- Encryption parameters
- Authentication method
- User identity field

The aggressive mode exchange exposed material that allowed offline analysis of authentication data.

This shifted the engagement from host exploitation to credential targeting.

---

## Credential Recovery

Analysis of captured authentication data enabled recovery of a valid access secret.

With valid credentials, authenticated access to the system was achieved.

This demonstrated the risk of aggressive mode VPN configurations combined with weak secrets.

---

## Attack Path Summary

1. Identified VPN service during recon.
2. Captured aggressive mode handshake.
3. Extracted user identity information.
4. Recovered authentication secret through offline analysis.
5. Established authenticated system access.

---

## Key Vulnerabilities Identified

- Exposure of aggressive mode VPN handshake
- Weak pre-shared key management
- Information disclosure through protocol negotiation

---

## Defensive Recommendations

- Disable aggressive mode where possible.
- Enforce strong PSK policies.
- Restrict identity disclosure during authentication.
- Monitor abnormal IKE enumeration activity.

---

## Skills Demonstrated

- Network protocol analysis
- VPN enumeration techniques
- Offline credential recovery
- Authentication abuse workflows
