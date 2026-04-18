# Proof of Concept — VULN-006

**Target:** omgnepal.com
**Type:** Missing HTTP Security Headers
**Endpoint:** https://omgnepal.com
**Tested on:** March 2026
**Researcher:** Vibhor Prasad (stack-guardian)

---

## Environment

- Tool: Burp Suite, curl
- OS: Arch Linux
- Authenticated: No
- Account type: Unauthenticated

---

## Preconditions

None.

---

## Steps to Reproduce

1. Send a GET request to `https://omgnepal.com`.
2. Inspect the response headers for the presence of security headers.

---

## Payload

```bash
curl -s -I "https://omgnepal.com" | grep -iE "content-security-policy|x-frame-options|x-content-type-options|strict-transport-security|referrer-policy|permissions-policy"
```

---

## HTTP Request

```
GET / HTTP/2
Host: omgnepal.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36
```

---

## HTTP Response

```
HTTP/2 200
date: Wed, 04 Mar 2026 05:59:50 GMT
content-type: text/html; charset=UTF-8
server: cloudflare
cf-cache-status: DYNAMIC
cf-ray: 9d6ea83a8ba188d4-SIN
```

The curl command returns no output — none of the security headers are present.

---

## Expected Behavior

The server should include standard security headers in all responses to instruct the browser on content handling, framing restrictions, and transport security.

---

## Actual Behavior

None of the seven security headers are present in the HTTP response. The site relies entirely on application-level controls with no browser-level security policy enforcement.

---

## Evidence

curl output shows no security headers. Comparison with a hardened site (e.g. github.com) shows headers such as `x-frame-options: deny`, `x-content-type-options: nosniff`, and a full `content-security-policy` directive present in every response.
