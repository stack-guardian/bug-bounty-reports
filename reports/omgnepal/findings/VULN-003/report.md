# VULN-003 — Username Enumeration via Login Error Messages

**Target:** omgnepal.com
**Platform:** BugV
**Researcher:** Vibhor Prasad (stack-guardian)
**BugV Report ID:** #000094R
**Date Submitted:** March 2026
**Status:** Won't Fix

---

## Summary

The WordPress login page at omgnepal.com returns different error messages depending on whether a submitted username exists in the database. When an invalid username is submitted, the response reads "Username not registered on this site." When a valid username is submitted with a wrong password, the response reads "The password you entered for username [name] is incorrect." This difference allows an unauthenticated attacker to confirm which usernames are valid without needing to authenticate.

---

## Vulnerability Details

| Field                   | Value                                                  |
|-------------------------|--------------------------------------------------------|
| Type                    | Username Enumeration via Differential Error Messages   |
| CWE                     | CWE-204: Observable Response Discrepancy               |
| Severity                | Low                                                    |
| CVSS v3.1 Score         | 5.3                                                    |
| CVSS v3.1 Vector        | AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N                  |
| Affected Endpoint       | https://omgnepal.com/wp-login.php                      |
| Vulnerable Parameter    | `log` (username field, POST body)                      |
| Authentication Required | No                                                     |
| Discovery Method        | Manual testing via Burp Suite                          |
| Tools Used              | Burp Suite, browser                                    |

---

## Reproduction Steps

1. Navigate to `https://omgnepal.com/wp-login.php`.
2. Submit a non-existent username (e.g. `testuser123`) with any password.
3. Observe the error: **"Username not registered on this site."**
4. Submit a known valid username (e.g. `nareshlamgade`) with a wrong password.
5. Observe the error: **"The password you entered for username nareshlamgade is incorrect."**
6. The difference in error messages confirms which usernames are valid.

---

## Proof of Concept

See [poc/README.md](./poc/README.md) for full reproduction details.

### HTTP Request (valid username, wrong password)

```
POST /wp-login.php HTTP/2
Host: omgnepal.com
Content-Type: application/x-www-form-urlencoded
Referer: https://omgnepal.com/wp-login.php

log=nareshlamgade&pwd=test&wp-submit=Log+In&redirect_to=%2Fwp-admin%2F&testcookie=1
```

### Observed Behavior

- Invalid username → `"Username not registered on this site."`
- Valid username + wrong password → `"The password you entered for username nareshlamgade is incorrect."`

---

## Impact

An attacker can use this differential response to build a confirmed list of valid WordPress usernames. Combined with VULN-001 (author ID enumeration) and VULN-002 (XMLRPC brute force), this creates a complete attack chain: enumerate usernames, confirm them via login errors, then brute force credentials at scale via XML-RPC.

---

## Remediation

Return a generic, identical error message for all failed login attempts regardless of whether the username exists:

```php
// In wp-login.php or via a plugin — return uniform error
add_filter('login_errors', function() {
    return 'The username or password you entered is incorrect.';
});
```

---

## References

- [OWASP — Testing for Account Enumeration](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account)
- [CWE-204: Observable Response Discrepancy](https://cwe.mitre.org/data/definitions/204.html)

---

## Timeline

| Date          | Event                                    |
|---------------|------------------------------------------|
| March 2026    | Vulnerability identified                 |
| March 2026    | PoC confirmed and documented             |
| March 2026    | Report submitted via BugV (#000094R)     |
| March 2026    | Platform acknowledged receipt            |
| March 2026    | Status assigned: Won't Fix               |
| April 2026    | Added to portfolio                       |
