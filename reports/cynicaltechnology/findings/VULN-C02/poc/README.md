# Proof of Concept — VULN-C02

**Target:** cynicaltechnology.com
**Type:** CORS Misconfiguration — Arbitrary Origin Reflection
**Endpoint:** https://cynicaltechnology.com/wp-json/
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

1. Run the curl command below.
2. Observe the `access-control-allow-origin` and `access-control-allow-credentials` headers in the response.

---

## Payload

```bash
curl -s "https://cynicaltechnology.com/wp-json/wp/v2/users" -H "Origin: https://evil.com" -I | grep -i "access-control"
```

---

## HTTP Request

```
GET /wp-json/wp/v2/users HTTP/1.1
Host: cynicaltechnology.com
Origin: https://evil.com
```

---

## HTTP Response

```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://evil.com
Access-Control-Allow-Credentials: true
Access-Control-Allow-Methods: OPTIONS, GET, POST, PUT, PATCH, DELETE
Access-Control-Allow-Headers: Authorization, Content-Type
Content-Type: application/json
```

---

## Expected Behavior

The server should not reflect the `Origin` header. `Access-Control-Allow-Origin` should be set to a specific trusted domain or omitted entirely for untrusted origins.

---

## Actual Behavior

The server reflects the attacker-controlled `Origin: https://evil.com` directly into `Access-Control-Allow-Origin` and sets `Access-Control-Allow-Credentials: true`, permitting credentialed cross-origin requests from any domain.

---

## Evidence

curl output confirms both headers present in the response. The `access-control-allow-origin` value exactly matches the attacker-controlled `Origin` header.
