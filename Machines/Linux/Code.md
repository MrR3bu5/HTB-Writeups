# HackTheBox Machine: Code
## Linux Offensive Security Writeup

---

## Overview

Code presented a Python based web application exposing an online code editor. The platform attempted to restrict execution but failed to properly sandbox Python runtime behavior. Exploitation allowed command execution, credential extraction, lateral movement, and privilege escalation.

Sensitive data and flags are intentionally removed.

---

## Reconnaissance

Service enumeration identified:

- SSH
- Gunicorn based web application

The application allowed user supplied Python execution within a restricted environment. :contentReference[oaicite:3]{index=3}  

This immediately suggested sandbox escape testing.

---

## Sandbox Escape

Testing revealed access to internal Python objects through globals.

By referencing internal modules indirectly, system level commands were executed.

This demonstrated incomplete sandbox isolation.

---

## Foothold

Command execution allowed:

- Reverse shell establishment
- Enumeration of application files
- Discovery of internal SQLite database

Database analysis revealed user credential hashes.

---

## Credential Abuse

Recovered hashes were cracked offline, revealing valid user credentials.

SSH access was obtained using recovered authentication material.

This pivot moved access from application context to system user context.

---

## Privilege Escalation

Local enumeration revealed a script executable through sudo without a password.

Reviewing the script behavior exposed an opportunity to manipulate backup tasks and execute commands with elevated privileges.

Root access was achieved through controlled task execution.

---

## Attack Path Summary

1. Identified insecure Python execution environment.
2. Escaped sandbox through globals access.
3. Retrieved database credentials.
4. Pivoted to SSH user access.
5. Abused misconfigured sudo script for privilege escalation.

---

## Key Vulnerabilities Identified

- Insecure Python sandbox implementation
- Weak credential storage
- Overly permissive sudo configuration
- Improper separation between application and system privileges

---

## Defensive Recommendations

- Implement strict runtime sandboxing.
- Avoid exposing interpreter internals.
- Secure database credentials with stronger hashing.
- Restrict sudo execution scope.

---

## Skills Demonstrated

- Python sandbox bypass techniques
- Web application exploitation
- Credential harvesting
- Linux privilege escalation
