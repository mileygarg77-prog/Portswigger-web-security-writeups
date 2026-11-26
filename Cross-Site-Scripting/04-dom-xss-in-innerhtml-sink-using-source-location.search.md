# Lab 04 — DOM XSS in innerHTML sink using source location.search

**Category:** Cross-Site Scripting (XSS)
**Difficulty:** Apprentice
**Reference:** https://portswigger.net/web-security/cross-site-scripting/dom-based

## Objective
A client-side search feature reads a URL parameter and assigns it to an element's `innerHTML` to display 'no results' messages. Exploit this sink.

## Vulnerability
JavaScript similar to `element.innerHTML = 'No results for ' + new URLSearchParams(location.search).get('search')` writes untrusted input straight into the DOM via `innerHTML`.

## Steps to Reproduce
1. Identify the vulnerable sink in the page's JS (search for `.innerHTML =`).
2. Supply a URL parameter containing an HTML tag with an event handler that needs no user click, such as `<img>`'s `onerror`.

## Payload(s) Used
```
https://LAB-ID.web-security-academy.net/?search=<img src=1 onerror=alert(1)>
```

## Explanation
`innerHTML` parses its assigned string as HTML, so injecting an `<img>` tag with an invalid `src` (`1`) guarantees the `onerror` event fires, executing the attacker's script the instant the page renders — no further interaction needed.

## Remediation
Never assign untrusted strings to `innerHTML`; use `textContent` for plain text or a sanitization library (e.g. DOMPurify) if HTML must be rendered dynamically.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
