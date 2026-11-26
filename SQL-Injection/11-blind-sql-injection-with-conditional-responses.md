# Lab 11 — Blind SQL injection with conditional responses

**Category:** SQL Injection
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/sql-injection/blind

## Objective
The injection point (a `TrackingId` cookie) is not reflected in the response, but the page shows a 'Welcome back' message only when a tracking identifier matching a real user exists. Use this boolean oracle to extract the administrator's password one character at a time.

## Vulnerability
The `TrackingId` cookie value is used unsanitized in a backend query whose boolean result only changes the presence/absence of a 'Welcome back' banner — a classic blind/boolean-based SQLi oracle.

## Steps to Reproduce
1. Confirm the oracle: a condition that's TRUE shows 'Welcome back'; FALSE does not.
2. Confirm the administrator user exists: `' AND (SELECT username FROM users WHERE username='administrator')='administrator'--`
3. Binary-search the password length: `' AND (SELECT LENGTH(password) FROM users WHERE username='administrator')>N--` for increasing/decreasing N.
4. Binary-search each character of the password using `SUBSTRING`.

## Payload(s) Used
```
TrackingId=x' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a'--
```

## Explanation
Because no data is ever reflected, the only signal available is whether the 'Welcome back' message appears. By wrapping a guess in a boolean condition and observing which guesses flip that signal, the full password can be reconstructed character-by-character via a scripted binary search (e.g. with Burp Intruder's cluster bomb/sniper attack and a grep-match rule on 'Welcome back').

## Remediation
Parameterized queries; also avoid conditionally changing visible application behavior based on raw query results tied to user-controllable input.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
