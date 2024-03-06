---
title: '04: Injection (pt 1)'
layout: 'bundle'
outputs: ['Reveal']
---

{{< slide class="center" >}}

# Week04

{{% section %}}

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

## Demo

[Demo](https://www.w3schools.com/sql/trysql.asp?filename=trysql_asc)
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

    **MySQL**: information*schema.[tables|columns]
    **SQLite**: sqlite*[master|schema]
    **SQL Server**: SHOW TABLES; DESCRIBE <table_name>

---

{{% / section %}}
