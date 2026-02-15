# HackTheBox Machine: Cobblestone
## Linux Offensive Security Writeup

---

## Overview

Cobblestone presented a web-focused Linux target exposing multiple virtual hosts and a custom application stack. Enumeration revealed subdomain expansion, client-side vulnerabilities, and backend injection flaws that enabled remote code execution and credential recovery.

The attack path progressed from web exploitation to authenticated SSH access, followed by privilege escalation through an internal XML-RPC service.

Sensitive values and flags are intentionally omitted.

---

## Initial Reconnaissance

Service enumeration identified:

- SSH service running on Debian
- Apache web server hosting a dynamic application

Host discovery revealed multiple virtual hosts tied to the target environment. :contentReference[oaicite:0]{index=0}  

Key takeaway:

The presence of several subdomains suggested a segmented web architecture and increased attack surface.

---

## Web Enumeration

Directory brute forcing identified application resources and a database directory.

Further analysis of the main website revealed references to additional subdomains including deployment and voting services. :contentReference[oaicite:1]{index=1}  

Subdomain fuzzing exposed an alternative service that became the primary entry point.

---

## Application Analysis

Inspection of the voting application revealed:

- Reflected client input behavior
- Message rendering functionality
- Potential cross-site scripting vectors

Testing confirmed the application accepted unsanitized user content. This behavior indicated weak server-side validation.

---

## Injection and Remote Code Execution

Further testing uncovered backend injection capabilities within application parameters.

Indicators included:

- Direct database interaction through user input
- Ability to write files to web directories
- Command execution through uploaded scripts

A web shell was achieved through database abuse and exposed an interactive execution surface. :contentReference[oaicite:2]{index=2}  

This stage transitioned access from web-level interaction to system-level control.

---

## Credential Discovery and Lateral Movement

Once command execution was obtained:

- Local application directories were enumerated.
- Configuration files exposed database credentials.
- Database access allowed extraction of user hashes.

Offline analysis of credential material enabled SSH authentication.

This pivot moved access from web execution to a stable user shell.

---

## Privilege Escalation

Local enumeration identified an internal service bound to localhost.

Key observations:

- XML-RPC endpoint exposed privileged functionality.
- Service required port forwarding for external interaction.
- Custom method calls allowed sensitive file access.

Using this service, elevated access was achieved through internal API misuse. :contentReference[oaicite:3]{index=3}  

---

## Attack Path Summary

1. Enumerated virtual hosts and web application structure.
2. Identified vulnerable voting service.
3. Achieved remote code execution via injection.
4. Extracted database credentials from configuration files.
5. Pivoted to SSH using recovered credentials.
6. Abused internal XML-RPC service for privilege escalation.

---

## Key Vulnerabilities Identified

- Insufficient input validation in web application
- Backend injection allowing file creation
- Sensitive credentials stored in plaintext configuration
- Internal services accessible without proper restrictions

---

## Defensive Recommendations

- Implement strict server-side input validation.
- Use prepared database queries.
- Restrict file write permissions for web services.
- Protect internal APIs behind authentication and network segmentation.
- Store credentials securely using environment isolation.

---

## Skills Demonstrated

- Subdomain enumeration and web recon
- XSS and injection discovery
- Remote command execution workflows
- Credential harvesting
- Linux privilege escalation methodology
