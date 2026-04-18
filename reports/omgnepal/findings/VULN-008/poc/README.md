# Proof of Concept — VULN-008

**Target:** omgnepal.com
**Type:** Information Disclosure — CTF Flag in robots.txt
**Endpoint:** https://omgnepal.com/robots.txt
**Tested on:** March 2026
**Researcher:** Vibhor Prasad (stack-guardian)

---

## Environment

- Tool: Browser, curl
- OS: Arch Linux
- Authenticated: No
- Account type: Unauthenticated

---

## Preconditions

None.

---

## Steps to Reproduce

1. Navigate to `https://omgnepal.com/robots.txt`.
2. Read the file contents.

---

## Payload

N/A — observation only.

---

## HTTP Request

```
GET /robots.txt HTTP/1.1
Host: omgnepal.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36
Connection: close
```

---

## HTTP Response

```
HTTP/1.1 200 OK
Content-Type: text/plain
Server: Apache

bugv{You_Found_Me}
```

---

## Expected Behavior

`robots.txt` should contain only crawl directives (`User-agent`, `Disallow`, `Allow`, `Sitemap`). It should not contain flags, credentials, internal paths, or any other sensitive information.

---

## Actual Behavior

The file contains a CTF flag: `bugv{You_Found_Me}`. Confirmed by the platform to be intentional.

---

## Evidence

Direct HTTP response from `https://omgnepal.com/robots.txt` contains the flag string. Screenshots were attached to the BugV submission.
