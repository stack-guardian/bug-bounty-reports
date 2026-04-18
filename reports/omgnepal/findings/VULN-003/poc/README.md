# Proof of Concept — VULN-003

**Target:** omgnepal.com
**Type:** Username Enumeration via Login Error Messages
**Endpoint:** https://omgnepal.com/wp-login.php
**Tested on:** March 2026
**Researcher:** Vibhor Prasad (stack-guardian)

---

## Environment

- Tool: Burp Suite, browser
- OS: Arch Linux
- Authenticated: No
- Account type: Unauthenticated

---

## Preconditions

None. The login page is publicly accessible.

---

## Steps to Reproduce

1. Navigate to `https://omgnepal.com/wp-login.php`.
2. Submit username `testuser123` (non-existent) with password `test`.
3. Note the error message returned.
4. Submit username `nareshlamgade` (valid, discovered via VULN-001) with password `test`.
5. Note the different error message returned.

---

## Payload

```
log=nareshlamgade&pwd=test&wp-submit=Log+In&redirect_to=%2Fwp-admin%2F&testcookie=1
```

---

## HTTP Request

```
POST /wp-login.php HTTP/2
Host: omgnepal.com
Content-Type: application/x-www-form-urlencoded
Referer: https://omgnepal.com/wp-login.php
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36

log=nareshlamgade&pwd=test&wp-submit=Log+In&redirect_to=%2Fwp-admin%2F&testcookie=1
```

---

## HTTP Response

Response body contains the error message:
```
The password you entered for username nareshlamgade is incorrect.
```

---

## Expected Behavior

The application should return an identical error message for both invalid usernames and valid usernames with wrong passwords, e.g.: "The username or password you entered is incorrect."

---

## Actual Behavior

The application returns distinct error messages:
- Non-existent username: `"Username not registered on this site."`
- Valid username, wrong password: `"The password you entered for username nareshlamgade is incorrect."`

This allows an attacker to confirm username validity without authentication.

---

## Evidence

Two screenshots were attached to the BugV submission showing the different error messages returned for an invalid username versus a valid username with a wrong password.
