# PortSwigger Web Security Academy — Lab Writeups

Personal writeups of labs completed on [PortSwigger's Web Security Academy](https://portswigger.net/web-security), documenting the vulnerability, exploitation steps, payload used, and remediation for each lab.

## Repository Structure

```
portswigger-writeups/
├── SQL-Injection/
│   ├── 01-sql-injection-vulnerability-in-where-clause-...md
│   ├── 02-sql-injection-vulnerability-allowing-login-bypass.md
│   ├── ... (17 labs total)
│   ├── screenshots/
│   └── README.md            <- index of all SQLi labs
├── Cross-Site-Scripting/
│   ├── 01-reflected-xss-into-html-context-with-nothing-encoded.md
│   ├── ... (15 labs total)
│   ├── screenshots/
│   └── README.md            <- index of all XSS labs
└── README.md                 <- you are here
```

## Categories

| Category | Labs Covered | Index |
|---|---|---|
| SQL Injection | 17 | [SQL-Injection/README.md](./SQL-Injection/README.md) |
| Cross-Site Scripting (XSS) | 19 | [Cross-Site-Scripting/README.md](./Cross-Site-Scripting/README.md) |

Each writeup follows the same format: **Objective → Vulnerability → Steps to Reproduce → Payload(s) Used → Explanation → Screenshots → Remediation**.

## Screenshots

Each category has its own `screenshots/` folder. Drop images there and reference them from the matching lab's `.md` file in this form:

```markdown
![Lab 01 screenshot](./screenshots/sqli-lab-01-01.png)
```

Suggested naming convention: `<category>-lab-<number>-<step>.png` (e.g. `xss-lab-02-01.png`, `xss-lab-02-02.png` for multiple screenshots in the same lab).

## Disclaimer

These writeups document vulnerabilities in PortSwigger's intentionally-vulnerable training labs, used strictly for learning purposes. No real-world systems were tested.

---

