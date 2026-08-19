# Networking 05 — DNS

## Overview

DNS stands for:

**Domain Name System**

DNS is one of the most important services used on networks and the Internet.

Humans prefer names such as:

```text
google.com
github.com
uaeu.ac.ae
```

but computers communicate using IP addresses.

DNS helps translate between human-readable domain names and information such as IP addresses.

A simplified idea is:

```text
github.com
     ↓
    DNS
     ↓
IP Address
```

Without DNS, users would often need to remember IP addresses instead of domain names.

---

# Domain Names

A domain name is a human-readable name used to identify Internet resources.

Example:

```text
www.example.com
```

This can be broken into parts:

```text
www . example . com
 │       │       │
 │       │       └── Top-Level Domain
 │       └────────── Domain
 └────────────────── Subdomain / Hostname
```

---

# Top-Level Domains — TLDs

The final part of a domain name is called the:

**Top-Level Domain**

Examples include:

```text
.com
.org
.net
.ae
.edu
.gov
```

Country-code TLDs also exist.

For example:

```text
.ae → United Arab Emirates
.uk → United Kingdom
.jp → Japan
```

---

# Subdomains

Organizations can create subdomains beneath their domain.

For example:

```text
mail.example.com
shop.example.com
portal.example.com
api.example.com
```

These may point to different systems or services.

From a cybersecurity perspective, subdomains can be important because an organization's Internet-facing attack surface may include more than its main website.

---

# DNS Resolution

Suppose I enter:

```text
example.com
```

into my browser.

Before the browser can communicate with the web server, the system generally needs to determine an IP address associated with that domain.

A simplified DNS resolution process is:

```text
Browser / Application
        ↓
Operating System
        ↓
DNS Resolver
        ↓
DNS Infrastructure
        ↓
Answer
        ↓
IP Address
```

The real DNS process can involve multiple servers and caches.

---

# DNS Resolver

A DNS resolver helps clients perform DNS lookups.

Examples can include resolvers operated by:

- Internet service providers
- Organizations
- Cloud providers
- Public DNS providers

The client sends a DNS query and receives a response.

---

# Recursive Resolver

A recursive resolver performs DNS resolution on behalf of the client.

If it does not already have the answer cached, it may contact other DNS infrastructure to find the requested information.

Conceptually:

```text
My Computer
     ↓
Recursive Resolver
     ↓
Finds the answer
     ↓
Returns result
```

---

# DNS Hierarchy

DNS is hierarchical.

A simplified lookup can involve:

```text
Root DNS Servers
       ↓
TLD Servers
       ↓
Authoritative DNS Server
       ↓
Requested DNS Record
```

---

# Root DNS Servers

Root servers sit near the top of the DNS hierarchy.

They do not normally contain the IP address of every website.

Instead, they can direct resolvers toward the appropriate TLD infrastructure.

For example:

```text
example.com
    ↓
Root
    ↓
.com TLD servers
```

---

# TLD Name Servers

TLD servers contain information about domains within a top-level domain.

For:

```text
example.com
```

the `.com` TLD infrastructure can direct the resolver toward the authoritative name servers for `example.com`.

---

# Authoritative DNS Server

An authoritative DNS server contains DNS information for a domain.

It can provide the actual DNS records requested by a resolver.

Simplified:

```text
Resolver:
What is the A record for example.com?

        ↓

Authoritative DNS Server:
Here is the record.
```

---

# DNS Records

DNS stores different types of information using records.

Understanding common record types is useful in both networking and cybersecurity.

---

## A Record

An:

```text
A
```

record maps a name to an IPv4 address.

Example:

```text
example.com
      ↓
192.0.2.10
```

---

## AAAA Record

An:

```text
AAAA
```

record maps a name to an IPv6 address.

Example:

```text
example.com
      ↓
2001:db8::10
```

---

## CNAME Record

CNAME stands for:

**Canonical Name**

A CNAME record makes one hostname an alias of another hostname.

Conceptually:

```text
www.example.com
       ↓
example.hosting-provider.com
```

The final target can then be resolved normally.

---

## MX Record

MX stands for:

**Mail Exchange**

MX records identify mail servers responsible for receiving email for a domain.

Example:

```text
example.com
    ↓
MX
    ↓
mail.example.com
```

Email security investigations may involve examining MX records.

---

## TXT Record

TXT records contain text information.

They are used for many purposes.

In security and email, TXT records are often involved with technologies such as:

```text
SPF
DKIM-related information
DMARC
Domain verification
```

---

## NS Record

NS stands for:

**Name Server**

NS records identify authoritative name servers for a domain.

---

## PTR Record

PTR records are commonly used for reverse DNS lookups.

Instead of:

```text
Domain → IP
```

reverse DNS can conceptually provide:

```text
IP → Hostname
```

when an appropriate PTR record exists.

---

# DNS Caching

DNS results can be cached.

Caching improves performance because the same DNS lookup does not always need to repeat the full resolution process.

Conceptually:

```text
First request
     ↓
DNS lookup
     ↓
Answer cached

Later request
     ↓
Cached answer
```

---

# TTL

DNS records can contain a:

**Time To Live — TTL**

TTL helps determine how long a DNS response may be cached before it should be refreshed.

A longer TTL can reduce DNS query traffic but can also make DNS changes take longer to propagate through caches.

---

# DNS Commands

Several command-line tools can be used to inspect DNS.

---

## `nslookup`

Example:

```bash
nslookup example.com
```

This can return DNS resolution information.

---

## `dig`

`dig` provides detailed DNS query information.

Example:

```bash
dig example.com
```

Specific record types can be requested.

### IPv4

```bash
dig example.com A
```

### IPv6

```bash
dig example.com AAAA
```

### Mail Servers

```bash
dig example.com MX
```

### Name Servers

```bash
dig example.com NS
```

### TXT Records

```bash
dig example.com TXT
```

---

# DNS and UDP/TCP

Traditional DNS commonly uses:

```text
Port 53
```

Many traditional DNS queries use UDP.

However, DNS can also use TCP.

Therefore:

```text
DNS = always UDP
```

would be incorrect.

Modern DNS can also use encrypted technologies such as:

```text
DNS over HTTPS — DoH
DNS over TLS — DoT
```

---

# DNS in Wireshark

Later, Wireshark will allow me to inspect DNS traffic.

I may see something conceptually similar to:

```text
Client
  ↓
DNS Query:
What is the A record for example.com?

  ↓

DNS Server
  ↓
DNS Response:
example.com → IP address
```

This will let me see DNS resolution happening in actual network traffic.

---

# DNS and Cybersecurity

DNS is extremely important in cybersecurity because almost every Internet-connected application depends on name resolution.

Security teams can analyze DNS activity to help identify unusual behavior.

---

# Suspicious DNS Activity

Potentially interesting observations can include:

- Requests to unusual domains
- Very high numbers of DNS queries
- Newly observed domains
- Algorithmically generated-looking domain names
- Unexpected DNS servers
- Unusual record types
- Repeated failed DNS lookups
- DNS requests from unexpected systems

None of these automatically proves malicious activity.

They are indicators that may require investigation.

---

# Malware and DNS

Malware may use DNS to locate infrastructure controlled by an attacker.

A simplified scenario could be:

```text
Compromised Device
       ↓
DNS Query
       ↓
Malicious Domain
       ↓
Attacker Infrastructure
```

Monitoring DNS can therefore provide useful information during security investigations.

---

# Domain Generation Algorithms

Some malware families use:

**Domain Generation Algorithms — DGAs**

A DGA can generate many possible domain names.

Conceptually:

```text
xk29ajd.com
qpm82ks.net
ab39dkq.org
```

The malware may attempt to contact generated domains until it finds one registered by the attacker.

Security tools may attempt to identify unusual patterns associated with this behavior.

---

# DNS Tunneling

DNS can potentially be abused as a communication channel.

This is sometimes referred to as:

**DNS tunneling**

Data can be encoded into DNS queries or responses to bypass some network controls.

For example, unusually long or frequent DNS queries may sometimes be worth investigating.

However, unusual DNS traffic alone is not proof of tunneling.

---

# DNS Spoofing / Cache Poisoning

An attacker may attempt to cause a victim or DNS resolver to accept incorrect DNS information.

Conceptually:

```text
Victim asks:

Where is example.com?

        ↓

Malicious / incorrect response

        ↓

Victim receives wrong IP
```

This could potentially redirect traffic toward an unintended system.

Modern DNS implementations include protections against various forms of these attacks.

---

# DNSSEC

DNSSEC stands for:

**Domain Name System Security Extensions**

DNSSEC provides mechanisms that allow DNS responses to be cryptographically validated.

Its goal is to help protect against certain forms of DNS data manipulation.

DNSSEC primarily provides authenticity and integrity for DNS data.

It does not encrypt normal DNS queries.

---

# Encrypted DNS

Traditional DNS queries can often be visible to network observers.

Encrypted DNS technologies include:

### DNS over HTTPS

```text
DoH
```

DNS queries are transported using HTTPS.

### DNS over TLS

```text
DoT
```

DNS queries are transported through TLS.

These technologies improve DNS privacy but can also affect how organizations monitor DNS traffic.

---

# Phishing and DNS

Domain names are important in phishing investigations.

Attackers may register domains that look similar to legitimate organizations.

Examples could include techniques such as:

```text
Character substitution
Typosquatting
Subdomain deception
Homoglyphs
```

For example, a suspicious domain might visually resemble a legitimate brand.

DNS information can become one part of investigating such domains.

---

# DNS Investigation Questions

If I encounter a suspicious domain, useful questions include:

```text
What IP addresses does it resolve to?

Which name servers does it use?

Does it have MX records?

What TXT records exist?

Is the domain newly observed?

Is the domain name attempting to imitate another organization?

Which systems queried this domain?

How frequently was it queried?

Did connections occur after DNS resolution?
```

DNS alone may not answer all of these questions, but it can provide useful evidence.

---

# Example Security Investigation

Imagine a security system reports:

```text
Host:
192.168.1.45

DNS Query:
login-company-secure-example.com
```

Instead of immediately calling it malicious, an analyst might investigate:

```text
1. Is the domain known?

2. What IP does it resolve to?

3. When was it first observed?

4. Does the name imitate another service?

5. Which internal devices queried it?

6. Did those devices connect to the resolved IP?

7. Was the user expecting to access this domain?

8. Do threat-intelligence sources report suspicious activity?
```

This demonstrates the importance of context.

---

# DNS Investigation Flow

A simplified investigation could look like:

```text
Suspicious Domain
       ↓
DNS Records
       ↓
IP Addresses
       ↓
Name Servers
       ↓
Related Traffic
       ↓
Systems That Queried It
       ↓
Context
       ↓
Assessment
```

---

# Connection to Web Security

DNS is part of the path between entering a website name and connecting to the web server.

Simplified:

```text
Domain
   ↓
DNS
   ↓
IP Address
   ↓
Network Connection
   ↓
Web Server
   ↓
HTTP / HTTPS
```

Understanding DNS therefore helps me understand the infrastructure behind web applications.

---

# Key DNS Records

| Record | Purpose |
|---|---|
| A | IPv4 address |
| AAAA | IPv6 address |
| CNAME | Alias to another hostname |
| MX | Mail server |
| NS | Authoritative name server |
| TXT | Text information |
| PTR | Reverse DNS |

---

# Key Terms

```text
DNS
Domain
Subdomain
TLD
Resolver
Recursive Resolver
Root Server
TLD Server
Authoritative DNS Server
A Record
AAAA Record
CNAME
MX
TXT
NS
PTR
TTL
DNS Cache
DNSSEC
DoH
DoT
DNS Tunneling
DNS Spoofing
DGA
```

---

# Security Takeaways

The main cybersecurity lessons from DNS are:

- DNS translates human-readable names into network information.
- DNS is hierarchical and distributed.
- Different DNS record types serve different purposes.
- DNS traffic can provide valuable security information.
- Malware can use DNS for infrastructure discovery or communication.
- Attackers may abuse DNS through techniques such as spoofing or tunneling.
- Suspicious domains require investigation rather than immediate assumptions.
- DNSSEC helps validate DNS data but does not encrypt normal DNS queries.
- DoH and DoT can provide privacy by encrypting DNS communication.
- DNS is an important source of evidence during incident investigations.

---

# What I Learned

This topic helped me understand that DNS does much more than simply turn a website name into an IP address.

DNS is a hierarchical system involving resolvers, root servers, TLD infrastructure, authoritative servers, caching, and different record types.

I also learned why DNS is important in cybersecurity. DNS activity can reveal which domains systems are attempting to contact and may help identify suspicious infrastructure, phishing domains, malware communication, or unusual network behavior.

At the same time, DNS observations need context. An unusual domain or DNS request is something to investigate, not automatic proof that a system is compromised.

Understanding DNS will also make HTTP/HTTPS and Wireshark analysis easier because I can follow the process from a domain name to an IP address and then to the actual network connection.
