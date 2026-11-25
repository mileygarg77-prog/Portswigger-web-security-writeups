# Lab 05 — SQL injection UNION attack, retrieving data from other tables

**Category:** SQL Injection
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/sql-injection/union-attacks

## Objective
Use a UNION attack to retrieve the `username` and `password` columns from the `users` table, given their names, and use the leaked administrator credentials to log in.

## Vulnerability
Injectable `category` parameter with a confirmed 2-column, string-capable result set (per labs 03/04).

## Steps to Reproduce
1. Confirm column count (2) and the string-compatible column, as in the previous labs.
2. Inject a UNION SELECT that pulls `username` and `password` from the `users` table into those two columns.
3. Read the leaked credentials off the rendered product listing page.
4. Log in as `administrator` using the leaked password.

## Payload(s) Used
```
category=Gifts' UNION SELECT username, password FROM users--
```

## Explanation
Because the application just concatenates the two `UNION SELECT` columns into the product listing, injecting `username, password` instead of literal strings causes every row of the `users` table to be rendered on the page, directly exposing stored credentials.

## Screenshots
_Add your screenshots here, e.g. `./screenshots/sqli-lab-05-01.png`_

## Remediation
Parameterized queries; also apply least-privilege database accounts so the web app's DB user cannot read tables (like `users`) it has no legitimate need to query.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
