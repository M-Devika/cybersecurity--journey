# Day -05 ARP Packet Analysis Using Wireshark

A hands-on network security practical analyzing **Address Resolution Protocol (ARP)** traffic using Wireshark.

## Objective

The objective of this practical was to understand how ARP resolves an IPv4 address to a MAC address on a local network.

## Tools Used

* Wireshark
* Linux / Ubuntu
* `wlp2s0` Wi-Fi interface

## Traffic Capture

ARP traffic was captured on the `wlp2s0` interface using the Wireshark display filter:

```text
arp
```

## Packets Analyzed

The capture contains:

1. **ARP Request**
2. **ARP Reply**

### ARP Request

The ARP Request is sent as a Layer 2 broadcast:

```text
Destination MAC: ff:ff:ff:ff:ff:ff
```

The request asks which device owns a particular IP address.

The request contains:

* Sender MAC address
* Sender IP address
* Target IP address
* Target MAC address set to `00:00:00:00:00:00`

### ARP Reply

The ARP Reply provides the MAC address associated with the requested IP address.

The reply contains:

* Sender MAC address
* Sender IP address
* Target MAC address
* Target IP address

## Communication Flow

```text
ARP Request
"Who has this IP address?"
        |
        v
ARP Reply
"This IP address is associated with this MAC address."
```

## Screenshots

```text
screenshots/
├── 01-arp-request.png
└── 02-arp-reply.png
```

* `01-arp-request.png` — ARP Request packet and packet details.
* `02-arp-reply.png` — ARP Reply packet and packet details.

Sensitive IP and MAC address information has been redacted before publication.

## Key Learning

This practical demonstrated how ARP enables communication on a local network by resolving an IPv4 address to the corresponding MAC address.

It also provided practical experience identifying Layer 2 information and analyzing ARP Request and Reply packets in Wireshark.
