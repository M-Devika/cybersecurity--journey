# Day 02 — DNS Packet Analysis with Wireshark

## Objective

To understand how DNS translates a domain name into an IP address and analyze DNS query and response packets using Wireshark.

## 1. Traffic Generation

I generated DNS traffic using:

```bash
nslookup google.com 8.8.8.8
