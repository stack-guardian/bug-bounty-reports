# Submission Log

Complete record of all bug bounty reports submitted through BugV. This log includes every submission regardless of outcome. Closed, duplicate, and not-applicable findings are included because they represent real research volume and methodology, not just accepted payouts.

Total submissions: 10
Platform: BugV
Targets: omgnepal.com (8 submissions), cynicaltechnology.com (2 submissions)
Severity range: Low to High
Period: March 2026

---

## Full Log

| ID       | Target                  | Vulnerability Type                                      | Severity      | Status          | BugV Report ID |
|----------|-------------------------|---------------------------------------------------------|---------------|-----------------|----------------|
| VULN-001 | omgnepal.com            | WordPress Username Enumeration via Author ID Parameter  | Low           | Duplicate       | #000094Q       |
| VULN-002 | omgnepal.com            | XMLRPC system.multicall Credential Brute Force          | High          | Duplicate       | #000094S       |
| VULN-003 | omgnepal.com            | Username Enumeration via Login Error Messages           | Low           | Won't Fix       | #000094R       |
| VULN-004 | omgnepal.com            | CORS Misconfiguration — Wildcard Origin Reflection      | High          | Won't Fix       | #000094V       |
| VULN-005 | omgnepal.com            | CORS Misconfiguration — Authenticated API Requests      | High          | Not Applicable  | #000094X       |
| VULN-006 | omgnepal.com            | Missing Security Headers                                | Moderate      | Not Applicable  | #000094W       |
| VULN-007 | omgnepal.com            | Analytics Cookies Missing HttpOnly and Secure Flags     | Informational | Not Applicable  | #0000950       |
| VULN-008 | omgnepal.com            | Hidden CTF Flag in robots.txt                           | Informational | Not Applicable  | #000094P       |
| VULN-C01 | cynicaltechnology.com   | WordPress REST API User Enumeration via /wp-json/       | Moderate      | Duplicate       | #0000952       |
| VULN-C02 | cynicaltechnology.com   | CORS Misconfiguration on WordPress REST API             | High          | Not Applicable  | #0000951       |

---

## Status Definitions

**Duplicate:** The vulnerability was real, independently discovered, and the report was correct in every way. Another researcher submitted the same finding first. A duplicate outcome confirms the vulnerability existed and was found through legitimate testing — it does not invalidate the research.

**Not Applicable:** The behavior identified did not meet the program's criteria for a valid finding. Reasons typically include the endpoint being outside defined scope, the vulnerability requiring conditions the program does not consider realistic, or the behavior being an intended design decision. Not applicable does not mean the behavior was not anomalous — it means the program chose not to act on it.

**Won't Fix:** The vendor reviewed the finding, confirmed it exists, and made a deliberate decision not to remediate it. This typically reflects an accepted risk decision based on low likelihood of exploitation or the cost of the fix relative to the impact. Won't Fix confirms the vulnerability is real.

---

## Severity Distribution

High severity findings: 4 (VULN-002, VULN-004, VULN-005, VULN-C02)
Moderate severity findings: 2 (VULN-006, VULN-C01)
Low severity findings: 2 (VULN-001, VULN-003)
Informational findings: 2 (VULN-007, VULN-008)
