# Networking 02 — IP Addressing

## Overview

IP addressing is one of the most important foundations of networking.

An IP address allows devices and network interfaces to be identified and allows IP packets to be routed between networks.

Understanding IP addresses is important in cybersecurity because logs, firewall rules, packet captures, alerts, and security tools frequently contain source and destination IP addresses.

---

## IPv4

IPv4 stands for:

**Internet Protocol Version 4**

An IPv4 address contains 32 bits and is normally written as four decimal numbers separated by dots.

Example:

```text
192.168.1.25
```

Each section is called an **octet**.

```text
192 . 168 . 1 . 25
 ↑     ↑    ↑    ↑
octet octet octet octet
```

Each octet contains 8 bits.

Therefore:

```text
8 + 8 + 8 + 8 = 32 bits
```

Each octet can have a decimal value from:

```text
0 – 255
```

---

## Binary Representation

Computers represent IPv4 addresses using binary.

For example:

```text
192
```

can be represented as:

```text
11000000
```

Therefore an IPv4 address ultimately consists of 32 binary bits.

Understanding binary becomes especially useful when learning subnetting.

---

## Public and Private IPv4 Addresses

Not every IPv4 address is intended to be directly routed across the public Internet.

Private IPv4 ranges are commonly used inside local networks.

The private ranges are:

```text
10.0.0.0 – 10.255.255.255

172.16.0.0 – 172.31.255.255

192.168.0.0 – 192.168.255.255
```

Examples:

```text
192.168.1.15
10.0.0.20
172.16.4.8
```

These can be used inside private networks.

A public IP address is globally routable on the Internet, subject to routing and provider configuration.

---

## Why Private Addresses Exist

IPv4 contains a limited number of possible addresses.

Private addressing allows many local devices to use addresses that do not need to be globally unique on the Internet.

For example:

```text
Home Network

Laptop       192.168.1.10
Phone        192.168.1.11
TV           192.168.1.12
                    ↓
                  Router
                    ↓
              Public Internet
```

Many different homes can use the same private address ranges internally.

---

## NAT

NAT stands for:

**Network Address Translation**

A home router commonly uses NAT to allow multiple devices using private addresses to communicate with external networks using public addressing.

Simplified:

```text
192.168.1.10 ─┐
192.168.1.11 ─┼→ Router / NAT → Public Internet
192.168.1.12 ─┘
```

NAT modifies addressing information as traffic moves between networks.

NAT is an important networking concept, but NAT by itself should not be treated as a replacement for a firewall or other security controls.

---

## Loopback

A special IPv4 range is:

```text
127.0.0.0/8
```

The address most commonly encountered is:

```text
127.0.0.1
```

This refers to the local machine through the loopback interface.

It is commonly associated with:

```text
localhost
```

For example, a developer might run a local application at:

```text
127.0.0.1:8000
```

The traffic stays on the local system rather than being sent to another physical device.

---

## Network Address

A subnet has an address representing the network itself.

For example, in a typical:

```text
192.168.1.0/24
```

network, the network address is:

```text
192.168.1.0
```

This represents the subnet rather than a normal host address.

---

## Broadcast Address

Traditional IPv4 subnets can also have a broadcast address.

For:

```text
192.168.1.0/24
```

the broadcast address is:

```text
192.168.1.255
```

A broadcast can be used to address all hosts on that local subnet for protocols that use IPv4 broadcast.

---

## Host Addresses

For a traditional `/24` subnet:

```text
Network:
192.168.1.0

Usable host range:
192.168.1.1 – 192.168.1.254

Broadcast:
192.168.1.255
```

That gives:

```text
254 traditionally usable host addresses
```

because the network and broadcast addresses are reserved for their respective purposes.

---

## Subnet Mask

A subnet mask helps determine which part of an IPv4 address represents the network and which part represents host addressing.

A common subnet mask is:

```text
255.255.255.0
```

This can also be written using CIDR notation as:

```text
/24
```

---

## CIDR

CIDR stands for:

**Classless Inter-Domain Routing**

Example:

```text
192.168.1.0/24
```

The:

```text
/24
```

means that the first 24 bits represent the network prefix.

Conceptually:

```text
NETWORK                    HOST
11111111.11111111.11111111.00000000
   8   +    8    +    8
              = 24
```

This corresponds to:

```text
255.255.255.0
```

---

## `/24`

A `/24` IPv4 subnet contains:

```text
32 - 24 = 8 host bits
```

Eight bits provide:

```text
2^8 = 256 total addresses
```

In a traditional subnet where network and broadcast addresses are reserved:

```text
256 - 2 = 254 usable host addresses
```

---

## `/16`

A `/16` has:

```text
16 network bits
16 host bits
```

Subnet mask:

```text
255.255.0.0
```

Total addresses:

```text
2^16 = 65,536
```

---

## `/8`

A `/8` has:

```text
8 network bits
24 host bits
```

Subnet mask:

```text
255.0.0.0
```

Total addresses:

```text
2^24 = 16,777,216
```

---

## Default Gateway

A default gateway is the router or next-hop device a host commonly sends traffic to when the destination is outside its directly connected networks and no more specific route exists.

Example:

```text
Laptop
192.168.1.20
      ↓
Default Gateway
192.168.1.1
      ↓
Other Networks
```

The exact address of the gateway depends on the network configuration.

It is not always `.1`.

---

## Source and Destination IP Addresses

IP packets contain source and destination addressing information.

Conceptually:

```text
Source
192.168.1.20

        ↓ packet ↓

Destination
203.0.113.50
```

In security analysis, these are extremely important.

A log might contain:

```text
Source IP:      192.168.1.20
Destination IP: 203.0.113.50
Destination Port: 443
```

An analyst can then investigate:

- What device owns the source IP?
- Is the destination expected?
- Is the traffic internal or external?
- Which service is being contacted?
- How often is the connection occurring?

---

## Static vs Dynamic IP Addresses

### Static

A static IP address is manually or otherwise persistently configured to remain stable.

Servers and infrastructure sometimes use stable addressing because other systems need to reliably locate them.

### Dynamic

A dynamic IP address can be automatically assigned and may change over time.

DHCP is commonly used to automatically provide network configuration to devices.

---

## DHCP

DHCP stands for:

**Dynamic Host Configuration Protocol**

DHCP can provide devices with configuration such as:

```text
IP address
Subnet mask/prefix
Default gateway
DNS server
```

Without automatic configuration, these values may need to be configured manually.

---

## IPv6

IPv6 was created in part to address the limitations of IPv4 address space.

IPv6 addresses contain:

```text
128 bits
```

Example:

```text
2001:db8:abcd:0012::1
```

IPv6 uses hexadecimal notation and provides a vastly larger address space.

IPv6 also changes or removes some IPv4 behaviors, so IPv6 should eventually be studied as its own topic rather than treated as simply "larger IPv4."

---

## Why IP Addressing Matters in Cybersecurity

Security tools constantly show IP addresses.

Examples include:

```text
Firewall logs
SIEM alerts
Packet captures
Web server logs
IDS/IPS alerts
Authentication logs
DNS logs
Cloud logs
```

If I see:

```text
192.168.1.15
```

I should recognize that it belongs to a private IPv4 range.

If I see:

```text
127.0.0.1
```

I should recognize loopback/local communication.

If I see:

```text
192.168.1.0/24
```

I should understand that this describes an entire subnet rather than one individual host.

---

## Security Analysis Example

Imagine an alert says:

```text
Source:      192.168.1.45
Destination: 198.51.100.20
Port:        443
```

Before deciding whether it is suspicious, I would need context.

Questions could include:

```text
What device is 192.168.1.45?

Who was using it?

What organization/service owns the destination?

What process created the connection?

Is HTTPS traffic expected?

How often does this happen?

Was a large amount of data transferred?
```

An IP address alone does not prove malicious activity.

---

## Key Terms

```text
IPv4
IPv6
Octet
Binary
Private IP
Public IP
NAT
Loopback
Network Address
Broadcast Address
Host Address
Subnet Mask
CIDR
Default Gateway
DHCP
Source IP
Destination IP
```

---

## What I Learned

This topic helped me understand that an IP address contains more information than simply identifying a device.

Network prefixes and subnet masks determine which addresses belong to the same network, while routers and default gateways help traffic move between networks.

I also learned the private IPv4 ranges, the purpose of loopback addressing, and the basic purpose of NAT and DHCP.

From a cybersecurity perspective, IP addresses appear almost everywhere, but they need context. Seeing an unfamiliar address does not automatically mean something is malicious. I need to understand whether the address is private or public, which system it belongs to, what communication occurred, and whether that behavior is expected.
