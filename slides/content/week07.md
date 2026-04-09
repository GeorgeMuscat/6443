---
title: '07: Client Side Injection'
layout: 'bundle'
outputs: ['Reveal']
---

{{< slide class="center" >}}

# week07

### COMP6443

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

The payload is part of a HTTP request, that is injected in the immediate HTTP response.
- GET request parameters
- Cookies
- POST request body

---

#### Stored XSS

The payload is stored serverside (e.g. file or database).

Typically more dangerous than reflected.
Anyone who opens a page that returns content from that same database may be victim to a stored XSS attack.

-   Twitter Self Retweeting
-   Myspace 'Samy' Worm

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

{{% /section %}}

---

#### Mitigating XSS Mitigations

-   Filter evasion [[1]](https://github.com/payloadbox/xss-payload-list) [[2]](https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting) [[3]](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html)
-   TL;DR Abuse bad sanitisation

---

{{% section %}}

### CSRF


Cross site request forgery

Tricking a user into making requests they didn't intend, sending data and loading attacker controlled documents

---

##### CSRF Tokens

Stop CSRF attempts by supplying the user with a single-use 'nonce' value.

-   When the page loads, a nonce is included
    -   e.g. cookie, header, HTML form
-   The nonce must be passed in the request
-   Should be random and single use (per request OR per session, depending on system design)

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

## SOP

[Same-Origin Policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)

-   Default protection in browsers
-   Restricts how resources from origin A (e.g. script) can interact with a resource from origin B
-   Typically allows embedding/using the resource, but doesn't allow "reading" from javascript.
  - For example you can load & execute JS hosted at CDN.com/code.js on sitea.com, but you cannot read the contents of code.js within Javascript on sitea.com
  - [more info](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Same-origin_policy#cross-origin_network_access)
-   Used with CORS to make sites functional but secure

---

{{% section %}}

## CORS

## [Cross-Origin Resource Sharing](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

-   Allows the server to specify to the client what parts of the SOP it needs to relax/ignore
-   Set in HTTP headers
-   Tells clients which sites (e.g. siteb.com) are allowed to read resources from itself (sitea.com)
-   Browser uses preflight requests to check for CORS policies before making further requests
-   Bad devs often set overly permissive CORS policies

---

## Example

- We host the frontend on `https://app.example.com/`
- The frontend interacts with an API hosted at `https://api.example.com/`
- SOP will block requests to this by default
- We need to use CORS to allow this.


---

Preflight response headers (repsonse to an OPTIONS request to `https://api.example.com/`):
```http
Access-Control-Allow-Origin: https://app.example.com
Access-Control-Allow-Credentials: true
Access-Control-Allow-Methods: GET, POST, PUT, PATCH, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization, X-Request-ID
Access-Control-Max-Age: 600
Vary: Origin
```


---

Response to actual request (e.g. PUT,POST)
```http
Content-Type: application/json
Access-Control-Allow-Origin: https://app.example.com
Access-Control-Allow-Credentials: true
Access-Control-Expose-Headers: X-Request-ID, X-RateLimit-Remaining
Vary: Origin
```

{{% /section %}}

---

### Webhooks

-   Useful for exfiltrating data
-   [requestbin](https://public.requestbin.com/r)
-   [webhook.site](https://webhook.site/)

---

### WAF Bypass

-   can use / instead of spaces
    -   `<img/src="x"/onerror="..."/>
-   can use enter tab/newline in 'javascript:'
