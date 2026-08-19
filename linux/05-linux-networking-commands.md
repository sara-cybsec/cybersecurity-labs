# Linux 05 — Networking Commands & Basic Network Investigation

## Overview

Networking is a major part of cybersecurity.

Before analyzing attacks, packets, suspicious connections, or network services, I need to understand how a system communicates with other devices.

This note covers useful Linux/macOS networking commands and explains how they can support basic network investigation.

---

## IP Addresses

An IP address identifies a device or network interface on an IP network.

Two major versions are:

### IPv4

Example:

```text
192.168.1.25
```

IPv4 addresses are 32 bits long.

### IPv6

Example:

```text
2001:db8::1
```

IPv6 addresses are 128 bits long and provide a much larger address space.

---

## Private vs Public IP Addresses

### Private IP

A private IP address is commonly used inside a local network.

Common IPv4 private ranges include:

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

### Public IP

A public IP address is used for communication across the Internet and is globally routable, subject to network design and provider configuration.

A device inside a home network commonly uses a private address while the router connects that network to the Internet.

---

## `ifconfig` / `ip`

Network-interface information can be inspected using commands such as:

```bash
ifconfig
```

On many Linux systems, the modern command is:

```bash
ip addr
```

These commands can provide information about network interfaces and their configured addresses.

Examples of interfaces may include:

```text
Ethernet
Wi-Fi
Loopback
Virtual interfaces
```

---

## Loopback

A commonly used loopback IPv4 address is:

```text
127.0.0.1
```

It refers back to the local machine.

The hostname:

```text
localhost
```

commonly resolves to a loopback address.

This is useful when testing applications or services locally without communicating with another device.

---

## `ping`

`ping` can be used to test basic IP reachability and measure round-trip response times when ICMP echo traffic is allowed.

Example:

```bash
ping example.com
```

A successful response can show that communication is possible between the systems.

However, failure to receive a ping response does NOT automatically mean the destination is offline.

Firewalls or system configurations may block ICMP traffic.

This is important when interpreting network results.

---

## DNS

DNS stands for:

**Domain Name System**

Humans prefer names such as:

```text
example.com
```

Networks ultimately communicate using IP addresses.

DNS helps map domain names to IP addresses and provides other types of records.

Conceptually:

```text
example.com
     ↓
DNS lookup
     ↓
IP address
```

---

## `nslookup`

One tool for querying DNS information is:

```bash
nslookup example.com
```

It can return information about DNS resolution for the requested domain.

---

## `dig`

Another DNS utility is:

```bash
dig example.com
```

`dig` can provide more detailed DNS information.

Different DNS record types can also be queried.

For example:

```bash
dig example.com A
```

requests an IPv4 address record.

```bash
dig example.com AAAA
```

requests an IPv6 address record.

```bash
dig example.com MX
```

requests mail-exchange records.

---

## `curl`

`curl` is an extremely useful command-line networking tool.

A basic request can be made with:

```bash
curl https://example.com
```

This retrieves the response body returned by the web server.

To inspect response headers:

```bash
curl -I https://example.com
```

Headers can reveal information such as:

```text
HTTP status
Content type
Caching behavior
Security headers
Server-related information
```

This connects directly to web-security analysis.

---

## HTTP Status Codes

When working with HTTP, responses contain status codes.

Common examples include:

```text
200 → OK
301 → Moved Permanently
302 → Found / Redirect
400 → Bad Request
401 → Unauthorized
403 → Forbidden
404 → Not Found
500 → Internal Server Error
```

Understanding status codes helps when analyzing web applications and HTTP traffic.

---

## Ports

Network services commonly listen on numbered ports.

Examples include:

```text
22   → SSH
53   → DNS
80   → HTTP
443  → HTTPS
```

Ports help the operating system determine which service or application should receive network traffic.

An IP address identifies a host/interface, while a port helps identify a network service on that host.

Conceptually:

```text
IP Address + Port
```

Example:

```text
192.168.1.10:443
```

---

## TCP and UDP

Two important transport protocols are:

### TCP

Transmission Control Protocol

TCP is connection-oriented and provides mechanisms for reliable, ordered delivery.

Common examples include many uses of:

```text
HTTP/HTTPS
SSH
```

### UDP

User Datagram Protocol

UDP is connectionless and has lower protocol overhead, but it does not provide the same delivery guarantees as TCP.

Common uses can include:

```text
DNS
Streaming
Real-time communication
```

The exact protocol used depends on the application.

---

## Viewing Network Connections

Depending on the operating system, tools can include:

```bash
netstat
```

and on many modern Linux systems:

```bash
ss
```

These can help inspect network connections and listening sockets.

A security analyst may want to know:

```text
What connections currently exist?
What ports are listening?
Which services are communicating?
Are there unexpected connections?
```

---

## Listening Ports

A listening port means a process is waiting for incoming network connections on a socket.

Conceptually:

```text
Application
    ↓
Listening socket
    ↓
Port
    ↓
Incoming connection
```

Unexpected listening services can be worth investigating during a security review.

However, an open/listening port is not automatically malicious.

Context matters.

---

## `lsof`

On systems such as macOS, `lsof` can also help investigate network activity.

For example:

```bash
lsof -i
```

can display processes using network connections.

This is useful because it helps connect:

```text
PROCESS
   ↕
NETWORK CONNECTION
```

This connects the process-monitoring concepts from Linux Lab 03 with networking.

---

## Hostnames

A hostname is a human-readable name assigned to a device.

The command:

```bash
hostname
```

can display the current system hostname.

Hostnames can make systems easier to identify than relying only on IP addresses.

---

## Basic Network Investigation Workflow

A simple investigation might look like:

```text
1. Identify the system
        ↓
2. Inspect network interfaces
        ↓
3. Identify IP configuration
        ↓
4. Inspect DNS resolution
        ↓
5. Check connectivity
        ↓
6. Inspect active connections
        ↓
7. Identify listening services
        ↓
8. Associate connections with processes
```

Different investigations require different tools, but this provides a useful starting structure.

---

## Cybersecurity Connection

Networking knowledge is necessary for understanding areas such as:

- Network monitoring
- Firewalls
- Web security
- Packet analysis
- Intrusion detection
- Incident response
- Port scanning
- Network segmentation
- DNS security
- Traffic analysis

A security tool may produce an IP address, port, protocol, or DNS record as output.

Without understanding networking fundamentals, those results are difficult to interpret correctly.

---

## Connection to WebSentinel

Networking and HTTP concepts also connect to my WebSentinel project.

Web security analysis can involve understanding:

- URLs
- DNS
- HTTP requests
- HTTP responses
- HTTP headers
- HTTPS
- Web servers
- Ports
- Security configuration

Learning the networking fundamentals behind these concepts helps me understand what security tools are actually analyzing rather than treating their output as unexplained data.

---

## Useful Commands

```bash
ifconfig
ip addr
ping
hostname
nslookup
dig
curl
curl -I
netstat
ss
lsof -i
```

Not every command is available by default on every operating system.

For example, macOS and Linux may use different preferred networking utilities.

---

## Important Security Lesson

Network-tool output needs interpretation.

For example:

```text
Ping failed
```

does not necessarily mean:

```text
Host is offline
```

And:

```text
Port is listening
```

does not necessarily mean:

```text
System is compromised
```

Security analysis requires context rather than jumping immediately from an observation to a conclusion.

---

## What I Learned

This topic helped me understand how networking information can be investigated from the command line.

I learned the relationship between IP addresses, DNS, ports, protocols, services, and processes.

I also learned that tools such as `curl` can reveal useful HTTP information, while utilities such as `lsof`, `netstat`, or `ss` can help investigate network activity.

Most importantly, I learned that cybersecurity tools provide observations that still need to be interpreted. A failed connection, open port, or unusual process is something to investigate rather than automatically proof of an attack.
