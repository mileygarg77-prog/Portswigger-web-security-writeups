# Lab 05 — DOM XSS in jQuery anchor href attribute sink using location.search source

**Category:** Cross-Site Scripting (XSS)
**Difficulty:** Apprentice
**Reference:** https://portswigger.net/web-security/cross-site-scripting/dom-based

## Objective
A 'back' link's `href` is built client-side from a URL parameter using jQuery, allowing a `javascript:` URI injection.

## Vulnerability
jQuery code such as `$('#backLink').attr('href', new URLSearchParams(location.search).get('returnUrl'))` sets an anchor's `href` directly from untrusted input.

## Steps to Reproduce
1. Find the back-link element and the script that sets its `href` from `returnUrl`.
2. Set `returnUrl` to a `javascript:` pseudo-URI.
3. Click the resulting link to trigger execution.

## Payload(s) Used
```
https://LAB-ID.web-security-academy.net/#returnUrl=javascript:alert(1)
```

## Explanation
Because the `href` attribute accepts the `javascript:` scheme and the app never validates the URL's scheme before assigning it, clicking the manipulated link causes the browser to execute the JavaScript after `javascript:` instead of navigating to a page.

## Screenshots
_Add your screenshots here, e.g. `./screenshots/xss-lab-05-01.png`. If you don't have a screenshot for this lab yet, leave this section as-is or replace with "Screenshot not available for this lab"._

## Remediation
Validate that user-supplied URLs use an allow-listed scheme (e.g. only `http:`/`https:`) before using them in `href`, and prefer safe DOM APIs over raw attribute assignment from untrusted sources.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
