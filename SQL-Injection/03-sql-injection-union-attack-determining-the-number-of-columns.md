# Lab 03 — SQL injection UNION attack, determining the number of columns returned by the query

**Category:** SQL Injection
**Difficulty:** Apprentice
**Reference:** https://portswigger.net/web-security/sql-injection/union-attacks

## Objective
Determine the number of columns returned by the original SQL query so that a UNION-based attack can later be constructed.

## Vulnerability
The `category` parameter is injectable and the results of the query are rendered in the page, making it a candidate for a UNION attack once the column count is known.

## Steps to Reproduce
1. Use `ORDER BY` with an increasing column index until the request errors out, which reveals the column count. Or:
2. Use `UNION SELECT NULL` with an increasing number of `NULL`s until no database error is returned.

## Payload(s) Used
```
category=Gifts' ORDER BY 1--
category=Gifts' ORDER BY 2--
category=Gifts' ORDER BY 3--   (errors here)

-- or equivalently --
category=Gifts' UNION SELECT NULL--
category=Gifts' UNION SELECT NULL,NULL--
```

## Explanation
`ORDER BY` fails once the index exceeds the number of columns in the result set, so the first index that errors tells you the query has one fewer column. Similarly, a `UNION SELECT` with the wrong number of `NULL`s throws a column-count mismatch error; the first payload that succeeds reveals the correct column count (2, in this lab).

## Screenshots
_Add your screenshots here, e.g. `./screenshots/sqli-lab-03-01.png`_

## Remediation
Avoid exposing raw database errors to end users, and use parameterized queries so injected `ORDER BY`/`UNION` clauses are never executed as SQL.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
