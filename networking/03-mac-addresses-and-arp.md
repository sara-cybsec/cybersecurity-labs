# Networking 03 — MAC Addresses & ARP

## Overview

IP addresses and MAC addresses both help devices communicate, but they operate at different parts of the networking process.

A simplified way to think about them is:

```text
IP Address
→ helps identify where traffic needs to go across IP networks

MAC Address
→ helps deliver frames between network interfaces on the local network
```

Understanding the difference becomes especially important when learning about switches, ARP, packet analysis, and local network security.

---

## MAC Addresses

MAC stands for:

**Media Access Control**

A MAC address identifies a network interface at the data-link layer.

A common MAC address format looks like:

```text
00:1A:2B:3C:4D:5E
```

A traditional MAC-48 address contains:

```text
48 bits
```

usually represented as six groups of hexadecimal values.

---

## Hexadecimal

MAC addresses use hexadecimal notation.

Hexadecimal uses:

```text
0 1 2 3 4 5 6 7 8 9 A B C D E F
```

This allows binary values to be represented more compactly.

For example:

```text
11111111
```

in binary is:

```text
FF
```

in hexadecimal.

---

## IP vs MAC Address

IP and MAC addresses serve different purposes.

### IP Address

Example:

```text
192.168.1.20
```

Used for logical addressing and routing across IP networks.

### MAC Address

Example:

```text
00:1A:2B:3C:4D:5E
```

Used for communication between network interfaces on the local link.

A useful simplified model is:

```text
IP
→ Where does the packet ultimately need to go?

MAC
→ Which interface should receive this frame on the current local link?
```

---

## Switches and MAC Addresses

Ethernet switches learn which MAC addresses are reachable through their ports.

Conceptually:

```text
             Switch
          /     |      \
         /      |       \
      PC-A    PC-B     Server
```

The switch can build a MAC address table containing information similar to:

```text
MAC Address          Port

AA:AA:AA:AA:AA:AA    Port 1
BB:BB:BB:BB:BB:BB    Port 2
CC:CC:CC:CC:CC:CC    Port 3
```

This helps the switch forward Ethernet frames toward the correct interface.

---

## The Local Network Problem

Suppose my computer wants to communicate with another device on the same IPv4 local network.

I may know its IP address:

```text
192.168.1.50
```

but Ethernet communication also needs a destination MAC address.

So my system needs a way to answer:

> Which MAC address currently corresponds to this IPv4 address on my local network?

This is where ARP is used.

---

# ARP

ARP stands for:

**Address Resolution Protocol**

ARP is used with IPv4 on local networks to resolve an IPv4 address to a link-layer address such as a MAC address.

Simplified:

```text
Known:
192.168.1.50

Need:
MAC address

        ↓

       ARP

        ↓

192.168.1.50
      =
AA:BB:CC:DD:EE:FF
```

---

## ARP Request

Imagine Device A wants to communicate with:

```text
192.168.1.50
```

but does not know its MAC address.

It can send an ARP request that conceptually asks:

> Who has 192.168.1.50?

The request is broadcast on the local network segment.

---

## ARP Reply

The device using that IPv4 address can respond with an ARP reply.

Conceptually:

```text
Device A:

Who has 192.168.1.50?

        ↓

Device B:

192.168.1.50 is associated with my MAC address:
AA:BB:CC:DD:EE:FF
```

Device A can then use that MAC address when sending local Ethernet frames.

---

## ARP Cache

Operating systems normally keep recently learned ARP information in a cache.

This prevents the system from needing to perform a new ARP request before every local communication.

A conceptual ARP cache could look like:

```text
IP Address       MAC Address

192.168.1.1      AA:AA:AA:AA:AA:AA
192.168.1.20     BB:BB:BB:BB:BB:BB
192.168.1.50     CC:CC:CC:CC:CC:CC
```

Depending on the operating system, ARP information can be inspected using commands such as:

```bash
arp -a
```

Modern Linux systems may also use:

```bash
ip neigh
```

to inspect neighboring devices.

---

## Communicating Outside the Local Network

An important detail is that my computer does not normally need the MAC address of a remote Internet server.

For example:

```text
My MacBook
192.168.1.20

        ↓

Remote Server
203.0.113.50
```

The remote server is not on my local network.

My computer instead sends the frame toward its next hop, commonly the default gateway.

So locally, the destination MAC address may belong to the router.

Simplified:

```text
Remote destination IP:
203.0.113.50

        ↓

Not on local subnet

        ↓

Send toward default gateway

        ↓

Need router's local MAC address
```

The IP destination can remain the remote server while the link-layer destination changes from hop to hop.

This distinction is important.

---

## Layer-by-Layer View

Imagine:

```text
My Laptop
IP: 192.168.1.20

Router
IP: 192.168.1.1

Remote Server
IP: 203.0.113.50
```

My laptop wants to communicate with the remote server.

At the IP layer:

```text
Source IP:
192.168.1.20

Destination IP:
203.0.113.50
```

But on the first local Ethernet/Wi-Fi link, the frame needs to reach the router.

So conceptually:

```text
Destination IP
→ Remote Server

Destination MAC
→ Local Router / Next Hop
```

This helped me understand why IP and MAC addresses cannot simply be treated as two versions of the same thing.

---

# ARP and Cybersecurity

ARP was designed for local network address resolution and does not provide strong authentication of ARP messages.

This creates security concerns on local networks.

---

## ARP Spoofing / ARP Poisoning

An attacker on an appropriate local network may attempt to send false ARP information.

For example, the attacker might try to convince a device:

```text
Router IP
192.168.1.1

        ↓

False ARP information

        ↓

Attacker's MAC address
```

If successful, traffic intended for the router may be redirected toward the attacker.

This technique is commonly called:

```text
ARP spoofing
```

or:

```text
ARP poisoning
```

---

## Potential Impact

Depending on the environment and other security controls, ARP spoofing can potentially contribute to:

- Man-in-the-middle positioning
- Traffic interception
- Traffic manipulation
- Connectivity disruption

However, being positioned between devices does not automatically mean encrypted application data can be read.

Protocols such as HTTPS provide additional protection at higher layers.

---

## Man-in-the-Middle Concept

A normal communication path might be:

```text
Victim
  ↓
Router
  ↓
Internet
```

A successful local interception scenario could attempt to create:

```text
Victim
  ↓
Attacker
  ↓
Router
  ↓
Internet
```

The attacker attempts to place their system between communicating devices.

Modern encryption and network-security controls can significantly affect what an attacker could actually observe or modify.

---

## Why HTTPS Still Matters

Suppose an attacker manages to observe network traffic.

If sensitive application data is transmitted without appropriate encryption, interception may expose that information.

With HTTPS:

```text
Client
   ↓
Encrypted application traffic
   ↓
Server
```

An observer may still see some network metadata, but properly validated TLS protects the confidentiality and integrity of HTTP content in transit.

This demonstrates why cybersecurity uses multiple layers of protection.

---

## Detecting Suspicious ARP Behavior

Potential indicators can include:

- Unexpected changes in IP-to-MAC mappings
- Multiple IP addresses unexpectedly mapping to the same MAC address
- Gateway MAC address changing unexpectedly
- Unusual ARP activity

These observations require context.

A changed MAC mapping is not automatically an attack because legitimate network changes can occur.

---

## IPv6 Note

ARP is specifically associated with IPv4.

IPv6 does not use ARP.

IPv6 uses:

**Neighbor Discovery Protocol (NDP)**

for related neighbor-discovery functions.

This is another reason IPv6 needs to be understood separately rather than treated as only a larger IPv4 address format.

---

## Connection to Wireshark

Later, when I use Wireshark, I should be able to recognize ARP traffic.

I may see communication conceptually similar to:

```text
Who has 192.168.1.1?
Tell 192.168.1.20
```

followed by an ARP reply.

Seeing this in a real packet capture will connect the theory to actual network behavior.

---

## Security Analysis Questions

If I encounter unusual ARP information, useful questions include:

```text
What IP address is involved?

What MAC address is involved?

Is this mapping expected?

Did the mapping recently change?

Is the MAC associated with the expected device?

Is there unusually frequent ARP traffic?

Could a legitimate network change explain this?
```

The goal is to investigate before concluding that an attack occurred.

---

## Key Terms

```text
MAC Address
Ethernet
Switch
MAC Address Table
ARP
ARP Request
ARP Reply
ARP Cache
Default Gateway
Next Hop
ARP Spoofing
ARP Poisoning
Man-in-the-Middle
Neighbor Discovery Protocol
```

---

## What I Learned

This topic helped me understand the difference between IP addresses and MAC addresses.

IP addresses help systems communicate across IP networks, while MAC addresses are used for frame delivery on the local link.

I learned that ARP helps an IPv4 device discover the MAC address associated with a local IPv4 address and that operating systems can cache these mappings.

One of the most important concepts was understanding communication with remote systems. My computer does not need the remote server's MAC address. It needs the MAC address of the local next hop, while the IP packet still identifies the remote destination.

I also learned why ARP has security implications. Because ARP itself does not strongly authenticate mappings, false ARP information can potentially be used in local network attacks such as ARP spoofing.

This will be useful later when I begin examining real ARP packets in Wireshark.
