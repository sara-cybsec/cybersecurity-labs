# Networking 08 — Wireshark Fundamentals

## Overview

Wireshark is a network protocol analyzer that allows me to capture and inspect network traffic.

Instead of only reading about protocols such as:

- ARP
- DNS
- TCP
- UDP
- HTTP
- TLS

Wireshark allows me to see many of these protocols inside actual network traffic.

This is important in cybersecurity because packet analysis can help investigate network behavior, troubleshoot communication, and understand security events.

---

# What Is a Packet Capture?

A packet capture records network traffic observed on a network interface.

A capture may contain information such as:

```text
Source IP
Destination IP
Source Port
Destination Port
Protocol
TCP Flags
DNS Queries
Packet Length
Timing
```

Depending on the protocol and whether encryption is being used, some payload information may also be visible.

---

# Network Interfaces

Before capturing traffic, Wireshark asks which network interface I want to monitor.

Examples can include:

```text
Wi-Fi
Ethernet
Loopback
Virtual interfaces
```

If my MacBook is connected through Wi-Fi, the Wi-Fi interface is normally the relevant interface for observing that traffic.

---

# Wireshark Packet List

The main packet list contains columns such as:

```text
No.
Time
Source
Destination
Protocol
Length
Info
```

Each row represents a captured packet/frame.

---

# Source

The Source column identifies where the packet came from.

Example:

```text
192.168.1.20
```

---

# Destination

The Destination column identifies where the packet is going.

Example:

```text
8.8.8.8
```

---

# Protocol

The Protocol column can identify protocols such as:

```text
ARP
DNS
TCP
UDP
TLS
ICMP
```

This allows me to quickly identify different types of network communication.

---

# Packet Details

Selecting a packet provides more detailed information.

Depending on the packet, Wireshark may show layers such as:

```text
Frame
Ethernet
Internet Protocol
TCP / UDP
Application Protocol
```

This helps connect the networking layers together.

For example:

```text
Ethernet
    ↓
MAC Addresses

IP
    ↓
IP Addresses

TCP
    ↓
Ports + Flags

TLS
    ↓
Encrypted communication
```

---

# Capture Filters vs Display Filters

Wireshark supports two important types of filters.

## Capture Filters

Capture filters determine which traffic is recorded.

Traffic that does not match the capture filter may not be captured.

## Display Filters

Display filters determine which packets from an existing capture are currently shown.

The packets still exist in the capture; Wireshark simply hides packets that do not match the filter.

For learning and investigation, display filters are extremely useful.

---

# Basic Display Filters

## ARP

```text
arp
```

Shows ARP traffic.

---

## DNS

```text
dns
```

Shows DNS traffic.

---

## TCP

```text
tcp
```

Shows TCP traffic.

---

## UDP

```text
udp
```

Shows UDP traffic.

---

## ICMP

```text
icmp
```

Shows IPv4 ICMP traffic.

---

## TLS

```text
tls
```

Shows traffic Wireshark identifies as TLS.

---

# Filtering by IP Address

To show packets involving a specific IP address:

```text
ip.addr == 192.168.1.20
```

This can show traffic where that address is either the source or destination.

---

# Source IP

```text
ip.src == 192.168.1.20
```

Shows packets where the specified address is the source.

---

# Destination IP

```text
ip.dst == 192.168.1.20
```

Shows packets where the specified address is the destination.

---

# Filtering by TCP Port

Example:

```text
tcp.port == 443
```

Shows TCP traffic involving port 443.

---

# Filtering by UDP Port

Example:

```text
udp.port == 53
```

can help identify traditional DNS traffic using UDP port 53.

However, not all DNS communication necessarily uses UDP 53 because DNS can use other transports.

---

# Combining Filters

Wireshark filters can be combined.

Example:

```text
dns && ip.addr == 192.168.1.20
```

This means:

> Show DNS packets involving this IP address.

Another example:

```text
tcp && tcp.port == 443
```

This means:

> Show TCP packets involving port 443.

---

# TCP SYN Packets

To find packets where the SYN flag is set:

```text
tcp.flags.syn == 1
```

This can help identify TCP connection establishment.

---

# Initial SYN Without ACK

A more specific filter is:

```text
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

This can help identify initial TCP SYN packets.

---

# TCP Handshake

From Networking 07, I learned:

```text
Client → SYN → Server

Client ← SYN-ACK ← Server

Client → ACK → Server
```

Wireshark allows me to observe this process in real traffic.

This connects the TCP theory directly to packet analysis.

---

# DNS in Wireshark

A DNS exchange can conceptually look like:

```text
Client
   ↓
DNS Query

"What is the IP address for example.com?"

   ↓

DNS Server
   ↓
DNS Response

"Here is the answer."
```

Using:

```text
dns
```

I can isolate DNS packets and inspect information such as:

- Queried domain
- Query type
- Response
- Resolved addresses
- DNS server

when that DNS traffic is visible to the capture.

---

# ARP in Wireshark

Using:

```text
arp
```

I can inspect ARP communication.

An ARP request may conceptually say:

```text
Who has 192.168.1.1?
```

and a reply may provide the corresponding MAC address.

This connects directly to Networking 03.

---

# HTTPS Traffic

When accessing an HTTPS website, I may observe:

```text
DNS
TCP
TLS
```

A simplified flow can be:

```text
DNS
 ↓
Find server IP

TCP
 ↓
Establish connection

TLS
 ↓
Establish protected communication

HTTP
 ↓
Application communication protected by TLS
```

Because HTTPS encrypts HTTP content, I should not expect to simply read passwords or page contents from normal encrypted traffic.

---

# Encryption and Packet Analysis

Encryption does not make network traffic completely invisible.

Even when application content is encrypted, packet captures may still reveal metadata such as:

```text
Source IP
Destination IP
Ports
Timing
Packet sizes
Transport protocol
Connection behavior
```

The exact metadata visible depends on the protocol and environment.

This is important because network analysis often involves metadata and behavior rather than reading application content.

---

# Follow TCP Stream

Wireshark includes a feature called:

```text
Follow TCP Stream
```

This can reconstruct data associated with a TCP conversation.

For unencrypted protocols, application data may sometimes be readable.

For properly encrypted TLS traffic, the application content will normally remain encrypted unless appropriate session keys or other authorized decryption material are available.

---

# Packet Coloring

Wireshark can use colors to visually distinguish different packet types or conditions.

Colors can make patterns easier to notice, but I should not decide whether traffic is malicious based only on packet color.

The actual packet details and context matter.

---

# Packet Timing

The Time column can help identify when packets occurred.

Timing can be useful when investigating:

```text
Repeated connections
Bursts of traffic
Timeouts
Scanning patterns
Communication intervals
```

---

# Packet Length

The Length column shows packet/frame size information.

Packet sizes can sometimes contribute to traffic analysis, although size alone does not determine whether traffic is malicious.

---

# Security Investigation Mindset

When examining a capture, I should avoid randomly clicking packets.

A better approach is:

```text
1. Define the question

2. Identify relevant protocols

3. Apply filters

4. Identify endpoints

5. Examine timing

6. Inspect ports

7. Inspect protocol details

8. Follow relevant conversations

9. Compare behavior with expectations

10. Document observations
```

---

# Example Investigation

Suppose I want to understand what happens when I visit a website.

I could investigate:

```text
1. DNS

Which domain was queried?

        ↓

2. IP

What address did it resolve to?

        ↓

3. TCP

Was a connection established?

        ↓

4. TLS

Was encrypted communication established?

        ↓

5. Traffic

What endpoints communicated?
```

This is more useful than simply capturing thousands of packets without a question.

---

# Wireshark and Cybersecurity

Wireshark can support activities such as:

- Network troubleshooting
- Incident investigation
- Malware traffic analysis
- Protocol analysis
- CTF challenges
- Network-security education
- Identifying suspicious communication
- Understanding application behavior

However, packet captures can contain sensitive information.

They need to be handled carefully.

---

# Privacy

A packet capture can potentially contain:

```text
IP addresses
Hostnames
Domains
Network identifiers
Unencrypted application data
Cookies
Tokens
Credentials
Internal infrastructure information
```

I should review captures carefully before uploading them publicly.

Real packet captures from private networks should not automatically be committed to GitHub.

For public portfolio documentation, screenshots or sanitized examples may be safer.

---

# Authorized Capture

I should capture network traffic only in environments where I am authorized to do so.

Examples include:

- My own device
- My own network where appropriate
- Authorized labs
- CTF environments
- Intentionally provided packet captures

Capturing other people's private communications without authorization can create ethical and legal problems.

---

# Useful Wireshark Filters

```text
arp

dns

tcp

udp

icmp

tls

ip.addr == X.X.X.X

ip.src == X.X.X.X

ip.dst == X.X.X.X

tcp.port == 443

udp.port == 53

tcp.flags.syn == 1

tcp.flags.syn == 1 && tcp.flags.ack == 0
```

---

# Connecting Everything So Far

My networking learning now connects together:

```text
DOMAIN
   ↓
DNS
   ↓
IP ADDRESS
   ↓
ROUTING
   ↓
LOCAL NEXT HOP
   ↓
ARP / MAC
   ↓
TCP / UDP
   ↓
PORT
   ↓
TCP HANDSHAKE
   ↓
TLS
   ↓
HTTP / APPLICATION
```

Wireshark gives me a way to observe many parts of this process directly.

---

# What I Learned

This topic helped me understand what Wireshark is actually for.

It is not simply a tool that shows a huge list of packets. It allows network communication to be filtered, inspected, and connected back to networking concepts such as IP addresses, MAC addresses, ports, DNS, TCP, UDP, and TLS.

I also learned the importance of beginning an investigation with a question instead of randomly searching through traffic.

The next step is to perform a small real packet-analysis lab where I capture authorized traffic from my own system and identify DNS, IP addresses, TCP connection establishment, and encrypted web communication.
