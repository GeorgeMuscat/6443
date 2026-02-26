---
title: '02: Auth[ZN]'
layout: 'bundle'
outputs: ['Reveal']
---

{{< slide class="center" >}}

# week02

### COMP6443

---

## Good Faith Policy

We expect a high standard of professionalism from you at all times while you are taking any of our courses. We expect all students to act in good faith at all times.

_TLDR: Don't be a dick_

[sec.edu.au/good-faith-policy](https://sec.edu.au/good-faith-policy)

---

## Admin

-   Challenges
  - [Marking](https://webcms3.cse.unsw.edu.au/COMP6443/26T1/resources/119416)
-   Create report groups (will do later)

---

## Challenges

-   Flags for approximately a CR
    -   Sales flag
    -   3 Blogs
    -   Files
    -   Support

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
    -   Have (e.g. Hardware Key)
-   Status Code [401](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/401) Unauthorized
    -   When no valid authentication is sent (yes i know it says unauthorized...)

--- 

## Passwords

- Hashing
  - One way function
- Salting
- Just use argon2 (or bcrypt)
  
---

### MFA

Multi-factor Authentication

- TOTP
  - QR Code

--- 

### Cookies

-   Cookies are used for session persistence
-   Usually authentication data + other data
    -   Session
    -   Authentication Token
    -   Ad tracking (still identification)
-   Set (and _usually_ stored) by the server and sent to the client
-   Stored by the browser and sent to the server in future requests
-   Target for hackers, why?

---

### How 2 Hax Cookies

-   Cookies are used as authentication
-   Can be stolen using:
    -   Cross-site scripting (XSS)
    -   MITM attacks
-   Can be forged (baked)
    -   Poor implementation (e.g. incremental/plaintext)
    -   Unsigned
-   Poor cookie protection can be used using:
    -   Cross-site request forgery (CSRF)

---

### Protecting The Cookie Jar

Cookie Settings
-   Expiry
-   HttpOnly (prevents XSS)
-   Secure (prevents MITM)
-   SameSite (prevents CSRF)

{{% /section %}}

---

## Authorization

-   Controlling who has access to what (policy)
-   Used with authentication to restrict access
-   Principle of least privilege
-   Status Code [403](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/403) Forbidden


---

## SSO


Single-sign on

- SAMLv2
- OIDC
- Outsources authentication to a trusted provider
- Use this where possible

---

{{% section %}}

## OAuth 2.0

Standard to grant authorization to `Application A` to access resources on `Application B`

---

### OAuth 2.0

- Examples:
  - Google, Github, Discord
- Able to be scoped to adhere to principal of least privilege
- Allows for `Application A` to access resources you control on `Application B` without your password
- Doesn't inherently provide *authentication*
  - OIDC builds on OAuth 2.0 to provide authentication.

---

### OAuth 2.0 Flow

![OAuth 2.0 Flow](https://upload.wikimedia.org/wikipedia/commons/7/72/Abstract-flow.png)

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
    -   Constantly verify authZ/N

{{% /section %}}

---

## Tools

-   python/nodejs libraries
-   [Flag Alert](https://featherbear.cc/UNSW-COMP6443/post/tools/flag_alert/) (Thanks Andrew)
-   [OWASP Cheatsheet](https://cheatsheetseries.owasp.org/index.html)
-   [hacktricks.xyz](https://book.hacktricks.xyz) (the bible)
-   [cookie-editor](https://cookie-editor.com/)
-   [jwt.io](https://jwt.io/)
-   [CyberChef](https://gchq.github.io/CyberChef/)

Note: _There will always be better tools, go find them_

---

## Questions?

-   Did I miss something?
-   More depth?
-   Any intro challenge you want me to talk through?
