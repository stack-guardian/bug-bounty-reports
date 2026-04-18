# VULN-005 — CORS Misconfiguration — Authenticated API Requests

**Target:** omgnepal.com
**Platform:** BugV
**Researcher:** Vibhor Prasad (stack-guardian)
**BugV Report ID:** #000094X
**Date Submitted:** March 2026
**Status:** Not Applicable

---

## Summary

The WordPress REST API at omgnepal.com reflects any `Origin` header and responds with `Access-Control-Allow-Credentials: true`, allowing any malicious website to make authenticated API requests on behalf of logged-in users. The platform marked this Not Applicable because the `/wp-json/` endpoint only exposes publicly accessible information and protected endpoints return 401 without demonstrated authenticated data exposure.

---

## Vulnerability Details

| Field                   | Value                                                        |
|-------------------------|--------------------------------------------------------------|
| Type                    | CORS Misconfiguration — Authenticated API Access             |
| CWE                     | CWE-942: Permissive Cross-domain Policy with Untrusted Domains |
| Severity                | High                                                         |
| CVSS v3.1 Score         | 8.1                                                          |
| CVSS v3.1 Vector        | AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:N                        |
| Affected Endpoint       | https://omgnepal.com/wp-json/                                |
| Vulnerable Parameter    | `Origin` request header                                      |
| Authentication Required | No (to trigger); Yes (to access protected endpoints)         |
| Discovery Method        | Manual testing via curl                                      |
| Tools Used              | curl, Burp Suite                                             |

---

## Summary of Platform Decision

The triage team noted that while the CORS misconfiguration exists, the `/wp-json/` endpoint only exposes publicly available information and protected endpoints return `401 – REST API restricted to authenticated users`. The report was closed as Not Applicable because no sensitive authenticated data exposure was demonstrated. The finding is documented here as the CORS misconfiguration itself is real and the configuration is insecure by design.

---

## Reproduction Steps

1. Run: `curl -s "https://omgnepal.com/wp-json/" -H "Origin: https://evil.com" -I | grep -i "access-control"`
2. Observe `access-control-allow-origin: https://evil.com` and `access-control-allow-credentials: true` in the response.

---

## Proof of Concept

See [poc/README.md](./poc/README.md).

### HTTP Response Headers

```
access-control-expose-headers: X-WP-Total, X-WP-TotalPages, Link
access-control-allow-headers: Authorization, X-WP-Nonce, Content-Disposition, Content-MD5, Content-Type
access-control-allow-origin: https://evil.com
access-control-allow-methods: OPTIONS, GET, POST, PUT, PATCH, DELETE
access-control-allow-credentials: true
```

---

## Impact

Any malicious website can make authenticated API calls to omgnepal.com on behalf of logged-in users. While the base `/wp-json/` endpoint is public, the misconfiguration extends to all REST API routes. If any authenticated endpoint exposes private data, it is accessible cross-origin without additional exploitation.

---

## Remediation

Restrict `Access-Control-Allow-Origin` to a specific trusted domain and never combine reflected origins with `Access-Control-Allow-Credentials: true`. See VULN-004 for the specific code fix.

---

## References

- [PortSwigger — CORS Vulnerabilities](https://portswigger.net/web-security/cors)
- [CWE-942](https://cwe.mitre.org/data/definitions/942.html)

---

## Timeline

| Date          | Event                                    |
|---------------|------------------------------------------|
| March 2026    | Vulnerability identified                 |
| March 2026    | Report submitted via BugV (#000094X)     |
| March 2026    | Status assigned: Not Applicable          |
| April 2026    | Added to portfolio                       |
