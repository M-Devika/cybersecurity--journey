# TCP Packet Analysis

## 1. Traffic Generation

I generated controlled TCP traffic using:

```bash
curl https://example.com
```

The traffic was captured using Wireshark.

I applied the display filter:

```text
tcp
```

This allowed me to focus on TCP packets and identify the TCP connection establishment process.

---

## 2. TCP Three-Way Handshake

The TCP connection was established through the standard three-way handshake.

The three packets were analyzed individually to understand how TCP establishes a connection.

```text
Client                         Server

SEQ = 0
SYN
   --------------------------->

                               SEQ = 0
                               ACK = 1
                               SYN + ACK
   <---------------------------

SEQ = 1
ACK = 1
   --------------------------->
```

---

## 3. SYN Packet Analysis

The first packet was a TCP SYN packet sent from my system to the server.

Important observations:

```text
Source Port: 60736
Destination Port: 443
SYN: Set
ACK: Not Set
Relative Sequence Number: 0
```

Port `60234` is an ephemeral client port.

Port `443` is the standard port associated with HTTPS communication.

The SYN flag indicates that the client is initiating a TCP connection and synchronizing its initial sequence number.

---

## 4. SYN-ACK Packet Analysis

The second packet was a SYN-ACK response from the server.

Important observations:

```text
Source Port: 443
Destination Port: 60736
SYN: Set
ACK: Set
Relative Sequence Number: 0
Acknowledgment Number: 1
```

The source and destination ports are reversed compared with the SYN packet.

The acknowledgment number is `1` because the server is acknowledging the client's SYN.

The SYN consumes one sequence number, so:

```text
Client Sequence Number: 0

Server Acknowledgment:
0 + 1 = 1
```

The server also starts its own sequence-number space with a relative sequence number of `0`.

---

## 5. Final ACK Packet Analysis

The third packet was an ACK sent from my system to the server.

Important observations:

```text
Source Port: 60736
Destination Port: 443
ACK: Set
Relative Sequence Number: 1
Acknowledgment Number: 1
```

The sequence number becomes `1` because the client's SYN consumed one sequence number.

The acknowledgment number is `1` because the client is acknowledging the server's SYN.

Therefore:

```text
Client:
SEQ = 1

ACK = Server SEQ + 1
ACK = 0 + 1
ACK = 1
```

After this packet, the TCP three-way handshake is complete.

---

## 6. Sequence and Acknowledgment Relationship

Each direction of TCP communication maintains its own sequence-number space.

The handshake can be understood as:

```text
Client                         Server

Client sends:
SEQ = 0
SYN
   --------------------------->

Server responds:
SEQ = 0
ACK = 1
SYN + ACK
   <---------------------------

Client responds:
SEQ = 1
ACK = 1
   --------------------------->
```

The acknowledgment number indicates the next sequence number expected from the other endpoint.

In this handshake, each SYN consumes one sequence number.

Therefore:

```text
Client SYN:
SEQ = 0

Server ACK:
ACK = 1

Server SYN:
SEQ = 0

Client ACK:
ACK = 1
```

---

## 7. TCP Flags

The important TCP flags observed during the handshake were:

| Packet | SYN | ACK |
|---|---|---|
| SYN | Set | Not Set |
| SYN-ACK | Set | Set |
| ACK | Not Set | Set |

The flags indicate the purpose of each packet during connection establishment.

```text
SYN
→ Initiates synchronization.

SYN + ACK
→ Acknowledges the client's SYN and synchronizes the server's sequence number.

ACK
→ Acknowledges the server's SYN.
```

---

## 8. TCP Ports

The TCP communication used:

```text
Client Port: 60736
Server Port: 443
```

The client uses an ephemeral port to identify its side of the connection.

Port `443` identifies the server-side HTTPS service.

The direction changes between the request and response:

```text
SYN:
60736 → 443

SYN-ACK:
443 → 60736

ACK:
60736 → 443
```

---

## 9. TCP Window and Options

TCP headers contain additional fields used for flow control and communication parameter negotiation.

Important fields include:

```text
Window Size
Window Scale
Maximum Segment Size (MSS)
Selective Acknowledgment (SACK)
TCP Timestamps
```

The TCP window is used for flow control, allowing the receiver to advertise how much data it can currently accept.

TCP options can be exchanged during connection establishment to negotiate parameters between the two endpoints.

The exact values depend on the TCP connection observed in the capture.

---

## 10. Packet Encapsulation

The TCP packet can be viewed through the following protocol layers:
```text
Ethernet II
↓
IPv4
↓
TCP
↓
TLS/HTTP
```
### Ethernet II

Provides Layer 2 information such as source and destination MAC addresses.

### IPv4

Provides Layer 3 addressing information such as source and destination IP addresses.

### TCP

Provides reliable transport between the two endpoints using ports, sequence numbers, acknowledgment numbers, and control flags.

### HTTPS

Provides encrypted application-layer communication over the TCP connection.

---

## 11. Investigation Thinking

The initial TCP capture contained unrelated network traffic, including traffic associated with IPP.

This demonstrated that a packet capture records traffic occurring on the selected network interface and is not automatically limited to the activity intentionally generated by the analyst.

After generating a controlled connection using:

```bash
curl https://example.com
```

the TCP handshake associated with the HTTPS connection could be identified.

This shows the importance of correlating packets using:

```text
Source IP
Destination IP
Source Port
Destination Port
Protocol
TCP Flags
Sequence Numbers
Acknowledgment Numbers
```

rather than assuming that every TCP packet belongs to the activity being investigated.

---

## 12. Key Learning

This practical helped me understand TCP connection establishment at the packet level.

I learned how to:

- Identify a TCP three-way handshake
- Identify SYN, SYN-ACK, and ACK packets
- Analyze TCP source and destination ports
- Understand relative sequence numbers
- Understand acknowledgment numbers
- Understand TCP flags
- Understand the relationship between sequence and acknowledgment numbers
- Identify HTTPS traffic using TCP port 443
- Understand the purpose of TCP window information
- Distinguish intentionally generated traffic from unrelated background traffic

The main communication pattern observed was:

```text
SYN
↓
SYN-ACK
↓
ACK
↓
TCP Connection Established
```

---

## 13. Evidence

The screenshots included with this analysis show:

- TCP SYN packet
- TCP SYN-ACK packet
- TCP ACK packet
- TCP source and destination ports
- TCP flags
- Sequence numbers
- Acknowledgment numbers
- TCP three-way handshake

Sensitive IP and MAC address information has been redacted before publication.
