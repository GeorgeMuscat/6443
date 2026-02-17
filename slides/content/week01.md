---
title: '01: intro'
layout: 'bundle'
outputs: ['Reveal']
---

{{< slide class="center" >}}

# week01

### COMP6443


---

## Good faith policy

We expect a high standard of professionalism from you at all times while you are taking any of our courses. We expect all students to act in good faith at all times.

_TLDR: Don't be a dick_

[sec.edu.au/good-faith-policy](https://sec.edu.au/good-faith-policy)

---

{{% section %}}

## > whoami

George Muscat
Senior Security Engineering Consultant @ Asontu

---

## how to contact me

Email: g.muscat@unsw.edu.au

---

## useful info

-   Everything is on WebCMS3
-   Lectures are very important

---

## places for course discussion

-  Discourse <-- Official
-   [secso.cc/discord](https://secso.cc/discord)
    -   #cs6443-6843

{{% /section %}}

---

{{% section %}}

## > whoareu

-   Your name, degree, year?
-   What time did you sleep last night?
-   Networks, frontend?

{{% /section %}}

---

## Questions

-   Are tuts compulsory? 
    - No, but I might be a bit biased and suggest that you come to all, or even multiple per week if you can
-   Where are these resources? [muscat.sh/6443]()

---

{{% section %}}

## Course content

-   Wargames (20%)
-   2 x PenTesting reports (15% + 15%)
    - Groups of 3 (can be cross tut)
-   Mid-term (0%) (Ask if not sure)
-   Final (50%)


**Check WebCMS3 and first Lecture**

---

## Wargames

-   marked individually

-   don't leave them to the last minute, you'll be sad :(

-   you should do the majority of the hacking individually, however you can help each other when really stuck

-   within the same week, the number will (mostly) correspond with difficulty

-   most won't solve all the challenges. That is ok.

---

## Report

-   pentesting / vulnerability report
    -   groups of 3
    -   **TAKE NOTES AS YOU GO**
    -   really important

{{% /section %}}

---

{{% section %}}

## Recon

passive & active

---

## Passive

Finding information about the target without interacting with the infra/site

A.K.A OSINT

-   Google Dorking
-   Certificate records
-   LinkedIn
-   Shodan

---

## Active

Anything where you are interacting with the infra/site

-   DNS (incl. bruteforcing)
-   Certificates
-   Endpoints
-   Source code
-   Headers

---

## Bruteforcing at Uni

> if you use automated tools, pls dont use uni DNS servers, use these :)

-   **NO BRUTEFORCING USING UNI DNS SERVERS**
-   Use these DNS servers:
    -   Google - 8.8.8.8
    -   Cloudflare - 1.1.1.1

---

## DNS

Things to know (or google):

-   DNS
-   A, AAAA, MX, PTR, TXT
-   DoH

---

# Tools

## Passive

-   nslookup
-   dig
-   Google dorking
-   crt.sh
-   DNS Dumpster
-   Wayback Machine
-   Wolfram Alpha

---

## Active

**NOTE**: When using these tools, ensure you rate limit yourself to avoid getting your IP restricted (Your family will hate this)

-   nmap
-   subbrute
-   sublist3r
-   dns recon

These tools need wordlists, Seclists is a good place to start

{{% /section %}}

---

## HTTP

Hyper Text Transfer Protocol

-   First lecture(s)!
-   Format is important
-   Headers are interesting and important
    -   Host
    -   Path
    -   User-Agent
    -   Content-\[Length/Type\]
    -   Origin
-   Method (GET/POST/PUT etc)

---

## Activities

-   Finish installing burp suite & setting up certs
-   Signing up/logging into [ctfd.quoccacorp.com]()
-   [Intro challenges](https://github.com/secedu/COMP6443-intro-challenges)
-   Try out some of the challenges!

---

## Demo

> BurpSuite

---
