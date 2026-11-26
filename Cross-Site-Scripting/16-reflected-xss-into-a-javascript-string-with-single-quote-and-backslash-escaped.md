# Lab 16 — Reflected XSS into a JavaScript string with single quote and backslash escaped

**Category:** Cross-Site Scripting (XSS)
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/cross-site-scripting/contexts

## Objective
The search term is reflected inside a JavaScript string inside an inline `<script>` block. The application escapes single quotes (`'`) and backslashes (`\`) so the string itself can't be broken out of directly. The goal is still to get `alert(1)` to execute.

## Vulnerability
The page includes something like:
```html
<script>
  var searchTerm = 'YOUR INPUT';
  ...
</script>
```
Both `'` and `\` in the input are backslash-escaped before being inserted, so a naive `'-alert(1)-'`-style breakout doesn't work here. However, the application does **not** encode angle brackets (`<` / `>`) in this context, because it assumes the value only ever lands inside the JS string, not in raw HTML.

## Steps to Reproduce
1. Confirm that `'` and `\` are escaped by submitting a search term containing both and viewing the reflected JS source.
2. Confirm that `<` and `>` are reflected unescaped (since the filter only targets JS-string-breakout characters).
3. Instead of trying to break out of the string, close the entire `<script>` tag with `</script>` at the HTML level, then open a brand new `<script>` tag containing the payload.

## Payload(s) Used
```
</script><script>alert(1)</script>
```

## Explanation
Escaping `'` and `\` only prevents breaking out of the *JavaScript string literal* — it does nothing to stop the surrounding HTML parser from seeing a literal `</script>` tag inside the reflected value. The browser's HTML parser closes the original `<script>` block as soon as it encounters `</script>`, regardless of what JavaScript thought it was doing with escaped quotes. A fresh `<script>alert(1)</script>` tag then starts clean, outside the original (now-broken) string context entirely, and executes without needing to touch a single quote or backslash at all.

## Screenshots
`c:\Users\MILEY\OneDrive\Pictures\Screenshots\Screenshot (847).png`

## Remediation
Encode `<` and `>` (not just quotes/backslashes) wherever user input is reflected inside or near a `<script>` block, and prefer passing dynamic data to scripts via a separate JSON-encoded `<script type="application/json">` data island or a `data-*` attribute rather than splicing it into executable JS source.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
