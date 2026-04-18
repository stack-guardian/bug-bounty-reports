# [Vulnerability Type] — [Target Domain]

**Target:** [domain]
**Platform:** [bug bounty platform]
**Researcher:** Vibhor Prasad (stack-guardian)
**Date of Discovery:** [date]
**Date Submitted:** [date]
**Status:** [Duplicate / Not Applicable / Won't Fix / Triaged / Resolved]

---

## Summary

[One paragraph written for a non-technical reader. Describe what the vulnerability is, where it exists in the application, and what a real attacker could accomplish by exploiting it. Avoid jargon here.]

---

## Vulnerability Details

| Field                   | Value                                           |
|-------------------------|-------------------------------------------------|
| Type                    | [e.g. Reflected XSS / IDOR / SQLi / SSRF]      |
| CWE                     | [e.g. CWE-79]                                   |
| Severity                | [Critical / High / Medium / Low]                |
| CVSS v3.1 Score         | [e.g. 6.1]                                      |
| CVSS v3.1 Vector        | [e.g. AV:N/AC:L/PR:N/UI:R/S:C/C:L/I:L/A:N]    |
| Affected Endpoint       | [full URL]                                      |
| Vulnerable Parameter    | [parameter name or location in request]         |
| Authentication Required | [Yes / No]                                      |
| Discovery Method        | [e.g. manual testing, JS review, Burp passive]  |
| Tools Used              | [e.g. Burp Suite, browser DevTools, curl]       |

---

## Reproduction Steps

1. [Step — precise enough to reproduce independently]
2. [Step]
3. [Step]
4. Observe: [exact application behavior that confirms the finding]

---

## Proof of Concept

### Payload
[exact payload — unmodified]

### HTTP Request
[Full raw HTTP request in Burp Suite format — Host, headers, body]

### HTTP Response
[Relevant excerpt from server response confirming the vulnerability triggered]

### Observed Behavior

[One paragraph. What the application did that it should not have done. Factual and specific.]

---

## Impact

[What an attacker can concretely achieve. Avoid vague language. Specify what data is exposed, from whose session, under what access conditions, and whether chaining with another finding would increase impact.]

---

## Remediation

[Specific recommendation for this code pattern. Name the sanitization function, encoding method, or access control check that is missing and where it should be applied. Do not write generic advice.]

---

## References

- [OWASP link specific to this vulnerability class]
- [CWE link]
- [PortSwigger explanation if applicable]
- [CVE reference if a known pattern applies]

---

## Timeline

| Date       | Event                                         |
|------------|-----------------------------------------------|
| [date]     | Anomalous behavior first observed             |
| [date]     | Vulnerability confirmed as exploitable        |
| [date]     | PoC documented                                |
| [date]     | Report written and reviewed                   |
| [date]     | Submitted to platform                         |
| [date]     | Platform acknowledged receipt                 |
| [date]     | Status assigned by platform                   |
| [date]     | Added to portfolio                            |
