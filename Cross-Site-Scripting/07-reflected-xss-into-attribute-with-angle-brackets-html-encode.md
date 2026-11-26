# Lab 07 — Reflected XSS into attribute with angle brackets HTML-encoded

**Category:** Cross-Site Scripting (XSS)
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/cross-site-scripting/contexts

## Objective
User input is reflected inside an HTML attribute value; angle brackets are encoded so a new tag can't be injected directly, but the attribute itself can be broken out of.

## Vulnerability
The search term is reflected as `<input value="YOUR INPUT">`, with `<` and `>` HTML-encoded but the double quote left unescaped.

## Steps to Reproduce
1. View source to confirm the reflection point is inside an attribute's value and that `<`/`>` are encoded but `"` is not.
2. Inject a payload that closes the `value` attribute and adds a new event-handler attribute to the same tag.

## Payload(s) Used
```
" onmouseover="alert(1)
```

## Explanation
Because the double quote isn't encoded, the payload closes the existing `value="..."` attribute early and appends a brand-new `onmouseover` attribute onto the same `<input>` tag, which fires JavaScript as soon as the victim's mouse moves over the field — all without ever needing a literal `<script>` tag.

## Screenshots
`c:\Users\MILEY\OneDrive\Pictures\Screenshots\Screenshot (845).png`

## Remediation
HTML-attribute-encode *all* characters that are meaningful in that context, including quotes, not just angle brackets; prefer a vetted auto-escaping templating engine over manual encoding.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
