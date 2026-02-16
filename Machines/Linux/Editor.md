# HackTheBox Machine: Editor
## Linux Offensive Security Writeup

---

## Overview

Editor exposed a web based documentation platform backed by XWiki. Enumeration revealed a vulnerable service running on a secondary port. Public vulnerability research enabled remote code execution which led to shell access and privilege escalation.

Sensitive values and flags are intentionally omitted.

---

## Initial Reconnaissance

Port scanning identified:

- SSH
- nginx web service
- Jetty instance hosting XWiki

The robots configuration and WebDAV methods indicated expanded functionality within the wiki service. :contentReference[oaicite:1]{index=1}  

This shifted focus toward application level vulnerabilities rather than brute force enumeration.

---

## Application Analysis

Version identification revealed a vulnerable XWiki deployment.

Research showed multiple publicly documented issues affecting the detected version.  
According to vulnerability research screenshots, remote code execution was possible through macro abuse within the platform. :contentReference[oaicite:2]{index=2}  

---

## Remote Code Execution

Testing confirmed that the platform allowed execution of crafted payloads.

Indicators included:

- External network callbacks
- Server side command execution
- Interactive shell access

This established the initial foothold.

---

## Post Exploitation

Once shell access was obtained:

- Local directories were enumerated.
- Application configuration was reviewed.
- User level access was achieved.

Privilege escalation followed through system level misconfiguration.

---

## Attack Path Summary

1. Enumerated web services and identified XWiki.
2. Mapped version to known vulnerabilities.
3. Achieved remote code execution.
4. Established interactive shell.
5. Performed local privilege escalation.

---

## Key Vulnerabilities Identified

- Outdated XWiki deployment
- Dangerous WebDAV methods exposed
- Improper access control within wiki macros

---

## Defensive Recommendations

- Patch third party platforms promptly.
- Disable unnecessary HTTP methods.
- Restrict execution rights within wiki environments.
- Monitor unusual macro activity.

---

## Skills Demonstrated

- Web application enumeration
- CVE mapping and exploit validation
- Remote command execution
- Linux post exploitation workflow
