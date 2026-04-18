# VULN-001 — WordPress Username Enumeration via Author ID Parameter

**Target:** omgnepal.com
**Platform:** BugV
**Researcher:** Vibhor Prasad (stack-guardian)
**Report ID:** #000094Q
**Date Reported:** March 2026
**Status:** Duplicate (of #000004F, originally submitted Mar 07, 2021)

---

## Summary

The WordPress installation at omgnepal.com exposes all registered usernames through the `?author=` query parameter. By incrementing the numeric ID value, an unauthenticated attacker can enumerate every valid username on the site. Ten usernames were confirmed during testing. This information directly enables targeted brute-force attacks, credential stuffing, phishing, and social engineering against identified staff members.

---

## Details

| Field                   | Value                                                  |
|-------------------------|--------------------------------------------------------|
| Type                    | Username Enumeration                                   |
| Severity                | Low                                                    |
| CVSS Score              | 5.3 (AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N)            |
| CWE                     | CWE-200: Exposure of Sensitive Information             |
| Affected Endpoint       | https://omgnepal.com/?author={id}                      |
| Vulnerable Parameter    | `author` (GET)                                         |
| Authentication Required | No                                                     |

---

## Reproduction Steps

1. Open a browser or HTTP client (no authentication required).
2. Navigate to `https://omgnepal.com/?author=1`.
3. Observe the 301 redirect to `https://omgnepal.com/author/nareshlamgade/` — the username is exposed in the URL.
4. Increment the ID and repeat: `?author=4`, `?author=7`, etc.
5. Collect all valid usernames from the redirect destinations.

---

## Proof of Concept

See [poc/README.md](./poc/README.md) for full reproduction details.

**Usernames discovered:**

| Author ID | Username      |
|-----------|---------------|
| 1         | nareshlamgade |
| 4         | aditya        |
| 7         | drojee        |
| 8         | prajita       |
| 10        | rashmi        |
| 12        | diwas         |
| 14        | ashishrana    |
| 15        | vineet        |
| 16        | subeksya      |
| 17        | gunjan        |

---

## Impact

An attacker with a valid username list can:

- **Brute-force the WordPress login** (`/wp-login.php`) using the enumerated usernames with common password lists
- **Credential stuffing** — test the usernames against other services where staff may reuse passwords
- **Targeted phishing** — craft convincing spear-phishing emails addressed to identified staff members by name
- **Social engineering** — use real employee names and roles to impersonate or manipulate staff

The vulnerability requires no authentication and is trivially automated with a simple loop or a tool like `wpscan`.

---

## Remediation

1. **Disable author archive redirects** — add the following to `functions.php` to prevent `?author=` from redirecting to the author slug:

   ```php
   add_action('template_redirect', function() {
       if (is_author()) {
           wp_redirect(home_url(), 301);
           exit;
       }
   });
   ```

2. **Remove author slugs from URLs** — use a plugin such as *Edit Author Slug* to replace usernames with non-guessable identifiers in author archive URLs.

3. **Restrict the REST API user endpoint** — the `/wp-json/wp/v2/users` endpoint also exposes usernames; restrict it to authenticated requests:

   ```php
   add_filter('rest_endpoints', function($endpoints) {
       if (isset($endpoints['/wp/v2/users'])) {
           unset($endpoints['/wp/v2/users']);
       }
       return $endpoints;
   });
   ```

4. **Enforce strong, unique passwords** and enable two-factor authentication on all WordPress accounts to reduce the impact of enumeration.

---

## References

- [OWASP — Username Enumeration](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account)
- [CWE-200: Exposure of Sensitive Information to an Unauthorized Actor](https://cwe.mitre.org/data/definitions/200.html)
- [WordPress Hardening Guide — Disable Author Archives](https://wordpress.org/documentation/article/hardening-wordpress/)

---

## Timeline

| Date          | Event                                    |
|---------------|------------------------------------------|
| March 2026    | Vulnerability identified during recon    |
| March 2026    | Report submitted via BugV (#000094Q)     |
| March 2026    | Marked as duplicate by platform (#000004F was the original, submitted Mar 07, 2021) |
| April 2026    | Added to portfolio after duplicate triage |
