---
title: '03: Auth & Trust'
layout: 'bundle'
outputs: ['Reveal']
---

{{< slide class="center" >}}

# Week03

---

## Feedback

less/same/more yapping?

---

{{% section %}}

## Report

Key things:

-   Report Groups
-   Format
-   Due date + submission

---

## Important Parts

-   **LINKED TABLE OF CONTENTS**
-   Linked appendix for:
    -   verbose definitions
    -   screenshots
    -   code
    -   long payloads
-   Should address vulnerability, not flag
-   Memes only outside of content

---

## Lectures

This is the law, watch.

---

## Examples

-   [Professional](https://www.offsec.com/reports/sample-penetration-testing-report.pdf)
-   [Andrew](https://docs.google.com/document/d/1dVXbABRPlAic2oNHqafXKrGmOYFSha-8_4kfLE_ilbQ/edit)

{{% /section %}}

---

## Content

-   PKI
    -   Certificates
    -   Trust
-   HSTS
-   Authentication
    -   MFA
    -   TOTP
    -   SSO + OAuth + SAML (Extended)
-   Secrets

---

{{% section %}}

## PKI

Public Key Infrastructure

-   Used to determine who is/owns something
-   Uses Certificates
-   Based on trust

---

## Certificates

-   TLS (and mTLS)
    -   [Handshake](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/)
-   Certificate Authorities (CA)
    -   CA issues certs
    -   CAs sign certs with their root cert
    -   Root certs are stored in OS/Browsers
-   Trust

{{% /section %}}

---

## HSTS

HTTP Strict Transport Security

-   Prevents MITM
-   Policy enforced by browsers
-   Common on financial and (some) govt. [sites](https://paypal.com/)

---

{{% section %}}

## AuthN 2.0

![/6443/assets/img/herewegoagain.jpg]

---

## MFA

Multi-factor Authentication

-   Requiring multiple factors
    -   Know
    -   Have
    -   Are
-   Avoid requiring same type of factor

---

## TOTP

Time-based One-Time Password

-   Unique codes
-   Short lifetime
-   [RFC6238](https://datatracker.ietf.org/doc/html/rfc6238)

---

## SSO + SAML + OAuth

-   Single Sign On
-   Security Assertion Markup Language
    -   Authentication
-   [OAuth 2.0](https://oauth.net/2/)
    -   Authorisation
-   Useful [Link](https://www.cloudflare.com/learning/access-management/what-is-oauth/)

{{% / section %}}
