# Lab 06 — SQL injection UNION attack, retrieving multiple values in a single column

**Category:** SQL Injection
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/sql-injection/union-attacks

## Objective
The query only returns one column capable of holding string data, but two pieces of information (username and password) need to be exfiltrated. Concatenate them into a single column.

## Vulnerability
Only one of the two UNION columns accepts text; the other errors if given a string.

## Steps to Reproduce
1. Determine which single column accepts strings (as in lab 04).
2. Concatenate `username` and `password` together with a separator in that one column, leaving the other column as `NULL`.

## Payload(s) Used
```
category=Gifts' UNION SELECT NULL, username || '~' || password FROM users--
```

## Explanation
`||` is the string concatenation operator used by this lab's backend (Oracle-style syntax; use `CONCAT()` or `+` on MySQL/MSSQL respectively). Combining both fields with a delimiter like `~` into the single text-capable column lets both values be exfiltrated even though only one column can render text.

## Remediation
Parameterized queries remove the injection point entirely, making column-count/type workarounds irrelevant.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
