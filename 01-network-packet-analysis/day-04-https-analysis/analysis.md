# HTTPS/TLS Packet Analysis

## 1. Traffic Generation

I generated HTTPS traffic using:

```bash
curl https://example.com
```

The traffic was captured using Wireshark.

I applied the display filter:

```text
tls
```

This allowed me to focus on packets identified by Wireshark as TLS traffic.

---

## 2. Objective

The objective of this practical was to analyze HTTPS communication at the packet level and understand how TLS is used to establish secure communication and protect application data.

The analysis focused on:

- TLS packet identification
- Client Hello
- Server Hello
- TLS handshake information
- Cipher suites
- TLS extensions
- Change Cipher Spec
- Encrypted Application Data
- TCP port 443
- TLS packet structure

---

## 3. TLS Packet Overview

The first screenshot shows the TLS packets identified in the Wireshark capture.

The packet list contains entries displayed as:

```text
TLSv1.2  Application Data
TLSv1.2  Application Data
TLSv1.3  Client Hello
TLSv1.3  Server Hello, Change Cipher Spec, Application Data
TLSv1.3  Change Cipher Spec, Application Data
TLSv1.3  Application Data
```

The capture therefore contains packets displayed by Wireshark with both `TLSv1.2` and `TLSv1.3` labels.

The handshake examined in the following screenshots is dissected by Wireshark as **TLS 1.3**.

### Screenshot

```text
screenshots/01-tls-packets.png
```

---

## 4. Client Hello Analysis

The second screenshot shows a **Client Hello** packet.

Wireshark identifies the packet as:

```text
TLSv1.3 Record Layer: Handshake Protocol: Client Hello
```

Important fields visible in the packet include:

```text
Content Type: Handshake (22)
Version: TLS 1.0 (0x0301)
Handshake Type: Client Hello (1)
Length: 508
Version: TLS 1.2 (0x0303)
Session ID Length: 32
Cipher Suites Length: 62
Cipher Suites: 31 suites
Compression Methods Length: 1
Extensions Length: 373
```

The Client Hello also contains several TLS extensions, including:

```text
server_name
ec_point_formats
supported_groups
application_layer_protocol_negotiation
encrypt_then_mac
extended_master_secret
post_handshake_auth
signature_algorithms
supported_versions
psk_key_exchange_modes
key_share
```

The Client Hello provides information used during TLS negotiation, including supported protocol versions, cipher suites, extensions, and key-exchange information.

### Version Field Observation

The screenshot contains several version-related fields.

The packet is identified by Wireshark as:

```text
TLSv1.3 Record Layer
```

However, the Client Hello contains:

```text
Version: TLS 1.2 (0x0303)
```

This `0x0303` value should not be interpreted by itself as proof that the connection is using TLS 1.2.

TLS 1.3 retains `0x0303` as a legacy version value in the Client Hello for compatibility. The `supported_versions` extension is used for TLS version negotiation.

Therefore, the Client Hello shown in the screenshot is analyzed as part of the TLS 1.3 handshake identified by Wireshark.

### Screenshot

```text
screenshots/02-client-hello.png
```

---

## 5. Server Hello Analysis

The third screenshot shows the **Server Hello** packet.

Wireshark identifies the packet as:

```text
TLSv1.3 Record Layer: Handshake Protocol: Server Hello
```

Important fields visible in the packet include:

```text
Handshake Type: Server Hello (2)
Length: 118
Version: TLS 1.2 (0x0303)
Session ID Length: 32
Cipher Suite: TLS_AES_256_GCM_SHA384
Compression Method: null (0)
Extensions Length: 46
```

The Server Hello contains extensions including:

```text
key_share
supported_versions
```

The Server Hello represents the server's response to the Client Hello and contains parameters selected during TLS negotiation.

The selected cipher suite visible in the screenshot is:

```text
TLS_AES_256_GCM_SHA384
```

The `Version: TLS 1.2 (0x0303)` field is a legacy version value used within the TLS 1.3 handshake and should not be interpreted alone as the negotiated TLS version.

### Screenshot

```text
screenshots/03-server-hello.png
```

---

## 6. Encrypted Application Data

The Server Hello screenshot also shows:

```text
TLSv1.3 Record Layer: Application Data Protocol: http-over-tls
```

The packet contains:

```text
Content Type: Application Data (23)
Version: TLS 1.2 (0x0303)
Encrypted Application Data
```

The actual HTTP application content is not directly visible because it is protected by TLS encryption.

The communication can be represented as:

```text
HTTP Application Data
        |
        v
       TLS
        |
        v
Encrypted Application Data
```

This demonstrates that TLS protects the application data while some protocol metadata remains visible to a network analyst.

---

## 7. Change Cipher Spec

The Server Hello screenshot also contains:

```text
TLSv1.3 Record Layer: Change Cipher Spec Protocol
```

A Change Cipher Spec message can appear in TLS 1.3 for compatibility purposes.

Its presence does not mean that the connection has switched to TLS 1.2.

In this capture, it appears alongside TLS 1.3 handshake and application-data records.

---

## 8. TLS Cipher Suite

The Server Hello packet shows the selected cipher suite:

```text
TLS_AES_256_GCM_SHA384
```

This cipher suite identifies the authenticated-encryption algorithm and hash function used by the TLS 1.3 cipher suite.

It uses:

- **AES-256-GCM** for authenticated encryption
- **SHA-384** as the associated hash function

The cipher suite is selected during the TLS handshake.

---

## 9. TLS Extensions

The Client Hello screenshot shows several TLS extensions used during negotiation.

Important extensions visible include:

```text
server_name
application_layer_protocol_negotiation
supported_versions
signature_algorithms
key_share
supported_groups
```

Examples:

```text
server_name
→ Identifies the requested server name.

supported_versions
→ Indicates the TLS versions supported by the client.

signature_algorithms
→ Indicates supported signature algorithms.

key_share
→ Provides key-exchange information.

supported_groups
→ Indicates supported cryptographic groups.
```

These extensions provide additional information required for TLS negotiation.

---

## 10. HTTPS and TLS Relationship

HTTPS is HTTP communication protected using TLS.

The relationship can be represented as:

```text
HTTP
 |
 v
TLS
 |
 v
TCP
 |
 v
IPv4
 |
 v
Ethernet II
```

The standard server-side port associated with HTTPS is:

```text
443
```

TLS provides cryptographic protection for HTTP application data transported through the TCP connection.

---

## 11. Packet Encapsulation

The HTTPS communication can be viewed through the following protocol stack:

```text
Ethernet II
 |
 v
IPv4
 |
 v
TCP
 |
 v
TLS
 |
 v
HTTP
```

### Ethernet II

Provides Layer 2 information such as source and destination MAC addresses.

### IPv4

Provides Layer 3 addressing information such as source and destination IP addresses.

### TCP

Provides reliable transport between the client and server using ports, sequence numbers, acknowledgment numbers, and TCP control flags.

### TLS

Provides cryptographic protection for the application-layer communication.

### HTTP

Provides the application-layer protocol used for web communication.

When HTTP is protected by TLS, the resulting communication is HTTPS.

---

## 12. Why the `tls` Filter Was Used

The Wireshark display filter used for this practical was:

```text
tls
```

This filter was selected because the objective was to analyze TLS packets involved in HTTPS communication.

Other filters can be used for different analysis purposes:

```text
tcp
```

Shows TCP traffic.

```text
tcp.port == 443
```

Shows TCP traffic associated with port 443.

```text
tls
```

Focuses on packets identified by Wireshark as TLS.

For this practical, the `tls` filter made it easier to identify:

```text
Client Hello
Server Hello
Change Cipher Spec
Application Data
```

---

## 13. Investigation Thinking

The packet capture demonstrates that encrypted communication can still provide useful information to a network analyst.

Although the application data is encrypted, the capture reveals metadata such as:

```text
TLS Version Information
Handshake Messages
Cipher Suite
TLS Extensions
Packet Direction
Encrypted Application Data
```

This shows that encryption protects the contents of communication while certain protocol metadata remains observable.

From a packet-analysis perspective, this metadata can help identify the protocol, understand connection behavior, and distinguish handshake traffic from encrypted application-data traffic.

---

## 14. Key Learning

This practical helped me understand HTTPS communication at the TLS layer.

I learned how to:

- Identify TLS traffic using Wireshark
- Identify a Client Hello packet
- Identify a Server Hello packet
- Examine TLS handshake information
- Identify the selected cipher suite
- Examine TLS extensions
- Identify Change Cipher Spec messages
- Identify encrypted application-data packets
- Understand the relationship between HTTP, HTTPS, TLS, and TCP
- Understand the purpose of TCP port 443
- Understand why TLS metadata remains visible even when application data is encrypted

The main communication flow observed was:

```text
TCP Connection
      |
      v
Client Hello
      |
      v
Server Hello
      |
      v
TLS Handshake
      |
      v
Encrypted Application Data
```

---

## 15. Screenshots

### Screenshot 1 — TLS Packet Overview

```text
screenshots/01-tls-packets.png
```

Shows:

- TLS packet list
- Client Hello
- Server Hello
- Change Cipher Spec
- Application Data
- TLSv1.2 and TLSv1.3 labels displayed by Wireshark

### Screenshot 2 — Client Hello

```text
screenshots/02-client-hello.png
```

Shows:

- Client Hello
- TLS handshake information
- Cipher suite information
- TLS extensions
- Supported versions
- Key-share information

## Screenshot 3 — Server Hello and Encrypted Application Data

```text
screenshots/03-server-hello.png
```

Shows:

- Server Hello
- Selected cipher suite
- Supported versions extension
- Key-share extension
- Change Cipher Spec
- Encrypted Application Data

Sensitive IP and MAC address information has been redacted before publication.

---

## 16. Conclusion

This exercise helped me move from TCP packet analysis to analyzing secure web communication at the TLS layer.

I observed the Client Hello and Server Hello messages, examined TLS negotiation information, identified the selected cipher suite, examined TLS extensions, and observed encrypted application data.

The practical demonstrated how TLS establishes security parameters for HTTPS communication and protects application data from being transmitted as readable plaintext.
