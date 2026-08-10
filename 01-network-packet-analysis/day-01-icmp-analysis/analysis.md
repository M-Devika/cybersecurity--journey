# ICMP Packet Analysis

## 1. Traffic Generation

I generated ICMP traffic using the ping command:

    ping -c 4 8.8.8.8

I captured the traffic using Wireshark and applied the display filter:

    icmp

## 2. Echo Request

The Echo Request is sent from my system toward the destination.

Important observations:

- ICMP Type: 8
- ICMP Code: 0
- Source IP: my system's IP address
- Destination IP: destination system's IP address
- Sequence Number: used to identify the individual request

## 3. Echo Reply

The destination responds with an ICMP Echo Reply.

Important observations:

- ICMP Type: 0
- ICMP Code: 0
- Source and destination IP addresses are reversed compared with the request.
- The sequence number matches the corresponding Echo Request, allowing the request and reply to be associated.

## 4. Packet Encapsulation

The packet can be viewed as:

    Ethernet II
        ↓
    IPv4
        ↓
    ICMP

### Ethernet II

Provides Layer 2 information such as MAC addresses.

### IPv4

Provides Layer 3 information such as source and destination IP addresses.

### ICMP

Provides information about the ICMP control message.

## 5. Request vs Reply

| Property | Echo Request | Echo Reply |
|---|---|---|
| ICMP Type | 8 | 0 |
| ICMP Code | 0 | 0 |
| Direction | Source → Destination | Destination → Source |
| Sequence | Identifies request | Corresponds to request |

## 6. Investigation Thinking

If an Echo Request is sent but no Echo Reply is received, I should not
immediately conclude that the destination is down.

Possible areas to investigate include:

- Network connectivity
- Packet loss
- Routing
- Firewall filtering
- ICMP filtering
- Local interface problems

## 7. Key Learning

This exercise helped me understand that the ping command generates
actual network packets that can be captured and analyzed.

I also learned to examine network communication layer by layer instead
of looking at the traffic only as a connectivity test.

## Evidence

The screenshots in this folder show:
- ICMP Echo Request and Echo Reply traffic
- Packet-level protocol encapsulation
- IPv4 and ICMP header fields
- Request/reply sequence matching

Sensitive IP and MAC address information has been redacted before publication.

