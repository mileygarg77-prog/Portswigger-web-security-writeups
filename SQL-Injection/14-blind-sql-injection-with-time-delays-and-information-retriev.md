# Lab 14 — Blind SQL injection with time delays and information retrieval

**Category:** SQL Injection
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/sql-injection/blind

## Objective
Extend the time-based blind technique to actually extract data (the administrator's password) by making the delay conditional on a guessed character.

## Vulnerability
Same injectable `TrackingId` cookie as lab 13, now used to exfiltrate data rather than just confirm the bug.

## Steps to Reproduce
1. Build a conditional sleep: only sleep if the guessed character matches.
2. Script a binary search (via Burp Intruder or a custom script) over each character position of the password, timing each request.
3. Reconstruct the password from the characters that produced a delay, then log in as administrator.

## Payload(s) Used
```
TrackingId=x' AND (SELECT CASE WHEN SUBSTRING(password,1,1)='a' THEN SLEEP(5) ELSE 0 END FROM users WHERE username='administrator')--
```

## Explanation
Because the delay only occurs when the guessed character is correct, timing each request (correct guess ≈ baseline + 5s, incorrect guess ≈ baseline) turns the response-time channel into a full data-exfiltration oracle, letting the entire password be recovered one character at a time without ever seeing it reflected on the page.

## Screenshots
_Add your screenshots here, e.g. `./screenshots/sqli-lab-14-01.png`_

## Remediation
Parameterized queries; rate-limit or cap query execution time at the database layer to blunt time-based attacks even if an injection flaw exists.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
