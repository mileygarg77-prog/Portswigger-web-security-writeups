# Lab 03 — DOM XSS in document.write sink using source location.search

**Category:** Cross-Site Scripting (XSS)
**Difficulty:** Apprentice
**Reference:** https://portswigger.net/web-security/cross-site-scripting/dom-based

## Objective
Client-side JavaScript writes an untrusted URL query parameter directly into the page via `document.write`, without ever involving the server. Exploit this DOM-based sink.

## Vulnerability
Inline script reads a tracking/search image parameter from `location.search` and passes it straight into `document.write(...)`, e.g. to build an `<img src="...">` tag.

## Steps to Reproduce
1. View the page source / use the browser's debugger to find the vulnerable script reading `location.search`.
2. Craft a URL query value that breaks out of the `<img>` tag's `src` attribute and injects a new tag.

## Payload(s) Used
```
https://LAB-ID.web-security-academy.net/?search="><svg onload=alert(1)>
```

## Explanation
Because the value from `location.search` is written into the DOM via `document.write` with no encoding, closing the `src` attribute's quote (`"`) and the `<img>` tag (`>`) lets an entirely new `<svg onload=...>` element be injected, whose `onload` handler fires immediately and executes the attacker's JavaScript — entirely client-side, so it never touches server logs.

## Remediation
Avoid `document.write` with untrusted data; use safe DOM APIs (`textContent`, `createElement`) or a templating library that auto-escapes, and validate/encode any data taken from the URL before using it to build markup.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
