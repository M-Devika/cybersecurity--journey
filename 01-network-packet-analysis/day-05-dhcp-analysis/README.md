# Day 05 — DHCP Packet Analysis with Wireshark

## Objective

To understand DHCP communication at the packet level and analyze DHCP traffic using Wireshark.

## Environment

- **Operating System:** Ubuntu Linux
- **Tool:** Wireshark
- **Network Interface:** `wlp2s0`

## Practical

I attempted to capture DHCP traffic from my network interface using Wireshark.

The display filter used was:

`dhcp`

The expected DHCP communication follows the DORA process:
```text
DHCP Discover
      ↓
DHCP Offer
      ↓
DHCP Request
      ↓
DHCP ACK
Capture Result
```

The capture did not contain the complete DHCP exchange.

I observed:
```text
DHCP Request
      ↓
DHCP ACK
```
The DHCP Discover and DHCP Offer packets were not captured.

I documented the packets that were actually observed rather than treating the missing packets as captured evidence.



## Key Learning

Through this practical, I learned:

- The purpose of DHCP
- The DHCP DORA process
- How to filter DHCP traffic in Wireshark
- The purpose of DHCP Request
- The purpose of DHCP ACK
- How packet captures may contain only part of a protocol exchange
- The importance of documenting actual observations during network analysis

## Conclusion

This practical helped me understand DHCP communication and how it can be investigated using Wireshark.

Although I did not capture the complete DORA exchange, I successfully observed DHCP Request and DHCP ACK packets and documented the actual results of the capture.
