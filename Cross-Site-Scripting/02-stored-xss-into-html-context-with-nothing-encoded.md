# Lab 02 — Stored XSS into HTML context with nothing encoded

**Category:** Cross-Site Scripting (XSS)
**Difficulty:** Apprentice
**Reference:** https://portswigger.net/web-security/cross-site-scripting/stored

## Objective
A blog comment function stores attacker input and renders it unencoded for every visitor. Post a comment that executes JavaScript for anyone viewing the blog post.

## Vulnerability
The comment's `comment` field is saved as-is and rendered directly into the page's HTML for all future visitors, with no sanitization or encoding.

## Steps to Reproduce
1. Open a blog post and find the comment form (name, email, website, comment).
2. Submit a comment whose body is a `<script>` payload.
3. Reload the blog post page to confirm the script executes for any viewer.

## Payload(s) Used
```
<script>alert(document.cookie)</script>
```

## Explanation
Because the comment is persisted and later rendered without encoding, the payload becomes part of the page's permanent HTML. Every subsequent visitor's browser executes it, making this far more dangerous than reflected XSS since no interaction/clicking a crafted link is required — simply viewing the page triggers it.

## Remediation
Sanitize/encode stored user content on output (not just input), use an allow-list HTML sanitizer if rich text is required, and set HttpOnly on session cookies to limit impact.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
