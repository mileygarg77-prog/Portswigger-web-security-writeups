# Lab 17 — Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped

**Category:** Cross-Site Scripting (XSS)
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/cross-site-scripting/contexts

## Objective
The search term is again reflected inside a JavaScript string in an inline `<script>` block, but this time the previous lab's trick is patched: angle brackets and double quotes are HTML-encoded, so `</script>` injection no longer works. Single quotes are escaped — but critically, the backslash character itself is **not** escaped.

## Vulnerability
The reflection looks like:
```html
<script>
  var searchTerm = 'YOUR INPUT';
  ...
</script>
```
The filter replaces every `'` in the input with `\'`, but it does nothing to a literal `\` the attacker supplies. Because `<`/`>`/`"` are HTML-encoded, breaking out via a new tag is blocked, so the only route left is JS-string escaping itself.

## Steps to Reproduce
1. Confirm `<`, `>`, and `"` are HTML-encoded in the reflection, ruling out the `</script>` tag-closing trick from the previous lab.
2. Confirm a submitted `'` comes back as `\'` — escaped, as expected.
3. Submit a payload that supplies your **own** leading backslash immediately before the quote you want to use to close the string.

## Payload(s) Used
```
\';alert(document.domain)//
```

## Explanation
The filter only escapes quote characters it finds in the *raw* input — it has no idea an attacker-supplied backslash is sitting right before one. When the filter walks the input `\';alert(document.domain)//` and escapes the `'`, it turns that single quote into `\'`, but the backslash the attacker already typed is left completely untouched. The final string the browser sees is:
```
var searchTerm = '\\';alert(document.domain)//';
```
JavaScript reads `\\` as a single literal backslash character (not an escape sequence), which means the following `'` is **not** escaped after all — it genuinely closes the string literal. Everything after that, `;alert(document.domain)`, runs as a real JavaScript statement, and the trailing `//` comments out the rest of the original line (including the closing `';` the application appends), so there's no syntax error.

## Screenshort
`c:\Users\MILEY\OneDrive\Pictures\Screenshots\Screenshot (850).png`

## Remediation
Never rely on a single escaping pass that assumes the attacker's input contains no backslashes of its own; escape backslashes *before* escaping quotes (so `\` → `\\` happens first), or better, avoid splicing user input into inline script at all — pass it through a safe JSON-encoding function or a `data-*` attribute read by the script instead.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
