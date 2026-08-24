# HTTPS/TLS Packet Analysis

## 1. Traffic Generation

I generated controlled HTTPS traffic using:

```bash
curl https://example.com
```

The traffic was captured using Wireshark.

I applied the display filter:

```text
tls
```

This allowed me to focus on TLS traffic associated with the HTTPS connection.

---

## 2. Objective

The objective of this practical was to analyze HTTPS communication at the packet level and understand how TLS establishes a secure connection and protects application data.

The analysis focused on:

- TLS packet identification
- Client Hello
- Server Hello
- TLS handshake information
- Cipher suites
- TLS extensions
- Encrypted Application Data
- TCP port 443
- TLS packet structure

---

## 3. TLS Packet Overview

The packet capture contains TLS traffic associated with the HTTPS connection.

The packet list contains entries identified by Wireshark as:

```text
TLSv1.2  Application Data
TLSv1.3  Client Hello
TLSv1.3  Server Hello
TLSv1.3  Change Cipher Spec
TLSv1.3  Application Data
```

The overall capture contains packets displayed by Wireshark with both TLS 1.2 and TLS 1.3 labels. The handshake analyzed in this practical is a TLS 1.3 handshake.

### Screenshot

```text
screenshots/01-tls-packets.png
```

---

## 4. Client Hello Analysis

The Client Hello packet was inspected in Wireshark.

Wireshark identifies the packet as:

```text
TLSv1.3 Record Layer: Handshake Protocol: Client Hello
```

Important observations include:

```text
Handshake Type: Client Hello
Version: TLS 1.2 (0x0303)
Cipher Suites: 62
Compression Methods: 1
Extensions Length: 373
```

The Client Hello contains several TLS extensions, including:

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

The Client Hello provides information that the server uses during TLS negotiation, including supported TLS versions, cipher suites, extensions, and key-exchange information.

### Version Field Observation

Although Wireshark displays:

```text
TLSv1.3 Record Layer
```

the Client Hello contains:

```text
Version: TLS 1.2 (0x0303)
```

This does not necessarily mean that the connection is using TLS 1.2.

In TLS 1.3, `0x0303` is retained as a legacy version value for compatibility. The actual TLS version negotiation is indicated through the `supported_versions` extension.

Therefore, the Client Hello is interpreted as part of a TLS 1.3 handshake rather than as evidence of a TLS 1.2 connection.

### Screenshot

```text
screenshots/02-client-hello.png
```

---

## 5. Server Hello Analysis

The Server Hello packet was inspected in Wireshark.

Wireshark identifies the packet as:

```text
TLSv1.3 Record Layer: Handshake Protocol: Server Hello
```

Important observations include:

```text
Handshake Type: Server Hello
Version: TLS 1.2 (0x0303)
Cipher Suite: TLS_AES_256_GCM_SHA384
Compression Method: null (0)
```

The Server Hello also contains extensions including:

```text
key_share
supported_versions
```

The Server Hello represents the server's response to the Client Hello and contains the parameters selected for the TLS connection.

The selected cipher suite shown in the capture is:

```text
TLS_AES_256_GCM_SHA384
```

As with the Client Hello, the `0x0303` version field is a legacy compatibility value in TLS 1.3. The negotiated TLS version is indicated through the `supported_versions` extension.

### Screenshot

```text
screenshots/03-server-hello.png
```

---

## 6. Encrypted Application Data

The capture also contains:

```text
TLSv1.3 Record Layer: Application Data Protocol: http-over-tls
```

The packet contains:

```text
Content Type: Application Data (23)
Version: TLS 1.2 (0x0303)
Encrypted Application Data
```

The application data is encrypted, so the actual HTTP content is not directly visible in the packet details.

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

This demonstrates how TLS protects application data while still allowing network analysts to observe certain protocol metadata.

---

## 7. Change Cipher Spec

The capture also contains:

```text
TLSv1.3 Record Layer: Change Cipher Spec Protocol
```

A Change Cipher Spec message can appear in TLS 1.3 for compatibility purposes.

Its presence does not mean that the connection has switched to TLS 1.2.

The important observation is that the capture contains TLS 1.3 handshake traffic followed by encrypted application data.

---

## 8. TLS Cipher Suite

The Server Hello packet shows the selected cipher suite:

```text
TLS_AES_256_GCM_SHA384
```

This identifies the cryptographic algorithms selected for the TLS 1.3 connection.

It uses:

- **AES-256-GCM** for authenticated encryption
- **SHA-384** as the associated hash function

The cipher suite is selected during the TLS handshake.

---

## 9. TLS Extensions

The Client Hello contains several extensions used during TLS negotiation.

Important extensions visible in the capture include:

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
→ Indicates supported TLS versions.

signature_algorithms
→ Indicates supported signature algorithms.

key_share
→ Provides key-exchange information.
```

These extensions allow the client and server to negotiate the parameters required to establish the secure connection.

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

The standard server-side port for HTTPS is:

```text
443
```

TLS provides cryptographic protection for HTTP application data transported over the TCP connection.

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

When HTTP is protected by TLS, the communication is HTTPS.

---

## 12. Why the `tls` Filter Was Used

The Wireshark display filter used for this practical was:

```text
tls
```

This filter was chosen because the objective was to analyze the TLS layer of HTTPS communication.

Other filters can be used for different levels of analysis:

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

Focuses on traffic identified by Wireshark as TLS.

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

Even though the application data is encrypted, the capture reveals metadata such as:

```text
TLS Version Information
Handshake Messages
Cipher Suite
TLS Extensions
Packet Direction
Encrypted Application Data
TCP Port
```

This shows that encryption does not make network traffic invisible. Instead, it protects the contents of the communication while certain protocol metadata remains observable.

---

## 14. Key Learning

This practical helped me understand HTTPS communication at the TLS layer.

I learned how to:

- Identify TLS traffic using Wireshark
- Identify a Client Hello packet
- Identify a Server Hello packet
- Examine TLS handshake information
- Identify cipher-suite information
- Examine TLS extensions
- Identify encrypted application-data packets
- Understand the relationship between HTTP, HTTPS, TLS, and TCP
- Understand the purpose of TCP port 443
- Understand why TLS packet metadata remains visible even when application data is encrypted

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

## 15. Evidence

The screenshots used for this analysis are:

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
- Cipher suites
- TLS extensions
- Supported versions
- Key-share information

### Screenshot 3 — Server Hello

```text
screenshots/03-server-hello.png
```

Shows:

- Server Hello
- Selected cipher suite
- Supported versions
- Key-share information
- Change Cipher Spec
- Encrypted Application Data

Sensitive IP and MAC address information has been redacted before publication.

---

## 16. Conclusion

This exercise helped me move from TCP packet analysis to analyzing secure web communication at the TLS layer.

I observed the Client Hello and Server Hello messages, examined TLS negotiation information, identified the selected cipher suite, and observed encrypted application data.

The practical demonstrated how TLS establishes the security parameters for HTTPS communication and protects application data from being transmitted as readable plaintext.
