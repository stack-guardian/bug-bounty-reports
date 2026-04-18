# VULN-006 — Missing Security Headers

**Target:** omgnepal.com
**Platform:** BugV
**Researcher:** Vibhor Prasad (stack-guardian)
**BugV Report ID:** #000094W
**Date Submitted:** March 2026
**Status:** Not Applicable

---

## Summary

The HTTP responses from omgnepal.com are missing all standard security headers. The absence of headers such as `Content-Security-Policy`, `X-Frame-Options`, `X-Content-Type-Options`, `Strict-Transport-Security`, and `Referrer-Policy` leaves the site more exposed to XSS, clickjacking, MIME confusion attacks, and protocol downgrade attacks. The platform marked this Not Applicable as missing headers alone do not constitute an exploitable vulnerability under the program's scope.

---

## Vulnerability Details

| Field                   | Value                                                        |
|-------------------------|--------------------------------------------------------------|
| Type                    | Missing HTTP Security Headers                                |
| CWE                     | CWE-693: Protection Mechanism Failure                        |
| Severity                | Moderate                                                     |
| CVSS v3.1 Score         | 5.3                                                          |
| CVSS v3.1 Vector        | AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N                        |
| Affected Endpoint       | https://omgnepal.com (all pages)                             |
| Vulnerable Parameter    | HTTP response headers                                        |
| Authentication Required | No                                                           |
| Discovery Method        | Manual HTTP response header inspection via Burp Suite        |
| Tools Used              | Burp Suite, curl                                             |

---

## Missing Headers

| Header                    | Risk if Absent                                      |
|---------------------------|-----------------------------------------------------|
| Content-Security-Policy   | No browser-level XSS mitigation                     |
| X-Frame-Options           | Site can be embedded in iframes (clickjacking)      |
| X-Content-Type-Options    | Browser may execute malicious files as HTML/JS      |
| Strict-Transport-Security | Users can be forced to HTTP (downgrade attack)      |
| X-XSS-Protection          | Legacy browser XSS filter disabled                  |
| Referrer-Policy           | Referrer data sent to third parties uncontrolled    |
| Permissions-Policy        | Browser features (camera, mic) unrestricted         |

---

## Reproduction Steps

1. Send a GET request to `https://omgnepal.com`.
2. Inspect the response headers.
3. Observe that none of the security headers listed above are present.

---

## Proof of Concept

### HTTP Response (excerpt)

```
HTTP/2 200
date: Wed, 04 Mar 2026 05:59:50 GMT
content-type: text/html; charset=UTF-8
server: cloudflare
cf-cache-status: DYNAMIC
cf-ray: 9d6ea83a8ba188d4-SIN
```

No `Content-Security-Policy`, `X-Frame-Options`, `X-Content-Type-Options`, `Strict-Transport-Security`, `Referrer-Policy`, or `Permissions-Policy` headers present.

---

## Impact

Without these headers, the site relies entirely on application-level controls for XSS and injection prevention. If any XSS vulnerability exists (or is introduced), there is no browser-level fallback. The site can be embedded in malicious iframes for clickjacking attacks. Users on networks with SSL stripping capabilities may be served the site over HTTP.

---

## Remediation

Add the following headers via Cloudflare Transform Rules or the web server configuration:

```
Content-Security-Policy: default-src 'self'; script-src 'self' 'unsafe-inline' https://www.google-analytics.com https://www.googletagmanager.com; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; font-src 'self' data:; frame-ancestors 'self';
X-Frame-Options: SAMEORIGIN
X-Content-Type-Options: nosniff
Strict-Transport-Security: max-age=31536000; includeSubDomains
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: geolocation=(), microphone=(), camera=(), payment=()
```

---

## References

- [OWASP Secure Headers Project](https://owasp.org/www-project-secure-headers/)
- [CWE-693: Protection Mechanism Failure](https://cwe.mitre.org/data/definitions/693.html)
- [Mozilla Observatory](https://observatory.mozilla.org/)

---

## Timeline

| Date          | Event                                    |
|---------------|------------------------------------------|
| March 2026    | Identified during header inspection      |
| March 2026    | Report submitted via BugV (#000094W)     |
| March 2026    | Status assigned: Not Applicable          |
| April 2026    | Added to portfolio                       |
