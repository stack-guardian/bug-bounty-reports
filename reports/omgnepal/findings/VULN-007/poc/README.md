# Proof of Concept — VULN-007

**Target:** omgnepal.com
**Type:** Analytics Cookies Missing HttpOnly and Secure Flags
**Endpoint:** https://omgnepal.com
**Tested on:** March 2026
**Researcher:** Vibhor Prasad (stack-guardian)

---

## Environment

- Tool: Browser DevTools
- OS: Arch Linux
- Authenticated: No
- Account type: Unauthenticated

---

## Preconditions

None.

---

## Steps to Reproduce

1. Visit `https://omgnepal.com`.
2. Open DevTools → Application tab → Storage → Cookies → omgnepal.com.
3. Inspect the `HttpOnly` and `Secure` columns for `_ga`, `_gid`, `_gat_gtag_UA_58097238_1`, and `FCNEC`.

---

## Payload

N/A — observation only.

---

## HTTP Request

```
GET / HTTP/2
Host: omgnepal.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36
```

---

## HTTP Response

```
set-cookie: _ga=GA1.2.123456789.1234567890; Path=/; Domain=omgnepal.com
set-cookie: _gid=GA1.2.987654321.1234567890; Path=/; Domain=omgnepal.com
set-cookie: _gat_gtag_UA_58097238_1=1; Path=/; Domain=omgnepal.com
```

---

## Expected Behavior

All cookies should include `Secure` and `HttpOnly` flags as a baseline security practice.

---

## Actual Behavior

The `HttpOnly` and `Secure` attributes are absent from all analytics cookies. They are accessible via JavaScript (`document.cookie`) and would be transmitted over HTTP if a downgrade were possible.

---

## Evidence

Browser DevTools Application tab shows empty `HttpOnly` and `Secure` columns for the affected cookies. Screenshots were attached to the BugV submission.
