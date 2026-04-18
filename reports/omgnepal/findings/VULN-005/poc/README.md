# Proof of Concept — VULN-005

**Target:** omgnepal.com
**Type:** CORS Misconfiguration — Authenticated API Requests
**Endpoint:** https://omgnepal.com/wp-json/
**Tested on:** March 2026
**Researcher:** Vibhor Prasad (stack-guardian)

---

## Environment

- Tool: curl, Burp Suite
- OS: Arch Linux
- Authenticated: No
- Account type: Unauthenticated

---

## Preconditions

None.

---

## Steps to Reproduce

1. Run: `curl -s "https://omgnepal.com/wp-json/" -H "Origin: https://evil.com" -I | grep -i "access-control"`
2. Observe the response headers.

---

## Payload

```bash
curl -s "https://omgnepal.com/wp-json/" -H "Origin: https://evil.com" -I
```

---

## HTTP Request

```
GET /wp-json/ HTTP/1.1
Host: omgnepal.com
Origin: https://evil.com
```

---

## HTTP Response

```
access-control-expose-headers: X-WP-Total, X-WP-TotalPages, Link
access-control-allow-headers: Authorization, X-WP-Nonce, Content-Disposition, Content-MD5, Content-Type
access-control-allow-origin: https://evil.com
access-control-allow-methods: OPTIONS, GET, POST, PUT, PATCH, DELETE
access-control-allow-credentials: true
```

---

## Expected Behavior

`Access-Control-Allow-Origin` should be restricted to trusted origins only and must not be combined with `Access-Control-Allow-Credentials: true` for untrusted origins.

---

## Actual Behavior

The server reflects the attacker-controlled origin and permits credentialed cross-origin requests from any domain.

---

## Evidence

curl output confirms both headers present. Identical to VULN-004 — this was a separate submission focusing on the authenticated API access angle.
