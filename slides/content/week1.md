---
title: '01: intro'
layout: 'bundle'
outputs: ['Reveal']
---

## TODO: actually write some of my own stuff

---

{{< slide class="center" >}}

# week01

### COMP6443

---

## Good faith policy

We expect a high standard of professionalism from you at all times while you are taking any of our courses. We expect all students to act in good faith at all times

_TLDR: Don't be a dick_

[sec.edu.au/good-faith-policy](https://sec.edu.au/good-faith-policy)

---

{{% section %}}

## > whoami

-   George Muscat
-   4th Year CS (Security)

## how to contact me

-   Email: g.muscat@unsw.edu.au <-- Offical Stuff
-   Teams: George Muscat <-- Quicker Official Stuff
-   [@woop.xyz]() on the SecSoc Discord <-- Unofficial Stuff

---

## places for course discussion

-   EdStem Forum (found via WebCMS3) <-- Official
-   [secso.cc/discord](https://secso.cc/discord)
    -   #cs6443-6843

{{% /section %}}

---

## > whoareu

{{% section %}}

-   Your name, degree, year?
-   Why'd you do the course?
-   What time did you sleep last night?
-   Fun fact?

{{% /section %}}

---

## Questions

-   Are tuts compulsory? No
-   Where are these resources? [muscat.sh/6443]()

---

{{% section %}}

## Course content

-   Wargames (10%)
-   2 x PenTesting reports (40%)
-   Mid-term (0%) (Ask if not sure)
-   Final (50%)

---

## Wargames

-   don't leave them to the last minute, you'll be sad :(

-   you should do the majority of the hacking individually, however you can help each other when really stuck

-   within the same week, the number will (mostly) correspond with dificulty

-   most won't solve all the challenges. That is ok.

---

## Report

-   pentesting / vulnerability report
    -   groups of 3 in this tutorial
    -   **TAKE NOTES AS YOU GO**
    -   really important

{{% /section %}}

---

{{% section %}}

## Recon

-   passive & active

---

## Passive

Finding information about the target without interacting with the infra/site

A.K.A OSINT

-   Google dorking
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
-   Use these DNS servers: - Google - 8.8.8.8 - Cloudflare - 1.1.1.1 -
    {{% /section %}}

---

## Demo

> BurpSuite and ProxySwitchy oh my

---

## Activities

-   Form groups for the reports (3 people)
-   Installing burp suite & setting up certs
-   Signing up/logging into [ctfd.quoccabank.com]()
-   Try out some of the challenges!
    -   Recon stuffs
