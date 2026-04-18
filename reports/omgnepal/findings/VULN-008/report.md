# VULN-008 — Hidden CTF Flag in robots.txt

**Target:** omgnepal.com
**Platform:** BugV
**Researcher:** Vibhor Prasad (stack-guardian)
**BugV Report ID:** #000094P
**Date Submitted:** March 2026
**Status:** Not Applicable

---

## Summary

During standard recon of omgnepal.com, a CTF-style flag (`bugv{You_Found_Me}`) was discovered embedded in the `robots.txt` file. This was submitted as a potential information disclosure finding. The platform confirmed the flag was intentionally placed as part of a CTF challenge and closed the report as Not Applicable — Informational.

---

## Vulnerability Details

| Field                   | Value                                      |
|-------------------------|--------------------------------------------|
| Type                    | Information Disclosure                     |
| CWE                     | CWE-200: Exposure of Sensitive Information |
| Severity                | Informational                              |
| CVSS v3.1 Score         | 0.0                                        |
| CVSS v3.1 Vector        | N/A (Informational)                        |
| Affected Endpoint       | https://omgnepal.com/robots.txt            |
| Vulnerable Parameter    | N/A                                        |
| Authentication Required | No                                         |
| Discovery Method        | Manual recon — robots.txt review           |
| Tools Used              | Browser, curl                              |

---

## Reproduction Steps

1. Navigate to `https://omgnepal.com/robots.txt`.
2. Observe the file contents.
3. Note the CTF flag embedded in the file.

---

## Proof of Concept

### HTTP Request

```
GET /robots.txt HTTP/1.1
Host: omgnepal.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Connection: close
```

### HTTP Response

```
HTTP/1.1 200 OK
Content-Type: text/plain
Server: Apache

bugv{You_Found_Me}
```

---

## Platform Response

The triage team confirmed the flag was intentionally placed as part of a BugV CTF challenge and does not constitute a vulnerability. Report closed as N/A Informational.

---

## Notes

This finding is documented here for completeness as part of the full recon methodology. Checking `robots.txt` for sensitive information is a standard recon step — in a real engagement, content in this file could expose admin paths, staging URLs, or internal directory structures.

---

## References

- [CWE-200: Exposure of Sensitive Information](https://cwe.mitre.org/data/definitions/200.html)
- [OWASP — Information Gathering via robots.txt](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage)

---

## Timeline

| Date          | Event                                    |
|---------------|------------------------------------------|
| March 2026    | Discovered during recon                  |
| March 2026    | Report submitted via BugV (#000094P)     |
| March 2026    | Status assigned: Not Applicable          |
| April 2026    | Added to portfolio                       |
