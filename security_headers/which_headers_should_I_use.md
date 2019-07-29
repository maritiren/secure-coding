# Which security headers should I use?

There is a lot of talk about, and I agree, that adding security headers to your web application or
API project is an easy way of getting lots of security. However, a question that often appear is:

`Which security header should I use for my [web app/API]?`

---

## TL;DR
So as a summary, we may say that you should consider including the following security headers in your system:
### API
* Strict-Transport-Security (HSTS)
* Access-Control-Allow-Methods: Define the options you allow
* X-Content-Type-Options: nosniff (not necessary if you serve responses with what type of content you
are sending, e.g. `Content-Type: application/json`. So make sure to do that!
* X-Frame-Options: Usually thought of for web apps as it stops clickjacking, but is useful for API's
as well. `Yet, as there are advanced attacks involving dragging content out of the frame which could disclose JSON responses, you might still want to leave that header there.`

### Web application
* Strict-Transport-Security (HSTS)
* Access-Control-Allow-Methods: Define the options you allow
* X-XSS-Protection
* X-Content-Type-Options: nosniff
* X-Frame-Options
* Content-Security-Policy (CSP)

### NOTE!
In order to not disclose information about you project, you should avoid adding the `X-Powered-By` 
and `Server` headers.

### Tips
Content Security Policy (CSP) is the security header that might take the longest to implement, 
as all CSP violation should be logged. CSP violations may easily DOS the system receiving them.
Therefore, it is recommended to make a separate service just for handling them. 

---

[This question](https://security.stackexchange.com/questions/147554/security-headers-for-a-web-api) 
on StackExchange hade some good answers:

Checking headers off a list is not the best technique to assert a site's security. Services 
like securityheaders.io can point you in the right direction but all they do is compare against 
a list of proposed settings without any context about your application. Consequently, some of 
the proposals wont't have any impact on the security of an API endpoint that serves nothing but 
JSON responses.

**Strict-Transport-Security (HSTS)** makes sense because it guarantees that users will directly connect 
to your site via HTTPS after their first visit and until the max-age timeout is reached - 
thereby preventing downgrade attacks. Even an API endpoint should be secured with SSL, so keep 
that header. Read more about HSTS [here](https://news.netcraft.com/archives/2016/03/17/95-of-https-servers-vulnerable-to-trivial-mitm-attacks.html).

**Access-Control-Allow-Methods**: GET, POST, OPTIONS is not a security option per se. If your API 
works via CORS preflight requests you need to decide which methods you allow for cross-origin 
sites to use. Disabling CORS could make your API unavailable. If that particular setting is 
sensible depends on your implementation.

**X-XSS-Protection**: 1; mode=block can be good adivice for regular sites because it instructs the 
XSS Auditor (which is implemented in WebKit browsers but not Firefox) to not render the site when 
it detects a reflected XSS attempt. But for an API that just provides JSON responses and doesn't 
serve active content, this header doesn't bring any benefit.

**X-Content-Type-Options**: nosniff prevents browsers from making assumptions about the content type 
if the site didn't declare the type correctly. If you're running a JSON API you should serve the 
responses with Content-Type: application/json. If you do that correctly there will be no need to 
add the nosniff directive.

**X-Frame-Options**: Deny prevents any website from embedding your site in an HTML frame. That option 
stops clickjacking attacks where an attacker tricks users into interacting with your website 
through a disguised frame. But without interactive elements there is limited risk through cross-origin 
framing. Yet, as there are advanced attacks involving dragging content out of the frame which could 
disclose JSON responses, you might still want to leave that header there.

**Content-Security-Policy** headers control what kind of content from what origin your site is allowed 
to interact with (scripts, stylesheets, images, etc.). Your setting "script-src 'self' means that 
only scripts from the same origin may be loaded. A CSP is useful for regular sites but doesn't make 
sense for your API endpoint because you don't serve any active content that could be controlled by 
the CSP.

The Server header specifies information about the server and the software running on it. It's often 
advised to not send that header at all to not disclose anything about backend software and versions.
