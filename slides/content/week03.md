---
title: '03: Auth & Trust'
layout: 'bundle'
outputs: ['Reveal']
---

{{< slide class="center" >}}

# {{ replace .Name "-" " " | title }}

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

{{% /section %}}

---

## Content

-   PKI
    -   Certificates
    -   Trust
-   HSTS
-   Authentication
    -   MFA
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
-   Certificate Authorities (CA)
    -   CA issues certs
    -   CAs sign certs with their root cert
    -   Root certs are stored in OS/Browsers

---

## Trust

--

{{% /section %}}

##
