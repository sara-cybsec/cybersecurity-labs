# Networking 07 — TCP Three-Way Handshake

## Overview

TCP stands for:

**Transmission Control Protocol**

TCP is a connection-oriented transport protocol.

Before two applications begin exchanging normal data over a new TCP connection, TCP establishes the connection using a process called the:

**Three-Way Handshake**

The three main steps are:

```text
SYN
SYN-ACK
ACK
```

Understanding this process is important in cybersecurity because TCP connection behavior appears in:

- Wireshark captures
- Firewall logs
- IDS/IPS alerts
- Network troubleshooting
- Port scanning
- Incident response
- Network attacks
- CTF challenges

---

# The Basic Handshake

Imagine my computer wants to connect to a web server using TCP.

The process can be simplified as:

```text
CLIENT                         SERVER

   |                              |
   | -------- SYN --------------> |
   |                              |
   | <------ SYN-ACK ------------ |
   |                              |
   | -------- ACK --------------> |
   |                              |
   |      CONNECTION READY        |
```

After the handshake completes, application data can normally begin flowing.

---

# Step 1 — SYN

The client begins by sending:

```text
SYN
```

SYN means:

**Synchronize**

Conceptually, the client is saying:

> I want to establish a TCP connection.

Example:

```text
Client
192.168.1.20:53142

        ↓ SYN

Server
203.0.113.50:443
```

The client is attempting to establish a TCP connection to the server.

---

# Step 2 — SYN-ACK

If the server is listening and accepts the connection attempt, it can respond with:

```text
SYN-ACK
```

This combines:

```text
SYN
+
ACK
```

Conceptually:

> I received your request, and I am also synchronizing with you.

The communication now looks like:

```text
Client

   SYN
    ↓

Server

 SYN-ACK
    ↓

Client
```

---

# Step 3 — ACK

The client responds with:

```text
ACK
```

ACK means:

**Acknowledgment**

Conceptually:

> I received your response.

The handshake is now complete.

```text
Client                         Server

SYN -------------------------->

    <------------------- SYN-ACK

ACK -------------------------->

        CONNECTION ESTABLISHED
```

---

# Why Three Steps?

TCP needs both endpoints to establish connection state and synchronize information used for reliable communication.

The handshake allows both sides to confirm that communication can occur in both directions.

It also establishes initial TCP sequence-number state.

---

# TCP Flags

TCP headers contain control flags.

Important flags include:

```text
SYN
ACK
FIN
RST
PSH
URG
```

For now, the most important are:

```text
SYN
ACK
FIN
RST
```

---

# SYN

Used during TCP connection establishment.

```text
SYN
→ synchronize sequence numbers / begin connection establishment
```

---

# ACK

Indicates acknowledgment information is valid.

ACKs are used throughout established TCP communication, not only during the handshake.

---

# FIN

FIN is commonly used during graceful TCP connection termination.

Conceptually:

> I have finished sending data.

---

# RST

RST means:

**Reset**

A TCP reset can abruptly terminate or reject a connection.

For example, depending on the situation, attempting to connect to a TCP port where no service is listening may result in:

```text
RST
```

---

# Sequence Numbers

TCP uses sequence numbers to keep track of data.

Imagine data conceptually as:

```text
A B C D
```

TCP needs to know how transmitted bytes fit into the ordered byte stream.

Sequence numbers help TCP maintain ordering and detect missing data.

---

# Acknowledgment Numbers

Acknowledgment numbers tell the sender which data the receiver expects next.

Simplified:

```text
Sender:
Here is data.

        ↓

Receiver:
I received it.
I expect the next part now.
```

Together, sequence and acknowledgment numbers support TCP's reliable delivery mechanisms.

---

# Example Connection

Imagine:

```text
Client:
192.168.1.20:53142

Server:
203.0.113.50:443
```

A simplified handshake could appear as:

```text
192.168.1.20:53142
        ↓
       SYN
        ↓
203.0.113.50:443


203.0.113.50:443
        ↓
     SYN-ACK
        ↓
192.168.1.20:53142


192.168.1.20:53142
        ↓
       ACK
        ↓
203.0.113.50:443
```

The TCP connection is then established.

---

# What Happens After the Handshake?

Once TCP establishes the connection, the applications can exchange data.

For HTTPS, a simplified sequence can be:

```text
DNS Resolution
      ↓
IP Address
      ↓
TCP Three-Way Handshake
      ↓
TLS Handshake
      ↓
Encrypted HTTP Communication
```

This connects several networking lessons together.

---

# TCP vs TLS Handshake

These are different processes.

### TCP Handshake

Creates the TCP transport connection.

```text
SYN
SYN-ACK
ACK
```

### TLS Handshake

Establishes cryptographic parameters and authenticates the server for HTTPS.

Simplified:

```text
TCP connection
      ↓
TLS negotiation
      ↓
Encrypted application communication
```

HTTPS using HTTP/1.1 or HTTP/2 normally relies on both.

---

# TCP Connection States

TCP connections can move through different states.

Examples include:

```text
LISTEN
SYN-SENT
SYN-RECEIVED
ESTABLISHED
FIN-WAIT
CLOSE-WAIT
TIME-WAIT
CLOSED
```

I do not need to memorize every state yet, but recognizing:

```text
LISTEN
ESTABLISHED
TIME-WAIT
```

can be useful during system and network investigation.

---

# LISTEN

A server waiting for incoming TCP connections can be in a listening state.

Conceptually:

```text
Web Server
     ↓
LISTEN
     ↓
TCP 443
```

---

# ESTABLISHED

After a successful handshake:

```text
ESTABLISHED
```

means a TCP connection exists between the endpoints.

---

# TIME-WAIT

After some TCP connections close, one endpoint can temporarily remain in:

```text
TIME-WAIT
```

This is normal TCP behavior and is not automatically evidence of a problem.

---

# Connection Termination

TCP connection termination can involve:

```text
FIN
ACK
```

from both sides.

A simplified graceful close can look like:

```text
Client ---- FIN ----> Server

Client <--- ACK ----- Server

Client <--- FIN ----- Server

Client ---- ACK ----> Server
```

The exact sequence can vary depending on which side closes first and timing.

---

# RST — Abrupt Reset

A connection can also be terminated using:

```text
RST
```

This is more abrupt than a normal FIN-based close.

RST packets can appear during:

- Connection rejection
- Application errors
- Unexpected connection states
- Closed-port responses
- Network scanning
- Security controls

Context is needed to understand why a reset occurred.

---

# TCP Handshake in Port Scanning

Understanding the handshake helps explain network scanning.

Suppose an authorized scanner sends:

```text
SYN
```

to a TCP port.

Different responses can provide information.

A simplified example:

### Possible Open Port

```text
Scanner → SYN

Target → SYN-ACK
```

This suggests a service may be listening.

### Possible Closed Port

```text
Scanner → SYN

Target → RST
```

This can indicate that the host is reachable but no service is listening on that port.

Firewalls can change this behavior.

---

# SYN Scanning

A commonly discussed Nmap technique is:

```text
SYN scan
```

The scanner sends SYN packets and examines responses without necessarily completing normal application communication.

This is sometimes called a:

**half-open scan**

I will only practice port scanning in:

- Authorized labs
- CTF environments
- My own systems
- Systems where I have explicit permission

---

# SYN Flood

The TCP handshake can also be abused.

A:

**SYN flood**

is a denial-of-service technique involving large numbers of TCP connection attempts.

Conceptually:

```text
Attacker

SYN
SYN
SYN
SYN
SYN
SYN
 ↓
Server
```

The attacker may attempt to consume resources by creating many incomplete connection attempts.

Modern operating systems and network-security devices can use protections against this behavior.

---

# SYN Cookies

One defense against certain SYN-flood conditions is:

**SYN cookies**

They allow a system to avoid allocating some connection state until the client proves it received the server's response.

This can help reduce resource exhaustion from incomplete connection attempts.

---

# Handshake and Firewalls

Firewalls can track TCP connection state.

A stateful firewall can understand whether traffic belongs to:

```text
NEW connection

ESTABLISHED connection

RELATED communication
```

This allows more intelligent filtering than simply looking at individual packets independently.

---

# Stateful Firewall

A stateful firewall keeps information about network connections.

For example:

```text
Internal device
     ↓
Starts TCP connection
     ↓
Firewall records connection
     ↓
Return traffic is recognized
```

This helps the firewall determine whether incoming traffic belongs to a legitimate existing connection.

---

# Handshake in Wireshark

In the next lesson, I will begin learning Wireshark.

A TCP handshake can often be identified using packets containing:

```text
[SYN]

[SYN, ACK]

[ACK]
```

Conceptually:

```text
1  Client → Server   SYN

2  Server → Client   SYN, ACK

3  Client → Server   ACK
```

Seeing this in a real capture will help connect the theory to actual network traffic.

---

# Wireshark Filters

Later, useful Wireshark display filters can include:

```text
tcp
```

to show TCP traffic.

A filter such as:

```text
tcp.flags.syn == 1
```

can help identify packets with the SYN flag set.

More specific filtering can be used during actual packet analysis.

---

# Security Investigation Example

Imagine a server log or network capture shows:

```text
Thousands of SYN packets

from many sources

with very few completed handshakes
```

An analyst might investigate whether this represents:

```text
Scanning
SYN flood activity
Network issue
Misconfigured application
Monitoring artifact
```

Again:

```text
Unusual behavior
≠
automatic attack
```

Context matters.

---

# Another Example

Suppose one internal device repeatedly sends SYN packets to:

```text
10.0.0.20:22
10.0.0.20:23
10.0.0.20:25
10.0.0.20:80
10.0.0.20:443
10.0.0.20:445
```

This could resemble port-scanning behavior.

An analyst should investigate:

```text
Which device sent the traffic?

Which process generated it?

Was scanning authorized?

Is this a vulnerability scanner?

Is this normal administration?

Could the device be compromised?
```

---

# Connecting the Networking Lessons

At this point, the networking concepts begin connecting together.

When I visit:

```text
https://example.com
```

a simplified process can look like:

```text
1. DNS

example.com
     ↓
IP Address


2. Routing

My device determines where traffic should go.


3. Local Delivery

ARP may help determine the MAC address of the local next hop.


4. TCP

SYN
SYN-ACK
ACK


5. TLS

Secure cryptographic connection established.


6. HTTP

Browser sends HTTP request.


7. Response

Server returns data.
```

This is the first time the earlier networking lessons begin forming one complete communication process.

---

# Key Terms

```text
TCP
Three-Way Handshake
SYN
SYN-ACK
ACK
FIN
RST
Sequence Number
Acknowledgment Number
LISTEN
ESTABLISHED
TIME-WAIT
SYN Scan
SYN Flood
SYN Cookies
Stateful Firewall
Connection State
```

---

# Security Takeaways

The main lessons are:

- TCP establishes connections using SYN, SYN-ACK, and ACK.
- Sequence and acknowledgment numbers support reliable communication.
- TCP flags provide information about connection state.
- FIN is commonly associated with graceful termination.
- RST can abruptly reset or reject a connection.
- TCP behavior can help identify open and closed ports.
- Port scanners can analyze TCP responses.
- SYN floods abuse connection establishment for denial-of-service attempts.
- Stateful firewalls track connection information.
- TCP handshakes are visible in packet captures and useful during security analysis.
- Network behavior always needs context before being classified as malicious.

---

# What I Learned

This topic helped me understand what "connection-oriented" actually means when describing TCP.

Before application data is exchanged, the client and server establish connection state through the SYN, SYN-ACK, and ACK process.

I also learned that TCP flags are useful for much more than simply creating connections. They can provide information about connection establishment, acknowledgment, termination, resets, scanning behavior, and potential network problems.

Most importantly, this lesson connected several earlier networking topics together. DNS helps find an IP address, routing determines where traffic goes, ARP can help reach the local next hop, TCP establishes a connection, TLS can secure it, and HTTP can then carry web communication.

The next step is to stop looking only at diagrams and begin identifying these protocols in real network traffic using Wireshark.
