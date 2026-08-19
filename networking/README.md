# Networking

This section documents my networking fundamentals and hands-on network analysis practice.

Networking is one of the foundations of cybersecurity. Before I can properly understand firewalls, packet analysis, network attacks, intrusion detection, web traffic, or tools such as Wireshark, I need to understand how devices actually communicate.

## Learning Goals

- Understand how devices communicate across networks
- Understand IP and MAC addressing
- Learn the roles of switches and routers
- Understand ARP
- Understand TCP and UDP
- Learn common ports and protocols
- Understand DNS
- Understand HTTP and HTTPS
- Understand the TCP three-way handshake
- Learn how packets carry information
- Analyze real network traffic using Wireshark
- Connect networking concepts to cybersecurity

## Roadmap

- [ ] Network Fundamentals
- [ ] IP Addressing
- [ ] MAC Addresses & ARP
- [ ] TCP vs UDP
- [ ] Ports & Services
- [ ] DNS
- [ ] HTTP & HTTPS
- [ ] TCP Three-Way Handshake
- [ ] Wireshark Fundamentals
- [ ] Packet Analysis Lab

## Planned Structure

```text
networking/
├── README.md
├── 01-network-fundamentals.md
├── 02-ip-addressing.md
├── 03-mac-addresses-and-arp.md
├── 04-tcp-udp-and-ports.md
├── 05-dns.md
├── 06-http-and-https.md
├── 07-tcp-three-way-handshake.md
├── 08-wireshark-fundamentals.md
└── 09-packet-analysis-lab.md
```

## Why Networking Matters in Cybersecurity

Many security events involve network communication.

Examples include:

- Suspicious connections
- Unexpected listening ports
- Malicious DNS requests
- Web attacks
- Network reconnaissance
- Command-and-control traffic
- Data exfiltration
- Unauthorized remote access

Understanding networking helps me interpret what security tools are actually showing instead of simply memorizing commands or alerts.

## Hands-On Goal

This section will not contain only notes.

After learning the fundamentals, I plan to use Wireshark to capture and analyze real network traffic in an authorized environment.

The goal is to connect concepts such as:

```text
IP addresses
+
MAC addresses
+
Ports
+
Protocols
+
DNS
+
TCP
+
HTTP/HTTPS
```

to actual packets and network activity.

## Ethics

All networking and security exercises documented here will be performed using my own systems, authorized labs, CTF environments, or systems where I have explicit permission to perform testing.

---

**Status:** In Progress
