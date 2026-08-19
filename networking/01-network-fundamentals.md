# Networking 01 — Network Fundamentals

## Overview

Before learning packet analysis, firewalls, ports, DNS, or network attacks, I first need to understand what a network actually is and how devices communicate.

A network is a group of devices that can communicate and exchange data.

Examples of network-connected devices include:

- Computers
- Phones
- Servers
- Routers
- Switches
- Printers
- Cameras
- IoT devices

The Internet is essentially a very large collection of interconnected networks.

---

## A Simple Example

If I open a website on my MacBook, the communication can be simplified as:

```text
My MacBook
    ↓
Wi-Fi
    ↓
Router
    ↓
Internet
    ↓
Website Server
```

The server then sends information back:

```text
Website Server
    ↓
Internet
    ↓
Router
    ↓
My MacBook
```

In reality, the path can involve many additional networking devices and systems.

---

## Client and Server

Many network interactions use a client-server model.

### Client

The client requests something.

Examples:

- Web browser
- Mobile application
- Email application

### Server

The server provides a service or resource.

For example:

```text
Browser
   ↓
Request
   ↓
Web Server
   ↓
Response
   ↓
Browser
```

When I visit a website, my browser acts as a client and communicates with a web server.

---

## Local Area Network — LAN

A LAN is a network covering a relatively small area.

Examples include:

- Home network
- School network
- Office network

Devices connected to the same home router are commonly part of the same local network.

Example:

```text
             Router
            /  |   \
           /   |    \
      Laptop  Phone  TV
```

These devices can potentially communicate locally depending on network configuration and security controls.

---

## Wide Area Network — WAN

A WAN connects networks across larger geographical areas.

The Internet is the largest example of a WAN.

Conceptually:

```text
Home LAN
   ↓
Router
   ↓
Internet / WAN
   ↓
Remote Network
```

---

## Router

A router connects different networks and forwards traffic between them.

For example, a home router commonly connects:

```text
Home Network
     ↕
Internet
```

Routers make forwarding decisions based primarily on IP addressing and routing information.

In cybersecurity, routers can also play roles in areas such as:

- Network segmentation
- Firewalling
- Traffic control
- Access restrictions
- Logging

depending on their capabilities and configuration.

---

## Switch

A switch connects devices within a local network.

Conceptually:

```text
          Switch
        /   |    \
       /    |     \
     PC   Server   Printer
```

Traditional Ethernet switches primarily use MAC addresses when forwarding frames within a LAN.

A switch and router perform different jobs.

A simplified distinction is:

```text
Switch
→ helps devices communicate within a local network

Router
→ helps different networks communicate
```

Real networks can contain devices with more advanced or combined functionality.

---

## Access Point

A wireless access point allows wireless devices to connect to a network.

For example:

```text
Laptop
Phone
Tablet
   ↓
Wi-Fi
   ↓
Wireless Access Point
   ↓
Network
```

Home devices often combine several functions into one physical box, including:

- Router
- Switch
- Wireless access point
- Firewall

So the device commonly called a "Wi-Fi router" may actually perform several networking roles.

---

## Packets

Data sent across networks is divided into smaller units.

At different networking layers these units can have different names, but the word **packet** is commonly used when discussing network traffic.

Instead of sending something as one giant block:

```text
Large Data
```

network communication involves structured units of data moving across the network.

Packets can contain information that helps systems determine things such as:

- Where data came from
- Where it is going
- Which protocol is being used
- How communication should be handled

Later, I will inspect real packets using Wireshark.

---

## Protocols

A protocol is a set of rules that systems follow when communicating.

Both systems need to understand the expected communication format.

Examples of networking protocols include:

```text
IP
TCP
UDP
DNS
HTTP
HTTPS
ARP
ICMP
```

Different protocols solve different networking problems.

For example:

```text
DNS
→ helps resolve domain names

HTTP
→ supports web communication

IP
→ supports addressing and routing packets

TCP
→ provides connection-oriented transport

ARP
→ helps resolve IPv4 addresses to MAC addresses on local networks
```

Understanding what each protocol does will make packet analysis much easier later.

---

## Network Interfaces

A device can have one or more network interfaces.

Examples include:

```text
Wi-Fi adapter
Ethernet adapter
Virtual network interface
Loopback interface
```

Each interface can have its own networking configuration.

This is why a computer can sometimes have multiple IP addresses.

---

## IP Address vs MAC Address

These are two different types of addresses.

### IP Address

IP addresses are used for logical addressing and routing across IP networks.

Example:

```text
192.168.1.20
```

### MAC Address

MAC addresses identify network interfaces at the data-link layer, particularly within local Ethernet/Wi-Fi networking.

Example format:

```text
AA:BB:CC:DD:EE:FF
```

A simplified way to think about them is:

```text
MAC
→ local network/interface communication

IP
→ communication and routing across IP networks
```

I will explore both more deeply in later notes.

---

## What Happens When I Visit a Website?

At a simplified level:

### Step 1 — Enter the domain

```text
https://example.com
```

### Step 2 — DNS

The system needs to determine an IP address associated with the domain.

```text
example.com
     ↓
DNS
     ↓
IP address
```

### Step 3 — Determine how to reach the destination

The operating system uses its network configuration and routing information to determine where traffic should be sent.

For remote Internet destinations, traffic commonly goes toward the local router/default gateway.

### Step 4 — Network communication

Packets travel through networking infrastructure toward the destination.

### Step 5 — Server receives the request

The destination server processes the communication.

### Step 6 — Server responds

Response traffic travels back toward my device.

### Step 7 — Browser processes the response

The browser uses the returned data to display the website.

This is simplified, but it gives me a mental model for what is happening behind a normal browser action.

---

## Why This Matters in Cybersecurity

Many cybersecurity events happen through networks.

Examples include:

```text
Port scanning
Suspicious DNS requests
Web attacks
Unauthorized connections
Malware communication
Remote access
Data exfiltration
Network reconnaissance
```

If I see:

```text
192.168.1.20 → 10.0.0.8:443
```

I eventually want to understand:

- What is the source?
- What is the destination?
- What does port 443 usually represent?
- Which protocol is being used?
- Is the connection expected?
- Which process created the connection?
- What traffic was exchanged?

Networking fundamentals make those questions possible.

---

## Basic Network Communication Model

A useful simplified model is:

```text
Application
    ↓
Network protocols
    ↓
Network interface
    ↓
Local network
    ↓
Router
    ↓
Other networks / Internet
    ↓
Destination
```

And the response travels back.

---

## Security Mindset

Seeing network traffic does not automatically mean something malicious is happening.

For example:

```text
Connection to unknown IP
```

does not automatically mean:

```text
Malware
```

The IP might belong to:

- A cloud provider
- Software update service
- CDN
- Website dependency
- API
- Legitimate background application

Security analysis requires context.

A useful mindset is:

```text
Observe
   ↓
Identify
   ↓
Understand
   ↓
Verify
   ↓
Determine whether behavior is expected
```

---

## Key Terms

```text
Network
Client
Server
LAN
WAN
Router
Switch
Access Point
Packet
Protocol
Network Interface
IP Address
MAC Address
DNS
Internet
```

---

## What I Learned

This topic helped me understand networking as communication between devices rather than just a list of protocols and commands.

I learned the basic differences between clients, servers, switches, routers, and wireless access points.

I also started building a mental model of what happens when I perform something as simple as opening a website. Even though the browser makes the process look immediate, several networking systems and protocols are working together underneath.

Understanding this path will help me later when I begin analyzing real packets and investigating network activity with Wireshark.
