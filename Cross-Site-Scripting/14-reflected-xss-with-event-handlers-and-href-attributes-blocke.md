# Lab 14 — Reflected XSS with event handlers and href attributes blocked

**Category:** Cross-Site Scripting (XSS)
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/cross-site-scripting/contexts

## Objective
A filter strips any `on*` event-handler attributes and `href` attributes. Find a tag/attribute combination that still executes JavaScript without either.

## Vulnerability
Blocklist-based sanitization specifically targets `on\w+=` patterns and `href=`, but fails to block other equally dangerous attributes like SVG/`<details>`'s `ontoggle`, or doesn't fully cover case variation/attribute aliasing.

## Steps to Reproduce
1. Test which attributes/tags survive the filter.
2. Use the `<details>` element's `ontoggle` attribute (frequently overlooked by filters focused on `on(click|load|error|mouseover)`), or any other non-blocked event attribute the lab's filter misses.

## Payload(s) Used
```
<details open ontoggle=alert(1)>
```

## Explanation
`ontoggle` is a legitimate HTML event attribute but far less commonly included in blocklists than `onclick`/`onerror`/`onload`. Setting `open` on the `<details>` element causes the `toggle` event — and therefore the attacker's script — to fire automatically as soon as the element renders, with no `href` or blocked `on*` keyword matched by a naive filter.

## Remediation
Use an allow-list sanitizer scoped to a minimal safe tag/attribute set rather than trying to blocklist dangerous attribute names, since HTML has dozens of event attributes beyond the common ones.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
