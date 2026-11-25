# Lab 08 — SQL injection attack, querying the database type and version on MySQL and Microsoft

**Category:** SQL Injection
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/sql-injection/examining-the-database

## Objective
Use SQL injection to determine the database type and version on a MySQL or Microsoft SQL Server backend.

## Vulnerability
Injectable `category` parameter; both MySQL and MSSQL expose version info via the `@@version` global variable.

## Steps to Reproduce
1. Confirm the column count and text-capable column as before.
2. Select `@@version` into the text-capable column.

## Payload(s) Used
```
category=Gifts' UNION SELECT @@version, NULL--
```

## Explanation
`@@version` is a built-in variable on both MySQL and Microsoft SQL Server that returns a descriptive version string, immediately revealing the DBMS type and exact version without needing to guess.

## Screenshots
_Add your screenshots here, e.g. `./screenshots/sqli-lab-08-01.png`_

## Remediation
Avoid leaking database errors/metadata to clients; parameterized queries prevent the injection outright.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
