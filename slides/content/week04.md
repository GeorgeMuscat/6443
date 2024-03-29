---
title: '04: Injection (pt 1)'
layout: 'bundle'
outputs: ['Reveal']
---

{{< slide class="center" >}}

# Week04

Thanks Lachlan and Andrew for some content

---

## Review

Any questions about previous content?

---

## Content

-   Injection
    -   SQLi
    -   SSTI
-   SSRF

---

## Injection

Injection attacks occur when an attacker injects malicious data that is then used to control program behaviour

---

{{% section %}}

## SQL

Structured Query Language

-   Language for querying databases
-   Implementations include:
    -   PostgreSQL
    -   MySQL
    -   SQLite
    -   SQL Server (Microsoft)

---

## SQL Statements

-   SELECT _ FROM _ ...
-   INSERT INTO \_ (COLn, ...) VALUES (VALn, ...)
-   UPDATE _ SET _ = \_ ...
-   DELETE FROM \_ ...
-   ... -- this is a comment
-   ... # this can also be a comment (sometimes)
    …

---

## SQL Syntax

-   WHERE

    -   \> - greater than
    -   < - less than
    -   = - equal to
    -   <> - not equal to

-   LIKE
    -   % - wildcard
-   UNION
    ...

---

### More SQL Syntax

-   ORDER BY
-   GROUP BY
-   DISTINCT
-   LIMIT
-   OFFSET

## Demo

## [Demo](https://www.w3schools.com/sql/trysql.asp?filename=trysql_asc)

{{% / section %}}

---

{{% section %}}

## SQLi

what?

-   User input contains control characters that interfere with the SQL statement

Why?

-   Bad programmers - The user input is being trusted to be a valid format.

---

## SQLi

#### Code before injection

`SELECT a FROM b WHERE a = '$userInput'`

#### Code after injection

`                          vvvvvvvvvvvvv
SELECT a FROM b WHERE a = '' OR '1' = '1'
                           ^^^^^^^^^^^^^`

---

## SQLi

What can you do with SQLi?

-   Bypass logins
-   Leak data
-   Spoof a user
-   Modify data (don't try in 6443)

---

## SQLi

You should be asking:

-   How can we tell what implementation is being used?
-   How do we know what tables/columns exist?

---

## Fingerprinting

-   Different implementations will have different artifacts
    -   **MySQL**: Version()
    -   **SQLite**: sqlite_version()
    -   **SQL Server**: @@Version
-   Seem more [here](https://www.sqlinjection.net/database-fingerprinting/) and [here](https://portswigger.net/web-security/sql-injection/examining-the-database)

---

## Schema Recon

-   what tables exist, what do they look like?

    -   **MySQL**: `information_schema.[tables|columns]`
    -   **SQLite**: `sqlite_[master|schema]`
    -   **SQL Server**: `SHOW TABLES; DESCRIBE <table_name>`

---

# Demo

Thanks Andrew

{{% / section %}}

---

## SQLi Mitigation

-   Disable debug logging
    -   No error messages, maybe just a blank screen?
-   WAF - Web Application Firewalls
    -   Reject/strip malicious payloads
-   Parameterised Queries

---

## Beating Mitigations

-   Payload stripped? Embed dummies
-   Blank response? Side channel attacks
    -   Timing Attacks
    -   Out of Band Attacks i.e inbuilt functions (therefore fingerprint!)
-   Error-based extraction
-   Boolean-based extraction
-   Subqueries
    -   SELECT a,b FROM c WHERE d UNION SELECT (SELECT ...), 2

---

## Remember

-   The payloads can be complex
-   Reporting a vulnerability != extracting data
-   Big database? - COUNT or LIMIT + OFFSET it instead
-   No SQLmap or automated SQL enumeration/exploitation
-   Don't try to DROP any tables

---

{{% section %}}

## Other Injection

### SSTI

Server side template injection

-   Jinja
-   Twig
-   Plates

---

## SSTI Example

In the backend:
`output = template.render(name=request.args.get('name'))`

Attacker requests:
`http://vulnerable-website.com/?name={{bad-stuff}}`

`bad-stuff` could be code to execute

---

## SSTI Demo

Thanks Lachlan

{{% / section %}}

---

{{% section %}}

## SSRF

Server-side request forgery

-   HaaS
-   Making a server in the target network make request on your behalf
-   Used to access servers that might not be public
-   Some servers trust internal requests (bad authz)

---

## Why SSRF

Utilising functionalities of a server to access resources

-   Information retrieval
-   Can lead to RCE
-   Server Side Includes
-   Horizontal/vertical priv. esc.

---

## Mitigation

-   Whitelist domains and IPs!
-   Lower the access control of services
-   Set limits! exec time, file sizes, recursion depth
-   Zero Trust
    -   Local devices should NOT be assumed to be safe

{{% / section %}}

---
