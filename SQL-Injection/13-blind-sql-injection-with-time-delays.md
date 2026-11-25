# Lab 13 — Blind SQL injection with time delays

**Category:** SQL Injection
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/sql-injection/blind

## Objective
The response gives no visible or error-based signal at all. Confirm the injection exists purely by observing a time delay caused by an injected `SLEEP`.

## Vulnerability
The `TrackingId` cookie is injectable into a query whose result is never reflected or distinguished by status code, requiring a time-based blind technique.

## Steps to Reproduce
1. Inject a payload that unconditionally sleeps and measure response time to confirm the injection point.
2. (This lab only requires proving the vulnerability exists; later labs build on it to extract data.)

## Payload(s) Used
```
TrackingId=x' AND SLEEP(5)--
```

## Explanation
If the response takes roughly 5 seconds longer than a baseline request, the injected `SLEEP(5)` was executed by the database, confirming the `TrackingId` cookie is injectable even though there is no other observable difference in the application's behavior.

## Screenshots
_Add your screenshots here, e.g. `./screenshots/sqli-lab-13-01.png`_

## Remediation
Parameterized queries; also consider database-level restrictions/timeouts that limit the impact of long-running injected queries.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
