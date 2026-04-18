# Research Methodology

This document covers the approach used across all 10 submissions. The intent is to show the process behind the findings — what was looked for, how testing was conducted, and how findings were documented before submission.

---

## Reconnaissance

Surface mapping started with subdomain enumeration using certificate transparency logs and DNS analysis. Endpoints were identified through crawling, JavaScript file review, and inspection of API responses in Burp Suite. Any endpoint accepting user-controlled input was flagged for further testing.

Standard recon checklist applied to each target:
- `robots.txt` and `sitemap.xml` review
- WordPress-specific paths: `/wp-json/`, `/xmlrpc.php`, `/wp-login.php`, `/?author=`
- HTTP response header inspection
- Cookie attribute review via browser DevTools
- CORS behavior testing with custom `Origin` headers

Tools used in this phase: Nmap, browser DevTools, Burp Suite passive crawler, manual DNS inspection, curl.

---

## Vulnerability Identification

Testing focused on the OWASP Top 10 vulnerability classes with priority given to authentication flows, access control logic, input handling, and API endpoints. Each potential finding was manually confirmed before documentation began. Automated scanners were used only to validate findings after manual identification, not as a primary discovery method.

The vulnerability classes investigated included:
- Username and user data enumeration (WordPress-specific vectors)
- CORS misconfiguration and credential exposure
- Missing HTTP security headers
- XML-RPC abuse for credential brute force
- Information disclosure via publicly accessible files

Exact classes found are documented in SUBMISSION_LOG.md.

---

## Documentation Standard

Every report submitted to BugV follows this structure:

- Plain-English summary written for both technical and non-technical readers
- Full details table including vulnerability type, CWE, severity, affected endpoint, vulnerable parameter, and authentication requirement
- Discovery method and tools used, specific to that finding
- Numbered reproduction steps written so the finding can be independently verified by the triage team
- Raw HTTP request in Burp Suite format showing the vulnerability triggered
- Server response excerpt showing the application's behavior confirming the issue
- Exact payload used, unmodified
- Impact assessment describing what an attacker can concretely achieve under realistic conditions
- Remediation recommendation specific to the code pattern observed, not generic advice
- Full timeline from first observation through submission and closure

---

## Severity Scoring

All findings are labelled using the severity bands defined by the BugV program. Where CVSS v3.1 vectors are available they are included in the individual report files.

Severity bands:
- Critical: 9.0 and above
- High: 7.0 to 8.9
- Medium / Moderate: 4.0 to 6.9
- Low: 0.1 to 3.9
- Informational: 0.0

All findings in this portfolio fall in the informational to high range.

---

## Scope Compliance

All testing was conducted only on endpoints and subdomains listed as in-scope by the BugV programs for omgnepal.com and cynicaltechnology.com. Out-of-scope assets were identified during recon and excluded. No automated attacks, denial of service attempts, or social engineering techniques were used at any point during this research.
