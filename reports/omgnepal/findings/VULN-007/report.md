# VULN-007 — Analytics Cookies Missing HttpOnly and Secure Flags

**Target:** omgnepal.com
**Platform:** BugV
**Researcher:** Vibhor Prasad (stack-guardian)
**BugV Report ID:** #0000950
**Date Submitted:** March 2026
**Status:** Not Applicable

---

## Summary

Several analytics cookies set by omgnepal.com (`_ga`, `_gid`, `_gat_gtag_UA_58097238_1`, `FCNEC`) are missing the `HttpOnly` and `Secure` flags. Without `HttpOnly`, these cookies are accessible to JavaScript on the page. Without `Secure`, they could be transmitted over unencrypted HTTP connections. The platform marked this Not Applicable because the affected cookies are third-party analytics cookies, not session or authentication cookies, and their exposure does not present a security impact under the program's scope.

---

## Vulnerability Details

| Field                   | Value                                                  |
|-------------------------|--------------------------------------------------------|
| Type                    | Insecure Cookie Attributes                             |
| CWE                     | CWE-614: Sensitive Cookie in HTTPS Session Without Secure Attribute |
| Severity                | Informational                                          |
| CVSS v3.1 Score         | 0.0                                                    |
| CVSS v3.1 Vector        | N/A (Informational)                                    |
| Affected Endpoint       | https://omgnepal.com (all pages)                       |
| Vulnerable Parameter    | `_ga`, `_gid`, `_gat_gtag_UA_58097238_1`, `FCNEC` cookies |
| Authentication Required | No                                                     |
| Discovery Method        | Browser DevTools — Application tab, Cookies section    |
| Tools Used              | Browser DevTools                                       |

---

## Reproduction Steps

1. Visit `https://omgnepal.com` in a browser.
2. Open DevTools → Application → Storage → Cookies → omgnepal.com.
3. Observe the `_ga`, `_gid`, `_gat_gtag_UA_58097238_1`, and `FCNEC` cookies.
4. Observe: the `HttpOnly` and `Secure` columns are empty (not set) for these cookies.

---

## Proof of Concept

### HTTP Response (Set-Cookie headers)

```
HTTP/2 200 OK
date: Wed, 04 Mar 2026 06:00:00 GMT
content-type: text/html; charset=UTF-8
server: cloudflare
set-cookie: _ga=GA1.2.123456789.1234567890; Path=/; Domain=omgnepal.com
set-cookie: _gid=GA1.2.987654321.1234567890; Path=/; Domain=omgnepal.com
set-cookie: _gat_gtag_UA_58097238_1=1; Path=/; Domain=omgnepal.com
```

No `HttpOnly` or `Secure` attributes present on any of these cookies.

---

## Impact

Low risk. These are analytics cookies, not session or authentication cookies. If an XSS vulnerability exists on the site, an attacker could read these cookie values and potentially track user analytics data. The absence of `Secure` means they could theoretically be sent over HTTP, though Cloudflare enforces HTTPS by default.

---

## Remediation

Set `HttpOnly` and `Secure` flags on all cookies, including analytics cookies, as a best practice:

```
Set-Cookie: _ga=...; Path=/; Domain=omgnepal.com; Secure; HttpOnly; SameSite=Lax
```

Note: Google Analytics cookies are set by the GA script and may require configuration changes in the GA tag or GTM rather than server-side header modification.

---

## References

- [OWASP — Testing for Cookies Attributes](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes)
- [CWE-614](https://cwe.mitre.org/data/definitions/614.html)

---

## Timeline

| Date          | Event                                    |
|---------------|------------------------------------------|
| March 2026    | Identified during cookie inspection      |
| March 2026    | Report submitted via BugV (#0000950)     |
| March 2026    | Status assigned: Not Applicable          |
| April 2026    | Added to portfolio                       |
