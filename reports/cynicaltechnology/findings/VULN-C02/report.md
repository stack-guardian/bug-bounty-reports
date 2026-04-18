# VULN-C02 — CORS Misconfiguration on WordPress REST API

**Target:** cynicaltechnology.com
**Platform:** BugV
**Researcher:** Vibhor Prasad (stack-guardian)
**BugV Report ID:** #0000951
**Date Submitted:** March 2026
**Status:** Not Applicable

---

## Summary

The WordPress REST API at cynicaltechnology.com reflects any `Origin` header and responds with `Access-Control-Allow-Credentials: true`. This allows any malicious website to make authenticated API requests on behalf of logged-in users. The platform marked this Not Applicable because the `/wp-json/wp/v2/users` endpoint only exposes publicly available WordPress author information and no sensitive authenticated data exposure was demonstrated.

---

## Vulnerability Details

| Field                   | Value                                                        |
|-------------------------|--------------------------------------------------------------|
| Type                    | CORS Misconfiguration — Arbitrary Origin Reflection          |
| CWE                     | CWE-942: Permissive Cross-domain Policy with Untrusted Domains |
| Severity                | High                                                         |
| CVSS v3.1 Score         | 8.1                                                          |
| CVSS v3.1 Vector        | AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:N                        |
| Affected Endpoint       | https://cynicaltechnology.com/wp-json/                       |
| Vulnerable Parameter    | `Origin` request header                                      |
| Authentication Required | No (to trigger); Yes (to access protected data)              |
| Discovery Method        | Manual testing via curl                                      |
| Tools Used              | curl, Burp Suite                                             |

---

## Reproduction Steps

1. Run: `curl -s "https://cynicaltechnology.com/wp-json/wp/v2/users" -H "Origin: https://evil.com" -I | grep -i "access-control"`
2. Observe `Access-Control-Allow-Origin: https://evil.com` and `Access-Control-Allow-Credentials: true` in the response.

---

## Proof of Concept

See [poc/README.md](./poc/README.md).

### HTTP Request

```
GET /wp-json/wp/v2/users HTTP/1.1
Host: cynicaltechnology.com
Origin: https://evil.com
```

### HTTP Response Headers

```
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Access-Control-Allow-Origin: https://evil.com
Access-Control-Allow-Credentials: true
Access-Control-Allow-Methods: OPTIONS, GET, POST, PUT, PATCH, DELETE
Access-Control-Allow-Headers: Authorization, Content-Type
Content-Type: application/json
```

---

## Impact

Any malicious website can make authenticated API calls to cynicaltechnology.com on behalf of logged-in users. Combined with VULN-C01 (user enumeration), an attacker can harvest user data cross-origin. If any authenticated endpoint exposes private data, it is accessible without additional exploitation.

---

## Remediation

Restrict `Access-Control-Allow-Origin` to a specific trusted domain. Never reflect the `Origin` header directly. Never combine a reflected or wildcard origin with `Access-Control-Allow-Credentials: true`.

---

## References

- [PortSwigger — CORS Vulnerabilities](https://portswigger.net/web-security/cors)
- [CWE-942](https://cwe.mitre.org/data/definitions/942.html)
- [OWASP — Testing for CORS](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing)

---

## Timeline

| Date          | Event                                    |
|---------------|------------------------------------------|
| March 2026    | Vulnerability identified                 |
| March 2026    | Report submitted via BugV (#0000951)     |
| March 2026    | Status assigned: Not Applicable          |
| April 2026    | Added to portfolio                       |
