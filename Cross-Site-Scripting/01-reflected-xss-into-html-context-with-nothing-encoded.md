# Lab 01 — Reflected XSS into HTML context with nothing encoded

**Category:** Cross-Site Scripting (XSS)
**Difficulty:** Apprentice
**Reference:** https://portswigger.net/web-security/cross-site-scripting/reflected

## Objective
A search box reflects user input directly into the page's HTML with no encoding at all. Inject a script that pops an alert dialog.

## Vulnerability
The `search` query parameter is echoed verbatim into the HTML body of the results page: `<h1>0 search results for '<search term>'</h1>`.

## Steps to Reproduce
1. Enter a test string in the search box and view source to confirm it's reflected unencoded.
2. Submit a `<script>` payload instead of a search term.

## Payload(s) Used
```
<script>alert(1)</script>
```

## Explanation
Because the application performs no HTML-encoding on the reflected value, the browser parses the injected `<script>` tag as real markup and executes its contents, firing the alert and proving arbitrary JavaScript execution in the victim's browser context.

## Screenshots
_Add your screenshots here, e.g. `./screenshots/xss-lab-01-01.png`. If you don't have a screenshot for this lab yet, leave this section as-is or replace with "Screenshot not available for this lab"._

## Remediation
Context-aware output encoding (HTML-entity-encode `<`, `>`, `&`, quotes) for any user input reflected into HTML, plus a restrictive Content-Security-Policy.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
