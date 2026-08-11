# Day 02 — DNS Packet Analysis with Wireshark

## Objective

To understand how DNS translates a domain name into an IP address and analyze DNS query and response packets using Wireshark.

## 1. Traffic Generation

I generated DNS traffic using:

```bash
nslookup google.com 8.8.8.8

I captured the traffic using Wireshark and applied the display filter:

dns
What I Analyzed
DNS Query
DNS Response
UDP source and destination ports
DNS transaction ID
Query and response relationship
DNS response status
DNS packet encapsulation
Key Observations
DNS communication was observed over UDP.
The DNS query used destination port 53.
The DNS response used source port 53.
The query used source port 41011.
The response was sent to destination port 41011.
The same transaction ID 0x743b was observed in the query and response.
The DNS response reported No error.
The query contained 1 question.
The response contained 1 answer record and 4 authority records.
Protocol Stack
Ethernet II
     ↓
IPv4
     ↓
UDP
     ↓
DNS
Evidence

The screenshots show:

DNS Query packet
DNS Response packet
UDP port information
DNS transaction ID
DNS response status
Query and response relationship

Sensitive IP and MAC address information has been redacted before publication.

Detailed Analysis

See analysis.md for the complete packet-level analysis.


### Commit message for this file

Use:

```text

