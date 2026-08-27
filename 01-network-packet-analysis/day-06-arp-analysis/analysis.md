# ARP Packet Analysis

## 1. Traffic Capture

ARP traffic was captured using Wireshark on the `wlp2s0` Wi-Fi interface.

The following display filter was used:

```text
arp
```

This filter was used to isolate ARP packets from the other traffic captured on the network interface.

---

## 2. Objective

The objective of this practical was to understand how ARP performs IPv4-to-MAC address resolution at the local network level.

The analysis focused on:

* ARP Request
* ARP Reply
* Ethernet Layer 2 addressing
* MAC addresses
* IPv4 addresses
* Broadcast communication
* IP-to-MAC address resolution

---

## 3. ARP Communication

ARP is used when a device knows the IPv4 address of another device on the local network but needs its MAC address to construct an Ethernet frame.

The basic communication is:

```text
ARP Request
      |
      v
ARP Reply
```

The Request asks:

```text
Who has this IP address?
```

The Reply provides:

```text
This IP address is associated with this MAC address.
```

---

## 4. ARP Request Analysis

The first analyzed packet is an ARP Request.

The Wireshark packet details show:

```text
Ethernet II
Address Resolution Protocol (request)
```

The Ethernet destination is:

```text
Broadcast
ff:ff:ff:ff:ff:ff
```

This means the request is sent to all devices on the local network because the sender does not yet know which device owns the target IP address.

The ARP section shows:

```text
Hardware type: Ethernet
Protocol type: IPv4
Hardware size: 6
Protocol size: 4
Opcode: request (1)
```

The packet also contains:

```text
Sender MAC address
Sender IP address
Target MAC address
Target IP address
```

The Target MAC address is:

```text
00:00:00:00:00:00
```

This represents the fact that the sender does not yet know the MAC address associated with the target IP.

### Interpretation

The ARP Request can therefore be understood as:

```text
Sender
  |
  | "Who has this target IP?"
  |------------------------------> Broadcast
  |
  | Target MAC is currently unknown
```

The sender knows the target IP address but needs the corresponding MAC address.

### Evidence

```text
screenshots/01-arp-request.png
```

---

## 5. ARP Reply Analysis

The second analyzed packet is an ARP Reply.

Wireshark identifies it as:

```text
Address Resolution Protocol (reply)
```

The packet shows:

```text
Opcode: reply (2)
```

Unlike the Request, the Reply contains the MAC address associated with the requested IP address.

The packet contains:

```text
Sender MAC address
Sender IP address
Target MAC address
Target IP address
```

The target MAC address corresponds to the device that originally sent the ARP Request.

### Interpretation

The communication can be represented as:

```text
Client
  |
  | ARP Request
  | "Who has the target IP?"
  |
  v
Local Network
  |
  | ARP Reply
  | "The target IP is associated with this MAC."
  |
  v
Client
```

This completes the IP-to-MAC resolution required for local Ethernet communication.

### Evidence

```text
screenshots/02-arp-reply.png
```

---

## 6. Ethernet Layer 2 Analysis

The ARP packets are carried using Ethernet Layer 2 addressing.

The Ethernet II section contains:

```text
Source MAC
Destination MAC
```

For the ARP Request, the destination is:

```text
ff:ff:ff:ff:ff:ff
```

This is the Ethernet broadcast MAC address.

The broadcast allows the ARP Request to reach devices on the local network.

The ARP Reply then provides the MAC address needed to identify the target device.

---

## 7. IP-to-MAC Resolution

The purpose of the exchange can be summarized as:

```text
Known:
Target IPv4 address

Unknown:
Target MAC address
```

ARP performs the resolution:

```text
IPv4 address
      |
      v
ARP Request
      |
      v
ARP Reply
      |
      v
MAC address
```

After obtaining the MAC address, the sender can use it as the Ethernet destination when communicating with that device on the local network.

---

## 8. Why the Request Uses Broadcast

The ARP Request uses:

```text
Destination: ff:ff:ff:ff:ff:ff
```

because the sender does not initially know which device has the target IP address.

Therefore, it asks every device on the local network:

```text
"Who has this IP address?"
```

The device that owns the requested IP can then respond with an ARP Reply.

---

## 9. ARP Request and Reply Comparison

| Feature              | ARP Request                   | ARP Reply                         |
| -------------------- | ----------------------------- | --------------------------------- |
| Opcode               | Request (1)                   | Reply (2)                         |
| Purpose              | Find MAC for an IP            | Provide the MAC address           |
| Ethernet destination | Broadcast                     | Specific MAC                      |
| Target MAC           | Unknown / `00:00:00:00:00:00` | Known                             |
| Direction            | Requesting device → network   | Target device → requesting device |

---

## 10. AzureWave Vendor Observation

Wireshark displays an **AzureWave** vendor name next to the MAC address in the Ethernet/ARP information.

This is a result of Wireshark identifying the manufacturer associated with the MAC address's **OUI (Organizationally Unique Identifier)**.

The important distinction is:

```text
MAC address
     |
     v
OUI
     |
     v
Vendor identification
     |
     v
AzureWave
```

This does not mean that AzureWave is a network protocol or that the ARP packet is communicating with Microsoft Azure.

The vendor name is simply associated with the MAC address through its OUI information.

---

## 11. Investigation Thinking

An important observation from this practical is that network communication can happen automatically without the user manually sending a message.

ARP traffic can be generated by the operating system and network stack when a device needs to communicate on the local network.

Therefore:

```text
No manual message from user
        ≠
No network communication
```

The operating system can perform network operations in the background.

Wireshark allows these protocol-level exchanges to be observed directly.

---

## 12. Observed Communication

The packets documented in this practical show:

```text
ARP Request
      |
      | "Who has the target IP?"
      v
ARP Reply
      |
      | "The target IP is associated with this MAC."
      v
IP-to-MAC resolution
```

The analysis is based on the packets actually observed in the capture.

---

## 13. Key Observations

The main observations were:

* ARP traffic was successfully captured using Wireshark.
* An ARP Request was observed.
* An ARP Reply was observed.
* The ARP Request used the Ethernet broadcast address.
* The Request contained an unknown Target MAC address.
* The ARP Reply provided the MAC address associated with the requested IP.
* ARP operates with IPv4 and MAC addresses.
* Ethernet II provides the Layer 2 source and destination MAC addresses.
* Wireshark can display the vendor associated with a MAC address through OUI information.

---

## 14. Key Learning

Through this practical, I learned:

* The purpose of ARP.
* How ARP performs IP-to-MAC address resolution.
* The difference between an ARP Request and ARP Reply.
* Why an ARP Request uses a broadcast destination MAC.
* Why the Target MAC is initially unknown in an ARP Request.
* How Ethernet Layer 2 addressing is visible in Wireshark.
* How Wireshark identifies a MAC address vendor using OUI information.
* How network communication can occur automatically in the background.

---

## 15. Evidence

The following screenshots provide evidence for the analysis:

### Screenshot 1 — ARP Request

```text
screenshots/01-arp-request.png
```

Shows:

* ARP filter
* ARP Request
* Ethernet II
* Broadcast destination
* ARP opcode
* Sender information
* Target information

### Screenshot 2 — ARP Reply

```text
screenshots/02-arp-reply.png
```

Shows:

* ARP Reply
* Ethernet II
* Source and destination MAC addresses
* ARP opcode
* Sender information
* Target information

Sensitive IP and MAC address information has been redacted before publication.

---

## 16. Conclusion

This practical provided hands-on experience analyzing ARP communication using Wireshark.

The captured Request and Reply demonstrate how a device resolves an IPv4 address to a MAC address before local Ethernet communication can take place.

The analysis also showed how Ethernet Layer 2 information, broadcast addressing, ARP fields, and OUI-based vendor identification can be observed directly from captured packets.

This practical helped connect the theoretical concept of ARP with an actual network packet capture.
