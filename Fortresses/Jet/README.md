# HackTheBox Fortress: Jet
## Offensive Security Writeup

---

## Overview

This Fortress focused on a multi service Linux host exposing custom applications and a legacy web stack. Initial reconnaissance identified DNS information disclosure, hidden web functionality, and multiple non standard network services.

Client side analysis exposed administrative endpoints which led to authentication bypass through SQL injection. Database enumeration enabled credential recovery and authenticated access to the application dashboard.

Sensitive data and flags are intentionally omitted.

---

## Threat Model Perspective

Assumed attacker profile:

- External user with no credentials
- Limited initial visibility into infrastructure
- Objective was escalation from anonymous access to administrative control

Primary goals:

- Expand attack surface through enumeration
- Identify weak trust boundaries
- Leverage application flaws for privilege escalation

---

## Reconnaissance

Initial enumeration exposed several services across both standard and high ports.

Key observations:

- Multiple SSH services suggested segmented roles.
- DNS responses exposed internal naming conventions.
- HTTP service presented a static landing page.
- Custom high ports indicated internally developed applications.

Reverse DNS enumeration revealed an internal hostname which expanded the web attack surface and enabled discovery of a secondary virtual host.

---

## Web Application Analysis

The newly discovered site appeared as a marketing interface with dynamic content.

Indicators worth deeper inspection:

- Live counters updated asynchronously
- External JavaScript loaded locally
- Obfuscated client side logic

Static analysis of the JavaScript uncovered encoded functions. Decoding the script revealed hidden administrative paths that were not indexed publicly.

This demonstrated a reliance on client side secrecy instead of server side access control.

---

## Administrative Endpoint Discovery

Navigating to the recovered directory exposed an administrative login panel.

Initial testing revealed:

- Response messages changed depending on input structure.
- Authentication behavior suggested direct database interaction.
- Proxy inspection confirmed raw parameters were passed to backend logic.

These indicators pointed toward improper input validation.

---

## Injection Identification

Testing focused on authentication parameters.

Behavior observed:

- Conditional inputs altered application responses.
- Timing differences suggested database execution paths.
- Error handling exposed backend query behavior.

Multiple injection techniques were identified during testing:

- Boolean based blind injection
- Error based injection
- Time based injection
- UNION based extraction

This indicated the absence of prepared statements within authentication queries.

---

## Database Enumeration

After confirming injection, database structure was explored.

Findings included:

- Dedicated administrative database
- User credential table
- Password hashes stored without adaptive hashing protections

Offline analysis of extracted hashes enabled credential recovery. Specific values are intentionally omitted.

---

## Authenticated Access

Recovered credentials granted administrative dashboard access.

Post authentication observations:

- Internal metrics and application data became visible.
- Additional hidden functionality was accessible through the interface.
- The application trusted authenticated users excessively.

This stage demonstrated a lack of role separation within the platform.

---

## Attack Path Summary

High level chain:

1. Network enumeration exposed DNS service.
2. DNS responses revealed internal hostname.
3. Client side JavaScript analysis exposed hidden admin paths.
4. Authentication workflow contained injectable parameters.
5. Database extraction enabled credential recovery.
6. Administrative dashboard access achieved.

Each phase relied on observable weaknesses rather than brute force techniques.

---

## Key Vulnerabilities Identified

- Information disclosure through DNS configuration
- Client side obfuscation exposing sensitive routes
- Lack of server side input validation
- SQL injection within authentication workflow
- Weak password storage practices
- Excessive administrative exposure

---

## Defensive Recommendations

- Implement parameterized queries for all database interactions.
- Avoid storing sensitive paths within client side scripts.
- Use adaptive password hashing with salting.
- Restrict DNS records to required responses only.
- Separate administrative interfaces from public services.
- Reduce verbose application feedback.

---

## Skills Demonstrated

- Service enumeration and attack surface expansion
- Client side code analysis and deobfuscation
- Web application exploitation methodology
- SQL injection validation and exploitation
- Credential recovery workflows
- Post authentication analysis

---

## Final Notes

This writeup focuses on methodology and learning outcomes. Flags, exploit strings, and sensitive values are intentionally removed to maintain responsible disclosure practices.
