# Bug Bounty Reports

Independent security research and vulnerability documentation by Vibhor Prasad.

All findings documented here were discovered through authorized bug bounty programs. Testing was conducted within defined program scope, following responsible disclosure practices throughout.

---

## Research Summary

| Platform | Target        | Findings | Status               |
|----------|---------------|----------|----------------------|
| BugV     | omgnepal.com  | 1        | Reported / Duplicate |

---

## Repository Structure

```
bug-bounty-reports/
├── reports/
│   └── omgnepal/
│       ├── README.md
│       └── findings/
│           └── VULN-001/
│               ├── report.md
│               └── poc/
│                   └── README.md
└── templates/
    ├── vuln-report-template.md
    └── poc-template.md
```

---

## Methodology

Recon is done through subdomain enumeration, endpoint discovery, and JavaScript analysis. Vulnerabilities are tested manually using Burp Suite, cross-referenced against OWASP Top 10 and relevant CWEs. Every finding is documented with reproduction steps and a minimal, non-destructive proof of concept before submission.

Disclosure follows the platform's process. Findings are published here after triage is complete or duplicate status is confirmed.

---

## Background

- B.Tech in Cyber Security, LNCT Group of Colleges, Bhopal (Expected June 2027)
- Certified Network Security Practitioner (CNSP) — 85%
- PortSwigger Web Security Academy — completed labs in XSS, SQL Injection, Access Control, Authentication, and API Security
- Tools: Burp Suite, Nmap, Wireshark, Hydra, Aircrack-ng

GitHub: https://github.com/stack-guardian
LinkedIn: https://www.linkedin.com/in/vibhor-prasad-3ba373381/
Contact: vibhorprasad14@gmail.com
