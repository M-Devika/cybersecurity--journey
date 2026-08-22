# Day 04 — HTTPS/TLS Packet Analysis with Wireshark

## Objective

To understand how HTTPS communication is protected using TLS and analyze TLS packets generated during an HTTPS connection using Wireshark.

## Environment

```text
Operating System: Ubuntu Linux
Tool: Wireshark
Network Interface: wlp2s0
Practical Task

I generated controlled HTTPS traffic using:

curl https://example.com

I captured the traffic using Wireshark and applied the display filter:

tls

This allowed me to focus on TLS packets involved in HTTPS communication.

Concepts Learned
HTTPS
TLS
TLS 1.2
TLS 1.3
TLS Handshake
Client and Server Communication
Encrypted Application Data
TLS Encryption
TCP Port 443
Packet Encapsulation
Packet Structure
Ethernet II
↓
IPv4
↓
TCP
↓
TLS
↓
HTTPS
Key Observations
HTTPS communication uses TCP port 443.

TLS packets were observed after TCP connection establishment.

TLS 1.2 and TLS 1.3 traffic were observed in the capture.

TLS handshake packets are used to establish secure communication.

Application Data packets carry encrypted communication.

The TLS layer protects application data from being transmitted as plaintext.
Investigation Lesson

Packet captures can reveal useful information about encrypted communication even when the actual application data cannot be directly read.

By examining TLS packet metadata, it is possible to identify information such as the TLS version, packet direction, handshake traffic, and encrypted application-data traffic.

Conclusion

This exercise helped me understand how HTTPS communication operates over TCP and how TLS provides encryption for application-layer communication.

I learned how to identify TLS traffic in Wireshark and distinguish TLS handshake traffic from encrypted application-data traffic.

This practical helped me move from analyzing TCP communication to understanding how secure HTTPS communication is established at the packet level.

Evidence

The screenshots included with this analysis show:

TLS packets
TLS 1.2 / TLS 1.3
TLS handshake traffic
Client and server TLS communication
Encrypted Application Data
TCP port 443

Sensitive IP and MAC address information has been redacted before publication.


