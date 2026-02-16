# HackTheBox Machine: Devel
## Windows Offensive Security Writeup

---

## Overview

Devel presented a Windows IIS environment exposing FTP and HTTP services. Anonymous FTP access allowed file upload into a web accessible directory, enabling remote code execution through a web shell. Local privilege escalation led to SYSTEM level access.

Sensitive values and flags are intentionally omitted.

---

## Initial Reconnaissance

Enumeration revealed:

- FTP with anonymous login enabled
- IIS web server
- TRACE HTTP method allowed

Anonymous access exposed the web root directory structure. :contentReference[oaicite:4]{index=4}  

This indicated a likely upload based attack path.

---

## Foothold

FTP allowed uploading files directly into a directory served by IIS.

A server executable script was uploaded and triggered through the web interface, resulting in remote shell access under a service account.

This confirmed improper separation between FTP storage and web execution paths.

---

## Post Exploitation

Local enumeration revealed:

- Outdated Windows kernel
- Service account execution context

System architecture analysis identified a known privilege escalation path.

---

## Privilege Escalation

A local exploit targeting the vulnerable kernel enabled SYSTEM level execution.

Once elevated:

- Administrative directories became accessible
- Full control of the host was achieved

---

## Attack Path Summary

1. Identified anonymous FTP access.
2. Uploaded executable script to web directory.
3. Triggered remote shell via IIS.
4. Enumerated local system.
5. Performed kernel privilege escalation.

---

## Key Vulnerabilities Identified

- Anonymous FTP access
- Shared FTP and web execution directory
- Outdated Windows kernel
- Excessive web service permissions

---

## Defensive Recommendations

- Disable anonymous FTP.
- Separate upload directories from execution paths.
- Patch operating system regularly.
- Restrict web server permissions.

---

## Skills Demonstrated

- FTP abuse and web shell deployment
- IIS exploitation workflow
- Windows privilege escalation methodology
