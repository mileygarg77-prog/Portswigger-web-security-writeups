# Lab 10 — DOM XSS in document.write sink using source location.search inside a select element

**Category:** Cross-Site Scripting (XSS)
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/cross-site-scripting/dom-based

## Objective
A product page uses `document.write` to populate a `<select>` dropdown's `<option>` values from a URL parameter (e.g. `storeId`), inside a context where angle brackets may be filtered.

## Vulnerability
Client-side script builds `<option value='STOREID'>` via `document.write` using the raw `storeId` parameter, inside an attribute context nested within a `<select>` element.

## Steps to Reproduce
1. Identify the sink and that it writes the parameter into an `<option>` tag's `value` attribute inside a `document.write` call.
2. Break out of the attribute and the `<select>`/`<option>` context, then inject a new element with an event handler.

## Payload(s) Used
```
https://LAB-ID.web-security-academy.net/product?productId=1&storeId='-alert(1)-'
```

## Explanation
As in the JS-string case, using `'-alert(1)-'` breaks out of the single-quoted JavaScript string that `document.write` is building (rather than trying to break raw HTML), executing `alert(1)` as part of the script's own expression evaluation before `document.write` ever renders the final markup.

## Remediation
Avoid building dynamic markup via `document.write`; validate `storeId` against a known allow-list of valid store identifiers server- or client-side before using it at all.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
