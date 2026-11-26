# Lab 09 — Reflected XSS into a JavaScript string with angle brackets HTML encoded

**Category:** Cross-Site Scripting (XSS)
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/cross-site-scripting/contexts

## Objective
User input is reflected inside a JavaScript string literal embedded in a `<script>` block. Angle brackets are HTML-encoded, so the goal is to break out of the string using JS syntax instead of HTML syntax.

## Vulnerability
The page includes something like `<script>var searchTerm = 'YOUR INPUT'; ...</script>`, where only `<`/`>` are encoded, leaving quotes and backslashes untouched inside the JS context.

## Steps to Reproduce
1. View source to find the reflection sits inside a single-quoted JS string.
2. Close the string and the statement, then append a new JS statement that calls `alert()`, and comment out any trailing code if needed.

## Payload(s) Used
```
'-alert(1)-'
```

## Explanation
This payload doesn't need to close the string with a literal quote-then-semicolon; instead it uses the `-` (subtraction) operator to terminate the original string expression, run `alert(1)` as part of an arithmetic expression, and then re-open a dummy string with another `'` so the remainder of the original line remains syntactically valid JavaScript.

## Screenshots
`c:\Users\MILEY\OneDrive\Pictures\Screenshots\Screenshot (848).png`

## Remediation
Never interpolate untrusted data directly into inline `<script>` blocks; instead, pass data to JS via a JSON-encoded, properly escaped data attribute or a dedicated JS string-escaping function, and apply a strict CSP that disallows inline scripts.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
