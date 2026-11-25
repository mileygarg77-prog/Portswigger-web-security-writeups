# Lab 06 — DOM XSS in jQuery selector sink using a hashchange event

**Category:** Cross-Site Scripting (XSS)
**Difficulty:** Apprentice
**Reference:** https://portswigger.net/web-security/cross-site-scripting/dom-based

## Objective
A client-side script listens for URL hash changes and passes the hash fragment into a jQuery selector, which can be abused to inject HTML.

## Vulnerability
Code resembling `$(window).on('hashchange', function(){ var post = $(location.hash); ... })` passes the raw hash value into jQuery's `$()` selector/sink, which can create DOM elements from certain strings.

## Steps to Reproduce
1. Identify the `hashchange` handler and that it funnels `location.hash` into a jQuery selector.
2. Navigate to the page with a crafted hash containing an HTML tag with an event handler, then trigger the hash-change (e.g. by reloading with the fragment present, or via a second hash navigation).

## Payload(s) Used
```
https://LAB-ID.web-security-academy.net/#<img src=1 onerror=alert(1)>
```

## Explanation
When passed a string that looks like an HTML tag, jQuery's `$()` function will construct and (depending on version/sink) insert that element into the document rather than treating it purely as a CSS selector, so the crafted `<img onerror=...>` tag executes the attacker's script once the hash change handler runs.

## Screenshots
_Add your screenshots here, e.g. `./screenshots/xss-lab-06-01.png`. If you don't have a screenshot for this lab yet, leave this section as-is or replace with "Screenshot not available for this lab"._

## Remediation
Never feed untrusted, attacker-controlled strings into jQuery's `$()`; validate/escape hash values, and keep jQuery updated since this exact selector/HTML ambiguity has been hardened in newer releases.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
