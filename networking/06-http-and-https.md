# Networking 06 — HTTP & HTTPS

## Overview

HTTP and HTTPS are fundamental to how websites, APIs, and many web applications communicate.

HTTP stands for:

**Hypertext Transfer Protocol**

HTTPS means:

**HTTP over TLS**

Understanding HTTP is especially important in cybersecurity because web-security testing and analysis often involve examining:

- Requests
- Responses
- Methods
- Headers
- Cookies
- Status codes
- Authentication
- Sessions
- Security headers
- TLS

These concepts also connect directly to my WebSentinel project.

---

# Client and Server

Web communication commonly follows a client-server model.

For example:

```text
Browser
   ↓
HTTP Request
   ↓
Web Server
   ↓
HTTP Response
   ↓
Browser
```

The browser acts as the client.

The web server receives the request, processes it, and returns a response.

---

# HTTP Request

An HTTP request is sent by a client to a server.

A simplified request could look like:

```http
GET /index.html HTTP/1.1
Host: example.com
User-Agent: Browser
Accept: text/html
```

Important parts include:

```text
Method
Path
HTTP version
Headers
Optional body
```

---

# HTTP Methods

HTTP methods describe the action a client wants to perform.

Common methods include:

## GET

Used to request a resource.

```http
GET /products HTTP/1.1
```

---

## POST

Commonly used to submit data.

```http
POST /login HTTP/1.1
```

The request may contain a body such as:

```text
username=sara
password=example
```

Sensitive credentials should be protected using HTTPS.

---

## PUT

Commonly used to create or replace a resource.

---

## PATCH

Commonly used to partially modify a resource.

---

## DELETE

Used to request deletion of a resource.

---

## HEAD

Similar to GET, but the response normally contains headers without the response body.

This can be useful when inspecting server metadata.

For example:

```bash
curl -I https://example.com
```

---

## OPTIONS

Can be used to discover communication options supported by a resource/server and is also involved in mechanisms such as CORS.

---

# HTTP Response

After receiving a request, the server returns a response.

Example:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1234

<html>
...
</html>
```

A response can contain:

```text
Status code
Headers
Body
```

---

# HTTP Status Codes

Status codes describe the result of an HTTP request.

They are grouped into categories.

## 1xx — Informational

The request is continuing or additional information is being provided.

---

## 2xx — Success

Example:

```text
200 OK
```

The request succeeded.

Another example:

```text
201 Created
```

A resource was successfully created.

---

## 3xx — Redirection

Example:

```text
301 Moved Permanently
302 Found
```

The client may need to access another location.

---

## 4xx — Client Errors

Examples:

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
```

### 401 vs 403

A simplified distinction:

```text
401
→ authentication is required or has failed

403
→ the server understood the request but refuses access
```

Exact behavior depends on the application.

---

## 5xx — Server Errors

Examples:

```text
500 Internal Server Error
502 Bad Gateway
503 Service Unavailable
```

These indicate problems while the server or related infrastructure is processing the request.

---

# HTTP Headers

Headers contain additional information about requests and responses.

Examples include:

```text
Host
User-Agent
Content-Type
Authorization
Cookie
Set-Cookie
Location
Cache-Control
Content-Length
```

Headers are extremely important in web security because they can affect browser behavior, authentication, sessions, caching, and security controls.

---

# Host Header

Example:

```http
Host: example.com
```

The Host header identifies the target hostname.

This is especially useful when multiple websites are hosted on the same server/IP infrastructure.

---

# User-Agent

Example:

```http
User-Agent: Mozilla/5.0 ...
```

The User-Agent can provide information about the client software making the request.

However, it should not be treated as trustworthy identity information because clients can modify it.

---

# Content-Type

Example:

```http
Content-Type: application/json
```

This tells the recipient what type of content is being transmitted.

Examples include:

```text
text/html
application/json
text/plain
image/png
```

---

# Cookies

Cookies allow websites to store small pieces of information in a user's browser.

Example:

```http
Set-Cookie: session_id=abc123
```

The browser may later send:

```http
Cookie: session_id=abc123
```

Cookies can be used for:

- Sessions
- Preferences
- Authentication-related state
- Tracking

Because cookies may contain sensitive session identifiers, their security configuration matters.

---

# Session Cookies

After successful authentication, a website may issue a session identifier.

Conceptually:

```text
User logs in
    ↓
Server authenticates user
    ↓
Server creates session
    ↓
Browser receives session cookie
    ↓
Cookie is sent with later requests
```

This allows the server to recognize the authenticated session without requiring the password to be submitted with every request.

---

# Cookie Security Attributes

Important cookie attributes include:

## Secure

```text
Secure
```

Tells the browser to send the cookie only over secure HTTPS connections.

---

## HttpOnly

```text
HttpOnly
```

Restricts access to the cookie from client-side JavaScript.

This can reduce some risks associated with session-cookie theft through certain XSS scenarios.

---

## SameSite

```text
SameSite
```

Controls how cookies are sent in cross-site requests.

Possible values include:

```text
Strict
Lax
None
```

This can help reduce some cross-site request forgery risks when configured appropriately.

---

# HTTP Is Not Encrypted

Traditional HTTP traffic does not provide TLS encryption.

Conceptually:

```text
Client
   ↓
HTTP
   ↓
Network
   ↓
Server
```

Someone capable of observing the traffic may potentially see or manipulate unencrypted HTTP data.

This is why sensitive web communication should use HTTPS.

---

# HTTPS

HTTPS is HTTP communication protected using TLS.

TLS stands for:

**Transport Layer Security**

Conceptually:

```text
HTTP
   ↓
TLS protection
   ↓
TCP
   ↓
IP
```

For HTTP/1.1 and HTTP/2, HTTPS commonly uses TCP.

HTTP/3 uses QUIC over UDP.

---

# What HTTPS Protects

TLS primarily provides:

## Confidentiality

Traffic contents are encrypted so network observers cannot normally read the application data.

## Integrity

TLS helps detect unauthorized modification of protected data while it is in transit.

## Authentication

Certificates help clients authenticate the server they are communicating with.

---

# HTTPS Does NOT Mean a Website Is Safe

This is extremely important.

A website using HTTPS can still be:

- A phishing website
- Malicious
- Vulnerable
- Poorly configured
- Controlled by an attacker

HTTPS tells me that the connection is protected with TLS and that certificate validation provides information about the server identity/domain.

It does NOT automatically mean:

```text
This website is trustworthy.
```

---

# TLS Certificates

Web servers using HTTPS normally present a digital certificate.

A certificate can contain information such as:

```text
Domain names
Issuer
Validity period
Public key information
Digital signatures
```

The browser validates the certificate and its trust chain.

---

# Certificate Authorities

A Certificate Authority — CA — can issue/sign certificates.

Browsers and operating systems maintain trusted root certificate stores.

A simplified trust model is:

```text
Trusted Root CA
      ↓
Intermediate CA
      ↓
Website Certificate
      ↓
example.com
```

If validation succeeds, the browser can establish greater confidence that the certificate presented for the domain is legitimate.

---

# TLS Certificate Errors

A browser may display warnings when problems occur.

Examples can include:

```text
Expired certificate
Hostname mismatch
Untrusted issuer
Invalid certificate chain
```

Users should not automatically ignore certificate warnings.

---

# Security Headers

HTTP response headers can instruct browsers to apply additional security controls.

These are especially relevant to my WebSentinel project.

---

## Content-Security-Policy

```text
Content-Security-Policy
```

CSP allows a website to define which sources of content are permitted.

It can help reduce the impact of certain content-injection attacks such as XSS when designed and configured properly.

Example concept:

```text
Only load scripts from approved sources.
```

---

## Strict-Transport-Security

```text
Strict-Transport-Security
```

Also called:

```text
HSTS
```

HSTS tells compatible browsers to access the site using HTTPS for a specified period.

This helps protect against certain HTTPS downgrade scenarios.

---

## X-Content-Type-Options

A common value is:

```text
X-Content-Type-Options: nosniff
```

This tells browsers not to perform certain MIME-type sniffing behavior.

---

## X-Frame-Options

Examples:

```text
DENY
SAMEORIGIN
```

This controls whether a page may be embedded inside a frame.

It can help reduce clickjacking risks.

Modern applications may also use CSP's:

```text
frame-ancestors
```

directive for frame control.

---

## Referrer-Policy

```text
Referrer-Policy
```

Controls how much referrer information the browser includes when navigating between resources.

This can help reduce unnecessary information leakage.

---

## Permissions-Policy

```text
Permissions-Policy
```

Allows a website to control access to certain browser features.

Examples can include:

```text
Camera
Microphone
Geolocation
```

---

# Why Missing Headers Matter

A missing security header does NOT automatically mean:

```text
The website is vulnerable.
```

Instead, it can indicate that a browser-side defense-in-depth control is not present.

Security analysis should consider:

- Application behavior
- Existing controls
- Context
- Header configuration
- Other security mechanisms

This is an important distinction.

---

# Connection to WebSentinel

My WebSentinel project analyzes passive website security information.

Understanding HTTP and HTTPS helps me understand what the tool is actually examining.

Instead of seeing:

```text
Content-Security-Policy: Missing
```

as simply a red result, I can understand:

```text
What CSP does

Why it exists

What risks it can help reduce

Why missing CSP is not automatically proof of a vulnerability
```

This makes the analysis more meaningful.

---

# `curl`

`curl` can be used to interact with HTTP servers from the command line.

Example:

```bash
curl https://example.com
```

This retrieves the response body.

To request only response headers:

```bash
curl -I https://example.com
```

This can reveal information such as:

```text
HTTP status
Content-Type
Cache-Control
Security headers
Redirects
```

---

# Following Redirects

`curl` can follow HTTP redirects using:

```bash
curl -L https://example.com
```

This is useful when a website redirects:

```text
HTTP → HTTPS
```

or between different URLs.

---

# Viewing More Request/Response Information

Verbose mode can be used with:

```bash
curl -v https://example.com
```

This can display more information about the connection and HTTP exchange.

Care should be taken not to publish sensitive headers, cookies, tokens, or credentials from real authenticated sessions.

---

# Authentication

Web applications need ways to verify user identity.

Examples can include:

```text
Username + password
Multi-factor authentication
Security keys
OAuth/OIDC-based login
```

After authentication, applications often use sessions or tokens to maintain authenticated state.

---

# Authentication vs Authorization

As learned in Linux:

### Authentication

```text
Who are you?
```

### Authorization

```text
What are you allowed to access?
```

This distinction is extremely important in web security.

A user may be properly authenticated but still gain unauthorized access to another user's data if authorization controls are broken.

---

# CORS

CORS stands for:

**Cross-Origin Resource Sharing**

Browsers enforce the Same-Origin Policy, which restricts how web pages interact with resources from different origins.

CORS allows servers to specify which cross-origin requests browsers should permit.

CORS is controlled through HTTP response headers.

Misconfigured CORS policies can sometimes create security risks.

---

# Origin

A web origin is generally defined by:

```text
Scheme + Host + Port
```

For example:

```text
https://example.com:443
```

Changing one of these can result in a different origin.

Examples:

```text
http://example.com
https://example.com
https://api.example.com
https://example.com:8443
```

may represent different origins.

---

# HTTP and Web Security

Many web vulnerabilities can be understood by inspecting HTTP requests and responses.

Examples include:

```text
SQL Injection
Cross-Site Scripting
CSRF
Broken Access Control
Authentication weaknesses
Session vulnerabilities
File upload vulnerabilities
Path traversal
Command injection
SSRF
```

The browser interface may hide much of what is happening.

Security tools such as Burp Suite allow analysts and authorized testers to inspect and modify HTTP traffic in controlled environments.

---

# Example Request Analysis

Imagine:

```http
GET /account?id=123 HTTP/1.1
Host: example.com
Cookie: session=abc123
```

A security analyst might ask:

```text
What does id=123 represent?

Can the user change it?

Does the server verify authorization?

Is the session cookie protected?

Is HTTPS being used?

What response is returned?
```

This demonstrates why understanding raw HTTP is important.

---

# Sensitive Information in HTTP

Requests and responses may contain sensitive information such as:

```text
Session cookies
Authorization tokens
API keys
Personal information
Credentials
```

These should not be copied into public GitHub repositories.

When documenting labs, sensitive values should be removed or replaced with safe examples.

---

# HTTP Investigation Flow

A simple web-security investigation can look like:

```text
Request
   ↓
Method
   ↓
URL / Parameters
   ↓
Headers
   ↓
Cookies / Authentication
   ↓
Body
   ↓
Server Response
   ↓
Status Code
   ↓
Response Headers
   ↓
Response Body
```

Understanding each part helps identify unusual behavior.

---

# Key Terms

```text
HTTP
HTTPS
Request
Response
Method
GET
POST
PUT
PATCH
DELETE
HEAD
OPTIONS
Status Code
Header
Cookie
Session
TLS
Certificate
Certificate Authority
CSP
HSTS
X-Frame-Options
X-Content-Type-Options
Referrer-Policy
Permissions-Policy
CORS
Origin
Authentication
Authorization
```

---

# Security Takeaways

The main lessons from HTTP and HTTPS are:

- HTTP uses requests and responses.
- HTTP methods describe actions requested by clients.
- Status codes communicate request results.
- Headers provide important metadata and controls.
- Cookies are commonly used for sessions and need secure configuration.
- HTTPS protects HTTP traffic using TLS.
- HTTPS does not automatically mean a website is trustworthy or vulnerability-free.
- TLS certificates help authenticate servers.
- Security headers provide browser-side defense-in-depth controls.
- Missing security headers require context and are not automatically vulnerabilities.
- Authentication and authorization are separate security concepts.
- HTTP traffic is fundamental to understanding web vulnerabilities.

---

# What I Learned

This topic helped me understand what is actually happening underneath a web browser.

When I visit a website, the browser is sending HTTP requests containing methods, paths, headers, cookies, and sometimes request bodies. The server processes those requests and returns status codes, headers, and response data.

I also learned what HTTPS actually means. It protects HTTP communication using TLS, providing confidentiality, integrity, and server authentication, but it does not guarantee that the website itself is safe.

The security-header section was especially useful because it connects directly to WebSentinel. I now understand that headers such as CSP and HSTS are not simply items to mark as present or missing. They represent specific browser security controls whose meaning and configuration need to be understood.

This foundation will help me when I begin learning web-security testing and analyzing HTTP traffic using tools such as Burp Suite and Wireshark in authorized labs.
