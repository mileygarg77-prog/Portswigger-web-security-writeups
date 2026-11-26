# Lab 07 — SQL injection attack, querying the database type and version on Oracle

**Category:** SQL Injection
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/sql-injection/examining-the-database

## Objective
Use SQL injection to display the database version string on an Oracle backend.

## Vulnerability
Injectable `category` parameter; Oracle requires selecting from the pseudo-table `dual` and exposes version data via `v$version`.

## Steps to Reproduce
1. Confirm the column count and text-capable column as before.
2. Query the version banner from Oracle's internal `v$version` table.

## Payload(s) Used
```
category=Gifts' UNION SELECT banner, NULL FROM v$version--
```

## Explanation
Oracle doesn't support a version function callable without `FROM`, so the version string must be pulled from the `v$version` system view. The returned `banner` column reveals the exact Oracle release in use.

## Remediation
Restrict the database account's privileges so it cannot query system/metadata views, in addition to fixing the injection with parameterized queries.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
