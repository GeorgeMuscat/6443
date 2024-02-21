---
title: '02: Auth[ZN]'
layout: 'bundle'
outputs: ['Reveal']
---

{{< slide class="center" >}}

# week02

### COMP6443

Thanks Lachlan+Andrew for some of the slides

---

## Good Faith Policy

We expect a high standard of professionalism from you at all times while you are taking any of our courses. We expect all students to act in good faith at all times.

_TLDR: Don't be a dick_

[sec.edu.au/good-faith-policy](https://sec.edu.au/good-faith-policy)

---

## Admin

-   Challenges
    -   [WebCMS3](https://webcms3.cse.unsw.edu.au/COMP6443/24T1/resources/95202)
-   Create report groups (will do later)
-   Correction: Points to not necessarily indicate difficulty

---

## Challenges

-   Flags for approximately a CR
    -   Sales flag
    -   3 Blogs
    -   Files
    -   Support
-   Due Sunday 3/3

---

## Content

-   HTTP + TLS
    -   [Status Codes](https://http.cat/)
-   Authentication
    -   Sessions
    -   Tokens ([JWT](https://jwt.io/), Flask, Express etc)
    -   Cookies
-   Authorization
    -   Permission control
-   IDOR

---

{{% section %}}

## Authentication

-   Verifying that your identity is **authentic**
-   Factors of authentication are something you
    -   Know (e.g. Password)
    -   Are (e.g. Biometric)
    -   Have (e.g. Hardware Key + Cookie)
-   Status Code [401](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/401) Unauthorized
    -   When no valid authentication is sent

---

## Authentication

### Cookies

-   Cookies are how session persistent is commonly implemented
-   Usually authentication data + other data
    -   Session
    -   Authentication Token
    -   Ad tracking (still identification)
-   Set (and stored) by the server and sent to the client
-   Stored by the client and sent to the server
-   Target for hackers, why?

---

## Authentication

### How 2 Hax Cookies

-   Targeted because cookies are used as authentication
-   Can be stolen using:
    -   Cross-site scripting (XSS)
    -   MITM attacks
    -   Cross-site request forgery (CSRF)
-   Can be forged (baked)
    -   Poor implementation (e.g. incremental/plaintext)
    -   Unsigned

---

## Authentication

### Protecting The Cookie Jar

-   Expiry
-   HttpOnly (prevents XSS)
-   Secure (prevents MITM)
-   SameSite (prevents CSRF)

---

## Authorization

-   Controlling who has access to what (policy)
-   Principle of least privilege
-   Used with authentication to restrict access
-   Status Code [403](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/403) Forbidden

{{% /section %}}

---

{{% section %}}

## IDOR

Insecure Direct Object Reference Vulnerability

-   Examples of an object
    -   Users
    -   Pages
    -   Groups
-   Attackers can access/modify objects
-   Can be exploited when lacking authn/authz
-   Often due to deterministic ids

---

## Preventing IDOR

-   Use non-deterministic ids (e.g. UUIDs)
-   Strong authorisation policies
    -   Restrict access
    -   Constantly verify

{{% /section %}}

---

## Tools

-   python/nodejs libraries
-   [Flag Alert](https://featherbear.cc/UNSW-COMP6443/post/tools/flag_alert/) (Thanks Andrew)
-   [OWASP Cheatsheet](https://cheatsheetseries.owasp.org/index.html)
-   [hacktricks.xyz](https://book.hacktricks.xyz) (the bible)
-   [cookie-editor](https://cookie-editor.com/)
-   [jwt.io](https://jwt.io/)

Note: _There will always be better tools, go find them_

---

## Questions?

-   Did I miss something?
-   More depth?
