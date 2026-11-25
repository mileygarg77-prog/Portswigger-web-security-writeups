# Lab 17 — SQL injection with filter bypass via XML encoding

**Category:** SQL Injection
**Difficulty:** Expert
**Reference:** https://portswigger.net/web-security/sql-injection

## Objective
A web application firewall (WAF) blocks common SQL injection payloads in the request body. Bypass the filter by encoding the payload as XML entities inside an XML request.

## Vulnerability
The login request is sent as XML and parsed server-side; the WAF inspects the raw request for SQLi signatures but the XML parser decodes entities *before* the value reaches the SQL layer, so entity-encoded characters slip past the filter.

## Steps to Reproduce
1. Switch the login request's `Content-Type` to `application/xml` and send credentials as an XML body (e.g. via Burp's 'Convert to XML' context menu option).
2. Define a custom internal DTD entity that decodes to the SQLi payload (e.g. `' OR 1=1--`).
3. Reference that entity in place of the username value so the WAF only ever sees the harmless-looking entity reference, not the raw payload.

## Payload(s) Used
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE user [<!ENTITY xxe "' OR 1=1-- ">]>
<user><username>&xxe;</username><password>pw</password></user>
```

## Explanation
The WAF pattern-matches on the literal bytes of the HTTP request and never sees the string `' OR 1=1--` because it's hidden inside a custom XML entity declaration. However, the backend's XML parser resolves `&xxe;` into its full payload *before* that value is handed to the SQL query builder, so the injection executes exactly as if it had been sent in plaintext.

## Screenshots
_Add your screenshots here, e.g. `./screenshots/sqli-lab-17-01.png`_

## Remediation
Never rely on signature-based WAF filtering as the primary defense; disable external/internal DTD processing in the XML parser, and use parameterized queries so even a fully decoded payload cannot alter query structure.

---
*Lab solved on PortSwigger Web Security Academy. Writeup by Miley.*
