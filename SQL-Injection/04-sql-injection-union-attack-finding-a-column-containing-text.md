# Lab 04 — SQL injection UNION attack, finding a column containing text

**Category:** SQL Injection
**Difficulty:** Apprentice
**Reference:** https://portswigger.net/web-security/sql-injection/union-attacks

## Objective
Having established the query returns two columns, find which of those columns can hold string data so arbitrary text (and later other tables' data) can be injected into the page output.

## Vulnerability
Same injectable `category` parameter; some columns in the underlying table are numeric and will throw a type-conversion error if a string is placed there in a UNION SELECT.

## Steps to Reproduce
1. Submit a UNION SELECT with a string literal in place of each NULL, one column at a time, to see which one doesn't error and is reflected in the response.

## Payload(s) Used
```
category=Gifts' UNION SELECT 'a',NULL--
category=Gifts' UNION SELECT NULL,'a'--
```

## Explanation
The payload that returns HTTP 200 and displays the literal `a` on the page identifies the column that accepts and displays string data. That column is the one to use for exfiltrating text (usernames, passwords, table/column names) in subsequent UNION attacks.

## Remediation
Same as other injection labs: parameterized queries eliminate the ability to inject UNION clauses at all.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
