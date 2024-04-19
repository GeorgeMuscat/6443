---
layout: page
title: list
---

-   Robots
-   Sitemap

-   Use the app
    -   Network Activity
        -   Headers
            -   Possible sinks
            -   Security policies
            -   Cookies
                -   HttpOnly, secure?
                -   Domain, path
        -   Where are they going?
        -   When do they occur?
        -   What data is being sent?
-   Source code

    -   Comments (CTF moment)
    -   Endpoints (esp. hidden)
    -   Scripts
        -   Relative path?
        -   Where is the script from?
        -   CSP
            -   Nonce
            -   Which rules exactly?
        -   Inline

-   XSS

    -   Sources and Sinks
        -   Use webhooks to enumerate
    -   CSP
        -   Can we:
            -   Response split
            -   Hijack Trust
            -   Forge Nonce
    -   Test the WAF
        -   Think about what rules it could be using

-   SQLi

    -   Break query
    -   Rebuild query
    -   Are there errors?
        -   If so, where?
            -   Rendered
            -   Console
            -   Responses
        -   What do they look like?
    -   Escape the query
        -   `'")`
        -   Comment at the end ` -- a`
    -   What DB?
        -   MySQL: information_schema
            -   information_schema.tables
            -   information_schema.columns
    -   Number of columns being rendered/returned
    -   Useful SQL keywords
        -   OR + AND 1=1/1=0
        -   GROUP BY
        -   LIMIT
        -   OFFSET
        -   ORDER BY
        -   UNION

-   SSTI

    -   What templating engine?
        -   Jinja?
    -   {{ 1 + 1 }}
    -   RCE
        -   Python MRO

-   Processed HTML

'"<img src=x>{{ 1 + 1}}
