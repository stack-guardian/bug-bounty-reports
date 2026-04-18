# VULN-004 — CORS Misconfiguration — Wildcard Origin Reflection with Credentials

**Target:** omgnepal.com
**Platform:** BugV
**Researcher:** Vibhor Prasad (stack-guardian)
**BugV Report ID:** #000094V
**Date Submitted:** March 2026
**Status:** Won't Fix

---

## Summary

The WordPress REST API at omgnepal.com reflects any `Origin` header sent in the request and responds with `Access-Control-Allow-Credentials: true`. This means any malicious website can make authenticated API requests to omgnepal.com on behalf of a logged-in user and read the responses. An attacker who hosts a malicious page can steal data accessible via the REST API from any victim who visits the page while logged into omgnepal.com.

---

## Vulnerability Details

| Field                   | Value                                                        |
|-------------------------|--------------------------------------------------------------|
| Type                    | CORS Misconfiguration — Arbitrary Origin Reflection          |
| CWE                     | CWE-942: Permissive Cross-domain Policy with Untrusted Domains |
| Severity                | High                                                         |
| CVSS v3.1 Score         | 8.1                                                          |
| CVSS v3.1 Vector        | AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:N                        |
| Affected Endpoint       | https://omgnepal.com/wp-json/                                |
| Vulnerable Parameter    | `Origin` request header                                      |
| Authentication Required | No (to trigger); Yes (to read authenticated data)            |
| Discovery Method        | Manual testing via curl                                      |
| Tools Used              | curl, Burp Suite                                             |

---

## Reproduction Steps

1. Send a GET request to `https://omgnepal.com/wp-json/` with a custom `Origin: https://evil.com` header.
2. Observe the response headers.
3. Confirm that `Access-Control-Allow-Origin: https://evil.com` and `Access-Control-Allow-Credentials: true` are both present.
4. Observe: any origin is trusted, and credentials (cookies) are permitted cross-origin.

---

## Proof of Concept

See [poc/README.md](./poc/README.md) for full reproduction details.

### Command

```bash
curl -s "https://omgnepal.com/wp-json/" -H "Origin: https://evil.com" -I | grep -i "access-control"
```

### HTTP Request

```
GET /wp-json/ HTTP/1.1
Host: omgnepal.com
Origin: https://evil.com
```

### HTTP Response Headers

```
access-control-expose-headers: X-WP-Total, X-WP-TotalPages, Link
access-control-allow-headers: Authorization, X-WP-Nonce, Content-Disposition, Content-MD5, Content-Type
access-control-allow-origin: https://evil.com
access-control-allow-methods: OPTIONS, GET, POST, PUT, PATCH, DELETE
access-control-allow-credentials: true
```

### Observed Behavior

The server reflects the attacker-controlled `Origin` value directly into `Access-Control-Allow-Origin` and simultaneously sets `Access-Control-Allow-Credentials: true`. This combination allows cross-origin requests with cookies, bypassing the same-origin policy.

---

## Impact

A logged-in omgnepal.com user who visits a malicious website will have their browser silently make authenticated API requests to omgnepal.com. The attacker's JavaScript can read the full API responses, including any data accessible to the authenticated user. Depending on the user's role, this could expose private posts, user profiles, or administrative data. Combined with VULN-001 and VULN-003, this creates a path to targeted account compromise.

---

## Remediation

1. **Restrict the allowed origin to a specific trusted domain:**
   ```php
   // In functions.php or a plugin
   add_filter('rest_pre_serve_request', function($served, $result, $request, $server) {
       $origin = get_http_origin();
       $allowed = 'https://omgnepal.com';
       if ($origin === $allowed) {
           header('Access-Control-Allow-Origin: ' . $allowed);
           header('Access-Control-Allow-Credentials: true');
       }
       return $served;
   }, 10, 4);
   ```
2. **Never reflect the `Origin` header directly** — always validate against an allowlist.
3. **Do not combine wildcard or reflected origins with `Allow-Credentials: true`** — this is explicitly prohibited by the CORS specification.

---

## References

- [OWASP — Testing for CORS](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing)
- [CWE-942: Permissive Cross-domain Policy](https://cwe.mitre.org/data/definitions/942.html)
- [PortSwigger — CORS Vulnerabilities](https://portswigger.net/web-security/cors)

---

## Timeline

| Date          | Event                                    |
|---------------|------------------------------------------|
| March 2026    | Vulnerability identified                 |
| March 2026    | PoC confirmed and documented             |
| March 2026    | Report submitted via BugV (#000094V)     |
| March 2026    | Platform acknowledged receipt            |
| March 2026    | Status assigned: Won't Fix               |
| April 2026    | Added to portfolio                       |
