# Bug Bounty Reports

Independent security research and vulnerability documentation by Vibhor Prasad.

All findings were discovered through the BugV authorized bug bounty platform. Targets include omgnepal.com and cynicaltechnology.com. Testing was conducted within defined program scope following responsible disclosure practices throughout.

---

## Research Overview

Platform: BugV
Targets: omgnepal.com, cynicaltechnology.com
Total submissions: 10
Severity range: Low to High
Period: March 2026 to March 2026

Submission breakdown:

- 3 marked Duplicate — valid findings, independently discovered, another researcher submitted the same issue first
- 5 marked Not Applicable — did not meet the program's exploitability threshold or fell outside defined scope
- 2 marked Won't Fix — vendor acknowledged the finding and accepted the risk rather than remediating

None of these outcomes indicate incorrect research. All 10 reports were written to standard with full reproduction steps and proof-of-concept documentation. The SUBMISSION_LOG.md file contains the full tracker. METHODOLOGY.md covers the research process in detail.

---

## Repository Structure

```
SUBMISSION_LOG.md       Master tracker of all 10 submissions with type, severity, CVSS, and status
METHODOLOGY.md          Research approach, tools used, scope compliance, and documentation standard
DISCLOSURE_POLICY.md    Responsible disclosure policy and explanation of each status outcome
reports/                One folder per target, one subfolder per finding
templates/              Reusable templates for future report and PoC documentation
```

---

## Skill Areas Demonstrated

Web application security testing covering vulnerability identification, manual exploitation, structured reporting, and responsible disclosure. Findings span multiple vulnerability classes across the low to high severity range. Each submission includes raw HTTP request and response, exact payload, reproduction steps, impact analysis, and a specific remediation recommendation.

Tools used across this research: Burp Suite, Nmap, browser DevTools, manual JavaScript analysis, curl.

---

## Background

B.Tech in Cyber Security, LNCT Group of Colleges, Bhopal, India. Expected graduation June 2027.
Certified Network Security Practitioner (CNSP) — 85%.
PortSwigger Web Security Academy — completed labs in XSS, SQL Injection, Access Control, Authentication, and API Security.

GitHub: https://github.com/stack-guardian
LinkedIn: https://www.linkedin.com/in/vibhor-prasad-3ba373381/
Contact: vibhorprasad14@gmail.com

---

## Legal

All research was conducted through authorized bug bounty programs only. No unauthorized access was performed. See DISCLOSURE_POLICY.md for the full responsible disclosure policy.
