# Networking 04 — TCP, UDP & Ports

## Overview

Once devices know how to reach each other using IP addresses, applications still need a way to communicate with the correct service.

Two important transport-layer protocols are:

- TCP — Transmission Control Protocol
- UDP — User Datagram Protocol

Ports are then used to help identify which application or service should receive network traffic.

Understanding TCP, UDP, and ports is important in cybersecurity because they appear constantly in:

- Firewall logs
- Wireshark captures
- Network monitoring
- Nmap results in authorized environments
- IDS/IPS alerts
- Server configuration
- CTF challenges
- Incident investigations

---

# IP Address vs Port

An IP address identifies a host or network interface in IP communication.

Example:

```text
192.168.1.20
```

But one device can run many network services at the same time.

For example, a server could provide:

```text
Website
SSH
DNS
Database
Email
```

So knowing only the IP address is not enough.

Ports help identify the service/application endpoint.

Conceptually:

```text
IP Address
    +
Port
    ↓
Network Service
```

Example:

```text
192.168.1.20:443
```

This means communication involving:

```text
IP   → 192.168.1.20
Port → 443
```

Port 443 is commonly associated with HTTPS.

---

# Ports

TCP and UDP use 16-bit port numbers.

The possible range is:

```text
0 – 65535
```

Ports help operating systems deliver incoming network traffic to the correct application or service.

For example:

```text
Browser
   ↓
HTTPS
   ↓
TCP Port 443
```

---

# Common Ports

Some common port numbers include:

| Port | Common Service |
|---|---|
| 20/21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 67/68 | DHCP |
| 80 | HTTP |
| 110 | POP3 |
| 143 | IMAP |
| 443 | HTTPS |
| 445 | SMB |
| 3389 | RDP |

These are common/default associations.

A service is not technically required to use its standard port.

For example, a web server could be configured to listen on:

```text
8080
```

instead of:

```text
80
```

Therefore:

> A port number can suggest what service may be running, but it does not prove which application is actually using that port.

---

# Port Categories

Port numbers are commonly divided into ranges.

## Well-Known Ports

```text
0 – 1023
```

Commonly associated with standard services and protocols.

Examples:

```text
22  SSH
53  DNS
80  HTTP
443 HTTPS
```

---

## Registered Ports

```text
1024 – 49151
```

These may be registered for particular applications or services.

---

## Dynamic / Private Ports

```text
49152 – 65535
```

These are commonly used as temporary or ephemeral client-side ports.

Exact ephemeral-port behavior can vary by operating system.

---

# Client and Server Ports

Suppose my browser connects to a website.

The server may listen on:

```text
443
```

But my computer also needs a source port for the connection.

For example:

```text
My MacBook

192.168.1.20:53142

        ↓

Web Server

203.0.113.50:443
```

Here:

```text
53142
```

could be a temporary client-side port.

And:

```text
443
```

is the server's HTTPS listening port.

This combination helps distinguish network conversations.

---

# Socket / Connection Information

Network communication can involve information such as:

```text
Source IP
Source Port
Destination IP
Destination Port
Protocol
```

Example:

```text
Source IP:        192.168.1.20
Source Port:      53142

Destination IP:   203.0.113.50
Destination Port: 443

Protocol:         TCP
```

This type of information appears frequently in packet captures and security logs.

---

# TCP

TCP stands for:

**Transmission Control Protocol**

TCP is connection-oriented.

Before normal application data is exchanged, TCP establishes a connection between the communicating endpoints.

TCP provides mechanisms for:

- Reliable delivery
- Ordered delivery
- Detecting missing data
- Retransmission
- Flow control
- Congestion control

---

# Reliability

Imagine an application needs to send:

```text
A
B
C
D
```

If part of the TCP data is lost during transmission, TCP can detect missing information and retransmit it.

The receiving application can receive the byte stream in the correct order.

This makes TCP useful for applications where reliable delivery is important.

---

# TCP Sequence Numbers

TCP uses sequence numbers to track data within a connection.

These help TCP:

- Maintain ordering
- Detect missing segments
- Handle retransmissions

Wireshark will later allow me to inspect TCP sequence and acknowledgment information.

---

# TCP Acknowledgments

TCP uses acknowledgments to confirm receipt of data.

Simplified:

```text
Sender
  ↓
Data
  ↓
Receiver
  ↓
ACK
  ↓
Sender
```

ACK means:

**Acknowledgment**

This contributes to TCP's reliability.

---

# TCP Connection Establishment

TCP normally establishes a connection using the:

**Three-Way Handshake**

Simplified:

```text
Client → SYN → Server

Client ← SYN-ACK ← Server

Client → ACK → Server
```

After this, the TCP connection is established.

I will study this in more detail in Networking Lesson 07.

---

# TCP Connection Termination

TCP connections also have a structured termination process.

Flags such as:

```text
FIN
ACK
```

can be involved in closing a connection.

A:

```text
RST
```

flag can reset a TCP connection.

These flags become useful during packet analysis.

---

# UDP

UDP stands for:

**User Datagram Protocol**

UDP is connectionless.

Unlike TCP, UDP does not establish a connection using a TCP-style handshake before sending application datagrams.

UDP has lower protocol overhead but does not provide TCP's built-in guarantees for:

- Reliable delivery
- Ordering
- Retransmission

If an application needs these behaviors while using UDP, it must provide them itself or tolerate their absence.

---

# Why Use UDP?

TCP's reliability mechanisms are useful, but they introduce additional overhead and behavior.

Some applications prioritize:

- Low latency
- Simplicity
- Real-time delivery
- Small request/response exchanges

and may use UDP.

Examples can include some uses of:

```text
DNS
Voice communication
Video streaming
Online gaming
DHCP
```

The actual protocol choice depends on the application.

---

# TCP vs UDP

| Feature | TCP | UDP |
|---|---|---|
| Connection-oriented | Yes | No |
| Reliable delivery mechanisms | Yes | No built-in guarantee |
| Ordered delivery | Yes | No built-in guarantee |
| Retransmission | Yes | No built-in retransmission |
| Lower protocol overhead | No | Yes |
| Three-way handshake | Yes | No |

Neither protocol is simply "better."

They are designed for different communication requirements.

---

# Example — Web Browsing

Traditional HTTPS communication commonly uses TCP:

```text
Browser
   ↓
TCP
   ↓
Port 443
   ↓
Web Server
```

Modern web technologies can also use protocols such as HTTP/3, which operates over QUIC using UDP.

This is a useful reminder that networking technologies continue to evolve.

---

# Example — DNS

Traditional DNS queries commonly use UDP port:

```text
53
```

However, DNS can also use TCP in certain situations.

Modern encrypted DNS technologies may use other transports and ports.

Therefore, it is better to understand protocols rather than memorize:

```text
DNS always = UDP
```

because that would be incorrect.

---

# Listening Ports

A server application can listen for incoming connections or traffic on a port.

Conceptually:

```text
Web Server
    ↓
Listening
    ↓
TCP 443
```

A listening port indicates that a service is waiting for network communication.

---

# Open Ports

Security tools may describe a port as:

```text
open
```

This generally means a service appears reachable/listening on that port from the perspective of the test being performed.

An open port is not automatically a vulnerability.

For example:

```text
443 open
```

may simply indicate that a legitimate HTTPS server is running.

The important questions are:

```text
What service is listening?

Should it be exposed?

Is the service securely configured?

Is it updated?

Who should be able to access it?
```

---

# Closed Ports

A closed port generally means that the host is reachable but no service is accepting connections on that tested port.

The exact behavior depends on the protocol and network configuration.

---

# Filtered Ports

Security scanning tools may describe ports as:

```text
filtered
```

This can mean the tool cannot determine the port's state because traffic is being blocked or filtered.

Possible causes can include:

```text
Firewall
Packet filtering
Network security controls
```

---

# Firewalls and Ports

Firewalls can make decisions based on information such as:

```text
Source IP
Destination IP
Source Port
Destination Port
Protocol
Connection state
```

For example, a policy could conceptually say:

```text
Allow TCP
Destination Port 443
```

while blocking other traffic.

Real firewall rules can be much more detailed.

---

# Why Ports Matter in Cybersecurity

Ports help security professionals understand which network services may be exposed.

For example:

```text
22   → SSH
80   → HTTP
443  → HTTPS
445  → SMB
3389 → RDP
```

Unexpected exposed services may increase attack surface.

However:

```text
Open port
≠
Vulnerability
```

The service behind the port must be investigated.

---

# Attack Surface

Every unnecessary exposed service can potentially increase the system's attack surface.

For example:

```text
Server needs:
443 HTTPS

But also exposes:
21 FTP
23 Telnet
445 SMB
3389 RDP
```

A security review might ask:

```text
Why are these services exposed?

Are they actually required?

Who can access them?

Are they securely configured?

Can unnecessary services be disabled?
```

This connects back to the Principle of Least Privilege:

> Only expose services that are actually necessary.

---

# Port Scanning

Port scanning is a technique used to determine which ports/services may be reachable on a system.

Security professionals use port scanning for legitimate purposes such as:

- Asset discovery
- Security assessments
- Configuration verification
- Authorized penetration testing
- Network troubleshooting

Attackers can also use scanning for reconnaissance.

Port scanning should only be performed against systems where testing is authorized.

---

# Nmap

Nmap is a widely used network discovery and security-auditing tool.

It can help identify information such as:

- Reachable hosts
- Open ports
- Possible services
- Service versions
- Network characteristics

I will only use tools such as Nmap in authorized environments, CTFs, labs, or systems I have permission to test.

---

# Example Security Investigation

Imagine a security alert shows:

```text
Source:
192.168.1.50:54123

Destination:
203.0.113.10:22

Protocol:
TCP
```

I can interpret:

```text
Source host:
192.168.1.50

Temporary source port:
54123

Destination:
203.0.113.10

Destination port:
22

Common service associated with 22:
SSH

Transport protocol:
TCP
```

But I still need context.

Questions include:

```text
Should this device be making SSH connections?

Who owns the source device?

Who owns the destination?

Was the connection successful?

What process created the connection?

Is this normal administrative activity?

How often is it happening?
```

---

# TCP and UDP in Wireshark

Later, Wireshark will allow me to inspect:

```text
Source IP
Destination IP
Source Port
Destination Port
TCP flags
Sequence numbers
Acknowledgments
UDP datagrams
Payload information when visible
```

Understanding TCP, UDP, and ports before opening Wireshark will make packet captures easier to interpret.

---

# Connection to Web Security

Web security depends heavily on networking concepts.

For example:

```text
HTTPS
   ↓
Application protocol
   ↓
Transport
   ↓
TCP or QUIC/UDP
   ↓
IP
```

Understanding the lower layers helps me understand what tools such as browsers, proxies, packet analyzers, and web-security tools are actually doing.

---

# Key Terms

```text
TCP
UDP
Port
Source Port
Destination Port
Socket
Service
Listening Port
Open Port
Closed Port
Filtered Port
TCP Sequence Number
Acknowledgment
SYN
ACK
FIN
RST
Firewall
Attack Surface
Port Scanning
Nmap
```

---

# Security Takeaways

The main security lessons from this topic are:

- IP addresses identify network endpoints, while ports help identify services.
- TCP and UDP provide different transport behaviors.
- TCP provides built-in reliability and ordering mechanisms.
- UDP has lower overhead but fewer built-in guarantees.
- Port numbers can suggest a service but do not prove which application is running.
- An open port is not automatically a vulnerability.
- Unnecessary exposed services can increase attack surface.
- Firewalls can control traffic based on network information including ports and protocols.
- Network observations require context before deciding whether activity is suspicious.

---

# What I Learned

This topic helped me understand why an IP address alone is not enough to describe network communication.

A single computer can run many services, and ports help the operating system direct network traffic to the correct application.

I learned the main differences between TCP and UDP and why different applications choose different transport protocols.

I also learned an important cybersecurity distinction: finding an open port does not automatically mean finding a vulnerability. The service behind the port, its configuration, exposure, purpose, and security controls all need to be investigated.

Understanding TCP, UDP, and ports will make future Wireshark captures, firewall logs, CTF networking challenges, and authorized network-security labs much easier to interpret.
