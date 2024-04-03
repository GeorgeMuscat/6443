---
title: '07: Client Side Injection'
layout: 'bundle'
outputs: ['Reveal']
---

{{< slide class="center" >}}

# week07

### COMP6443

Thanks Lachlan+Andrew for some of the slides

---

## Good Faith Policy

We expect a high standard of professionalism from you at all times while you are taking any of our courses. We expect all students to act in good faith at all times.

_TLDR: Don't be a dick_

[sec.edu.au/good-faith-policy](https://sec.edu.au/good-faith-policy)

---

## Questions?

---

{{% section %}}

## Client Side Injection

-   WWW
    -   Everything is a document
    -   Origin vs Site
    -   HTML + JS
    -   DOM
-   XSS
    -   Source + Sink
    -   Goals
    -   Exfiltration Methods
-   CSRF

---

-   WAF Bypass
    -   Encoding
    -   Other tricks
-   Prevention
    -   CORS
    -   SOP
    -   CSP

{{% /section %}}

---

{{% section %}}

## WWW

-   World Wide Web
-   Everything is a document
    -   How those documents is client dependent
    -   HTML, JS, VBScript

---

## DOM

-   Document Object Model
-   An API for scripting languages to interact with web documents
-   document.cookie, location.url, window.onload,
-   [Mozilla](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)
-   We use the DOM to access + exfiltrate info

---

### HTML

-   Static (on its own)
-   Tags
    -   Paired: `<script>..</script>`
    -   Single: `<img .. />`
-   Manipulating the tags in HTML is useful
-   HTML encoding

---

### JS

-   Adds dynamic functionality
-   Inline HTML using `<script>` or loaded [externally](https://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html)
-   Separate files
-   Interacts with the DOM
    -   document.write()
    -   document.getElementById()
    -   document.cookie
    -   window.localstorage

---

### l33t notes

-   HTML is cAsE INSEnSItive
-   `<script>...</script>`
-   `<img onerror="..." src="x">`
-   `<svg/>`
-   `document.write()`
-   `fetch()`
-   [hacktricks.xyz](https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting)
-   [portswigger](https://portswigger.net/web-security)

{{% /section %}}

---

{{% section %}}

### Origin vs Site

What is an origin?

> <span style="color: #f1c232">http://</span><span style="color: #00b050">www\.website\.com</span><span style="color: #0000ff">:80</span>

origin = <span style="color: #f1c232">scheme</span> + <span style="color: #00b050">host</span> + <span style="color: #0000ff">port</span>

---

What is a site?

> <span style="color: #f1c232">http://</span><span style="color: #80ffba">www.</span><u><span style="color: #00b050">website</span><span style="color: #007133">.com</span></u><span style="color: #0000ff">:80</span> > <span style="color: #f1c232">https://</span><span style="color: #80ffba">mobile.</span><u><span style="color: #00b050">website</span><span style="color: #007133">.com</span></u><span style="color: #0000ff">:443</span> > <span style="color: #f1c232">ftp://</span><span style="color: #80ffba">f.</span><u><span style="color: #00b050">website</span><span style="color: #007133">.com</span></u><span style="color: #0000ff">:21</span>

site = <span style="color: #00b050">private_domain</span> + <span style="color: #007133">public_suffix</span>

-   <span style="color: #f1c232">Scheme</span> doesn't matter
-   <span style="color: #0000ff">Port</span> doesn't matter
-   Just the <u><span style="color: #00b050">domain</span></u>!

---

> <span style="color: #f1c232">http://</span><span style="color: #80ffba">www.</span><u><span style="color: #00b050">website</span><span style="color: #007133">.com</span></u><span style="color: #0000ff">:80</span>

&nbsp;

origin = <span style="color: #f1c232">scheme</span> + <span style="color: #80ffba">h</span><span style="color: #00b050">os</span><span style="color: #007133">t</span> + <span style="color: #0000ff">port</span>

&nbsp;

site = <span style="color: #00b050">private_domain</span> + <span style="color: #007133">public_suffix</span>
site = <u><span style="color: #00b050">dom</span><span style="color: #007133">ain</span></u>

{{% /section %}}

---

{{% section %}}

### XSS

-   User input is rendered as HTML
-   Variants
    -   Self
    -   Reflected
    -   Stored
    -   DOM

---

#### Reflected XSS

The payload is part of user input
_(i.e. URL bar, inside a cookie, etc)_

> Demo: [`Reflected XSS`](http://localhost:3000/reflected?q=%3Cimg+onerror%3D%22alert%28%27pwned%21%27%29%22+src%3D%22http%3A%2F%2Fgoogle.com%22%3E)

Who'd click on them though....
But also like, link shorteners...
But also like, "..."

---

#### Stored XSS

The payload is stored in some sort of database.

Arguably more dangerous...
Anyone who opens a page that returns content from that same database may be victim to a stored XSS attack

-   Twitter Self Retweeting
-   Myspace 'Samy' Worm

> Demo: Stored XSS

---

#### DOM-based XSS

The client pieces together data which eventually becomes an exploit itself.

> i.e. `document.write(...)`

{{% /section %}}

---

#### Mitigating XSS

{{% section %}}

-   Validation - Blacklisting / whitelisting of input
-   Sanitisation - Remove unsafe tags and attributes
-   Encode - Escape data so it's not a control character

---

### Defense

Don't use `.innerHTML` or `.outerHTML`

use `.innerText` or `.textContent`

> Demo: DOM XSS

JS Frameworks

---

> `X-XSS-Protection` header

Turn it off, it's broken 🔥 🌊🚒

✅ `X-XSS-Protection: 0`

-   Sometimes causes client-side errors on real code
-   Has its own [vulnerabilities](https://github.com/helmetjs/helmet/issues/230)

> <span style="font-size: 0.6em">"Firefox never supported X-XSS-Protection and Chrome and Edge have announced they <s>are dropping</s> have dropped support for it."</span>

---

> Sandboxing

iframes, sandbox, seamless, etc...

Link: [GitHub](https://github.com/featherbear/UNSW-CompClub2019Summer-CTF/commit/778c26087f0f6baa7b286037b4fe162aefd4ad67)

<!--
https://github.com/featherbear/UNSW-CompClub2019Summer-CTF/commit/778c26087f0f6baa7b286037b4fe162aefd4ad67
https://github.com/featherbear/UNSW-CompClub2019Summer-CTF/commit/7b6e5875f31fc1c12da9fbd9e149e833130fd4e2
-->

{{% /section %}}

---

#### Mitigating XSS Mitigations

-   Filter evasion [[1]](https://github.com/payloadbox/xss-payload-list) [[2]](https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting) [[3]](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html)
-   TL;DR Abuse bad sanitisation

---

### CSRF

{{% section %}}

Cross site request forgery

> "Heyyyy, look <a class="sparkle">here</a> 🥺 👉👈"

<script src="https://ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js"></script>
<script src="https://evejweinberg.github.io/sparkleHover/sparkleHover.js"></script>

<script>
    for (let elem of document.getElementsByClassName('sparkle')) {
        $(elem).sparkleHover({
            colors : ['#297E97', "#2EB8D5",'#36BEC1'],
            num_sprites: 22,
            lifespan: 3000,
            radius: 500,
            sprite_size: 40,
            shape: 'circle',
            image: 'http://loganchamber.org/files/Pumplin.jpg'

        })
    }
</script>

Tricking a user into making requests they didn't intend, sending data and loading attacker controlled documents

![](https://featherbear.cc/tutoring-unsw-21t2-cs6443-cs6843/week7-shared/Snipaste_2021-07-15_05-25-43.png)

---

##### CSRF Tokens

Stop CSRF attempts by supplying the user with a single-use 'nonce' value.

-   When the page loads, a nonce is included
    -   e.g. cookie, header, HTML form
-   The nonce must be passed in the request
-   Should be random and single use

_Can't forge a request if you don't know the nonce before hand... sort of..._

---

##### CSRF Token Attacks

-   Better hope the server is handling CSRF tokens properly
-   Single. Use. grr.
-   Forge?
-   Override?

---

> CSRF vs XSS

-   XS<u>S</u> - The JavaScript method
-   CSRF - The HTML method

Generally XSS is performed in the background
(since it's a script exploit)

{{% /section %}}

---

{{% section %}}

## CSP

[Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)

-   Only allow content from specific sources
-   Really useful (when used properly)
-   Nonce is used
-   Prevents
    -   `eval()`
    -   inline scripts
    -   iframes

---

### Implementation

-   Policy defined in HTTP headers or HTML tags
    -   HTTP is better
-   Various settings

---

### Beating CSP

-   Exploiting trust
    -   Hijack trusted source
    -   Redirection
    -   Inheritance
-   Turn off the restriction
    -   Insert own header
    -   [Split the response](https://github.com/featherbear/demo-response-header-splitting)

{{% /section %}}

---

## CORS

## [Cross-Origin Resource Sharing](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

-   Default browser behaviour to prevent sharing cross origin
    -   So cookies/local storage aren't shared
-   Set in HTTP headers
-   Set which origins other than its own that can load its resources
-   Used with SOP

---

## SOP

[Same-Origin Policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)

-   Restricts how resources from origin A (e.g. script) can interact with a resource from origin B
-   Applies only to scripts
    -   Can use non-scripting attacks to beat
-   Used with CORS

### Webhooks

-   Useful for exfiltrating data
-   [requestbin](https://public.requestbin.com/r)
-   [webhook.site](https://webhook.site/)

### WAF Bypass

-   can use / instead of spaces
    -   `<img/src="x"/onerror="..."/>
-   can use enter tab/newline in 'javascript:'
