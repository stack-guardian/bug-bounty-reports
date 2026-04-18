# Proof of Concept — VULN-004

**Target:** omgnepal.com
**Type:** CORS Misconfiguration — Wildcard Origin Reflection
**Endpoint:** https://omgnepal.com/wp-json/
**Tested on:** March 2026
**Researcher:** Vibhor Prasad (stack-guardian)

---

## Environment

- Tool: curl, Burp Suite
- OS: Arch Linux
- Authenticated: No (to trigger the misconfiguration)
- Account type: Unauthenticated

---

## Preconditions

None. The endpoint is publicly accessible.

---

## Steps to Reproduce

1. Run the following curl command from any machine.
2. Observe the `access-control-allow-origin` and `access-control-allow-credentials` response headers.

---

## Payload

```bash
curl -s "https://omgnepal.com/wp-json/" -H "Origin: https://evil.com" -I | grep -i "access-control"
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

The server should either return no `Access-Control-Allow-Origin` header, or return it only for explicitly trusted origins. It must not reflect the request's `Origin` value directly, especially when combined with `Access-Control-Allow-Credentials: true`.

---

## Actual Behavior

The server reflects the attacker-supplied `Origin: https://evil.com` directly into `Access-Control-Allow-Origin: https://evil.com` and simultaneously sets `Access-Control-Allow-Credentials: true`, permitting cross-origin authenticated requests from any domain.

---

## Evidence

curl output confirms both headers are present in the response. The `access-control-allow-origin` value exactly matches the attacker-controlled `Origin` header sent in the request.
