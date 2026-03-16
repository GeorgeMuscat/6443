---
title: 'Request Smuggling'
layout: 'bundle'
outputs: ['Reveal']
---

{{< slide class="center" >}}

<style type="text/css">
p { text-align: left; }
</style>

# HTTP Request Smuggling

---

{{% section %}}

## > whoami

George Muscat

Senior Security Engineering Consultant @ Asontu

---

## how to contact me

Email: g.muscat@unsw.edu.au

{{% /section %}}

---

## Why this matters

- Production systems are multi-hop:
  - e.g. CDN → WAF → reverse proxy → app server
- Each hop can parse HTTP differently
- One non-compliant parser in the chain can break end-to-end safety

---

{{% section %}}

## Typical HTTP Request Handling

- Server listens on a socket that provides raw bytes
- Server reads those bytes to a buffer and parses them into HTTP requests
- If there are leftover bytes, the server leaves them in buffer to append future bytes received on the socket

---

## Basic Example

```py
# Accept an incoming connection
client_connection, _ = listen_socket.accept()

# Manually buffer the request data
request_data = b""
while True:
    # Receive bytes in chunks
    data = client_connection.recv(1024)
    request_data += data
    # Check if the end of the HTTP headers is reached
    if b'\r\n\r\n' in request_data:
        # If this is a POST request, we would need to do more stuff here...
        break
# parse request_data further
```

{{% / section %}}

---

{{% section %}}

## RFC grounding (HTTP/1.1)

Key specs to know:

- **RFC 9112** — HTTP/1.1 Message Syntax and Routing
- **RFC 9110** — HTTP Semantics (fields, methods, status semantics)

For smuggling, RFC 9112 framing rules are the core.

---

## RFC 9112: message body length precedence

RFC 9112 defines strict message delimitation behavior.

1. `Transfer-Encoding` (if present and valid) governs decoding/framing
2. `Content-Length` is used when `Transfer-Encoding` is absent
3. Ambiguous or invalid framing must be treated as an error

If two components apply different precedence, desync emerges.

---

## RFC requirement that matters most

When both `Transfer-Encoding` and `Content-Length` appear together, this is dangerous ambiguity.

Best practice is to reject such requests.

If one tier accepts and another rejects/interprets differently, request smuggling becomes possible.

{{% / section %}}


---

## Core idea of request smuggling

Attacker sends one byte stream/packet that frontend and backend parse differently:
- frontend: sees one complete request
- backend: sees two requests

The smuggled bytes are then prepended to the next request in queue on the backend.

---

## Typical architecture

Client → Frontend Proxy → Backend App

Vulnerability condition:

- frontend parser behavior ≠ backend parser behavior
- shared persistent connections carry multiple users’ requests

Result: attacker controls parsing context for subsequent traffic.

---

{{% section %}}


## CL.TE desync

Frontend uses `Content-Length`  
Backend uses `Transfer-Encoding`

- frontend effectively ignores `Transfer-Encoding`
- backend decodes chunked framing

This causes leftover bytes to become a smuggled request.

---

## TE.CL desync

Frontend uses `Transfer-Encoding`  
Backend uses `Content-Length`

- backend does not apply chunked decoding semantics as expected
- or trusts `Content-Length` in an ambiguous message

Frontend and backend disagree on request end, producing queue desync.

---

## TE.TE desync

Both claim to support `Transfer-Encoding`, but differ in parsing details:

- duplicate `Transfer-Encoding` headers handled inconsistently
- casing/whitespace quirks
- invalid codings tolerated by one parser, rejected by another
- different behavior for malformed chunk extensions/terminators

{{% / section %}}


---

## Header parsing and line handling pitfalls

Common root causes tied to standards handling:

- lenient parsing of invalid header syntax
- inconsistent folding/whitespace acceptance
- conflicting duplicate field handling policies
- tolerance for malformed chunk-size lines

---

## Conceptual CL.TE example

Attacker sends both:

- `Content-Length: 40`
- `Transfer-Encoding: chunked`

Frontend reads fixed 40-byte body.  
Backend stops at chunk terminator and leaves trailing bytes queued.

Those queued bytes become the start of the next backend request.

---



# DEMO

---

## Reminder

Smuggling generally needs:

- persistent backend connection reuse
- mixed parser implementations
- different error-handling decisions for malformed/ambiguous input

An RFC violation alone may be benign in isolation.

---

## Practical impact

- Request hijacking / prefix injection
- Web cache poisoning
- Authentication/session confusion
- Access-control bypass via route/header confusion
- Internal endpoint reachability through smuggled internal-style requests

These are downstream effects of one upstream parsing flaw.

---

## HTTP/2 downgrade issues

- H2 frontends often downgrade to H1 backends.

- Risk point: translator emits H1 with imperfect normalization/framing controls

- Vulnerable component is frequently H2→H1 conversion, not the H2 client link itself.

---

## Detection signals

Watch for:
- `400/502` clusters after malformed requests
- unexplained backend timeouts / queue anomalies
- cache key/content mismatches

---

## Mitigations 

At the trusted edge (e.g. CDN, reverse proxy):
- HTTP/2 for all jumps in the network with no downgrade
- Reject ambiguous framing (`CL` with `TE`)
- Enforce strict RFC-conformant header syntax
- Reject malformed chunked encoding
- Normalize or reject duplicate hop-by-hop fields

Goal: remove ambiguity before traffic enters shared pools.

---

## Recent real world examples

- [Pingora](https://blog.cloudflare.com/pingora-oss-smuggling-vulnerabilities/) < v0.8.0 
  - CVE-2026-2833
  - CVE-2026-2835
  - CVE-2026-2836. 

---

## Summary

HTTP request smuggling is best understood as:

> Exploitation of inconsistent HTTP message framing decisions across components that do not adhere uniformly to RFC-defined behavior.

If two components disagree on where a request ends, attackers can control what comes next.

---

{{< slide class="center" >}}

# Questions?
