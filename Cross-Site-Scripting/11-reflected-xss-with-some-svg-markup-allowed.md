# Lab 11 — Reflected XSS with some SVG markup allowed

**Category:** Cross-Site Scripting (XSS)
**Difficulty:** Practitioner
**Reference:** https://portswigger.net/web-security/cross-site-scripting/contexts

## Objective
A filter strips `<script>` tags and certain common XSS vectors, but still allows some SVG elements and attributes through. Find an SVG-based vector that survives the filter.

## Vulnerability
Input sanitization blocks obvious tags/keywords (e.g. `<script>`, `onerror`) but does not account for the full breadth of SVG animation elements that can execute script-equivalent behavior.

## Steps to Reproduce
1. Submit a few common payloads to observe which tags/attributes are stripped.
2. Use the SVG `<animatetransform>` (or similar animate element) with an `onbegin` handler, which is often missed by naive blocklists.

## Payload(s) Used
```
<svg><animatetransform onbegin=alert(1) attributeName=transform dur=1s>
```

## Explanation
Blocklist-based filters typically target well-known XSS sinks like `onerror`/`onload` on `<img>`/`<svg>` but overlook SVG's animation-related event attributes such as `onbegin` on `<animatetransform>`, which fires as soon as the animation starts, giving an alternate path to script execution that bypasses the filter entirely.

## Screenshots
_Add your screenshots here, e.g. `./screenshots/xss-lab-11-01.png`. If you don't have a screenshot for this lab yet, leave this section as-is or replace with "Screenshot not available for this lab"._

## Remediation
Replace blocklist filtering with an allow-list HTML/SVG sanitizer (e.g. DOMPurify) that only permits a known-safe set of tags/attributes, rather than trying to enumerate every dangerous one.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
