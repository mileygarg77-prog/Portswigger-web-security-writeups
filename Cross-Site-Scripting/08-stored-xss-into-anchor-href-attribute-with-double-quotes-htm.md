# Lab 08 — Stored XSS into anchor href attribute with double quotes HTML-encoded

**Category:** Cross-Site Scripting (XSS)
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/cross-site-scripting/contexts

## Objective
A comment's 'website' field is stored and rendered inside an anchor's `href` attribute. Double quotes are encoded, but the value isn't otherwise validated, allowing a `javascript:` URI.

## Vulnerability
The website field is rendered as `<a href="WEBSITE">...</a>`; quotes are encoded (so you can't break out of the attribute) but the scheme of the URL itself is never validated.

## Steps to Reproduce
1. Post a comment and set the 'website' field to a `javascript:` URI instead of a normal URL.
2. View the resulting comment and click the generated link.

## Payload(s) Used
```
javascript:alert(document.cookie)
```

## Explanation
Since quote-escaping only stops attribute *breakout*, it does nothing to stop the attribute's *value* itself from being a dangerous URI scheme. Clicking the rendered link causes the browser to execute the `javascript:` payload instead of navigating anywhere, and because the comment is stored, this fires for every visitor who clicks the link.

## Screenshots
_Add your screenshots here, e.g. `./screenshots/xss-lab-08-01.png`. If you don't have a screenshot for this lab yet, leave this section as-is or replace with "Screenshot not available for this lab"._

## Remediation
Enforce an allow-list of acceptable URL schemes (`http`, `https`, `mailto`, etc.) for any user-supplied link, in addition to attribute encoding.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
