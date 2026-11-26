# Lab 12 — Blind SQL injection with conditional errors

**Category:** SQL Injection
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/sql-injection/blind

## Objective
There is no visible difference in the response for true/false conditions, but a malformed/erroring query returns an HTTP 500. Use a conditional error (e.g. divide-by-zero) as the oracle instead.

## Vulnerability
The `TrackingId` cookie feeds an injectable query; the application returns a generic error page whenever the underlying SQL query throws an exception, which can be triggered conditionally.

## Steps to Reproduce
1. Confirm the error oracle: a payload that always errors (e.g. forcing 1/0) returns HTTP 500; one that never errors returns 200.
2. Make the error conditional on a guess, so the error only fires when the guess is correct (or incorrect, depending on how it's phrased).
3. Binary-search the administrator's password length and then each character, using presence/absence of the 500 error as the signal.

## Payload(s) Used
```
TrackingId=x' AND (SELECT CASE WHEN (username='administrator' AND SUBSTRING(password,1,1)='a') THEN 1/0 ELSE 1 END FROM users)--
```

## Explanation
The `CASE ... THEN 1/0` construct only triggers a division-by-zero database error when the inner condition evaluates to TRUE. Since the app surfaces that error as an HTTP 500 but shows 200 otherwise, the presence of a 500 response becomes a reliable one-bit oracle for each guessed condition.

## Remediation
Never surface raw database errors differently based on user input; use parameterized queries and generic error handling.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
