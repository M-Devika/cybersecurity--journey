
# HTTPS/TLS Packet Analysis

## 1. Traffic Generation

I generated controlled HTTPS traffic using:

```bash
curl https://example.com
````

The traffic was captured using Wireshark.

I applied the display filter:

```
tls
```

This allowed me to focus on TLS traffic associated with the HTTPS connection.

---

## 2. Objective

The objective of this practical was to analyze HTTPS communication at the packet level and understand how TLS establishes a secure connection and protects application data.

The analysis focused on:

-  TLS packet identification 
-  Client Hello 
-  Server Hello 
-  TLS handshake information 
-  Cipher suites 
-  TLS extensions 
-  Encrypted Application Data 
-  TCP port 443 
-  TLS packet structure 

---

## 3. TLS Packet Overview

The first screenshot shows the TLS packets captured in Wireshark.

The packet list contains entries identified as:

```
```

```
TLSv1.2  Application Data
TLSv1.3  Client Hello
TLSv1.3  Server Hello
TLSv1.3  Change Cipher Spec
TLSv1.3  Application Data
```

The overall capture contains packets identified by Wireshark as both TLS 1.2 and TLS 1.3 traffic. The HTTPS handshake analyzed in the screenshots is a TLS 1.3 handshake.

### Screenshot

```
```

```
screenshots/01-tls-packets.png
```

---

## 4. Client Hello Analysis

The second screenshot shows the **Client Hello** packet.

Wireshark identifies the packet as:

```
```

```
TLSv1.3 Record Layer: Handshake Protocol: Client Hello
```

Important observations include:

```
```

```
Handshake Type: Client Hello
Version: TLS 1.2 (0x0303)
Cipher Suites: 62
Compression Methods: 1
Extensions Length: 373
```

The Client Hello also contains several TLS extensions, including:

```
```

```
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

The Client Hello is used by the client to provide information needed for TLS negotiation, including supported versions, cipher suites, and key-exchange information.

### Important Observation About the Version Field

Although Wireshark displays:

```
```

```
TLSv1.3 Record Layer
```

the Client Hello contains:

```
```

```
Version: TLS 1.2 (0x0303)
```

This does **not** necessarily mean that the connection is using TLS 1.2.

TLS 1.3 uses `0x0303` as a legacy version value in parts of the handshake for compatibility. The actual supported TLS versions are negotiated using the `supported_versions` extension.

Therefore, the packet should be interpreted as part of a **TLS 1.3 handshake**, rather than concluding that it is a TLS 1.2 connection solely from the `0x0303` field.

### Screenshot

```
```

```
screenshots/02-client-hello.png
```

---

## 5. Server Hello Analysis

The third screenshot shows the **Server Hello** packet.

Wireshark identifies the packet as:

```
```

```
TLSv1.3 Record Layer: Handshake Protocol: Server Hello
```

Important observations include:

```
```

```
Handshake Type: Server Hello
Version: TLS 1.2 (0x0303)
Cipher Suite: TLS_AES_256_GCM_SHA384
Compression Method: null (0)
```

The Server Hello also contains extensions including:

```
```

```
key_share
supported_versions
```

The Server Hello represents the server's response to the Client Hello and contains the parameters selected for the TLS connection.

The selected cipher suite shown in the screenshot is:

```
```

```
TLS_AES_256_GCM_SHA384
```

Similar to the Client Hello, the `0x0303` version field in the TLS 1.3 Server Hello is a legacy compatibility value. The negotiated TLS version is indicated through the `supported_versions` extension.

### Screenshot

```
```

```
screenshots/03-server-hello.png
```

---

## 6. Encrypted Application Data

The Server Hello screenshot also shows:

```
```

```
TLSv1.3 Record Layer: Application Data Protocol: http-over-tls
```

The packet contains:

```
```

```
Content Type: Application Data (23)
Version: TLS 1.2 (0x0303)
Encrypted Application Data
```

The application data is encrypted, so the actual HTTP content is not directly visible in the packet details.

This demonstrates the purpose of TLS encryption:

```
```

```
HTTP Application Data
        ↓
       TLS
        ↓
Encrypted Application Data
```

The packet capture therefore allows the analyst to observe TLS metadata and encrypted traffic without exposing the protected application content.

---

## 7. Change Cipher Spec

The Server Hello screenshot also shows:

```
```

```
TLSv1.3 Record Layer: Change Cipher Spec Protocol
```

The presence of a Change Cipher Spec message in a TLS 1.3 connection can occur for compatibility purposes.

It should not be interpreted as meaning that the connection has switched to TLS 1.2.

The important evidence for this analysis is that the capture contains TLS 1.3 handshake traffic followed by encrypted application data.

---

## 8. TLS Cipher Suite

The Server Hello packet shows the selected cipher suite:

```
```

```
TLS_AES_256_GCM_SHA384
```

This cipher suite indicates the cryptographic algorithms selected for the TLS 1.3 connection.

It uses:

- **AES-256-GCM** for authenticated encryption 
- **SHA-384** as the hash function associated with the cipher suite 

The cipher suite selection is negotiated during the TLS handshake.

---

## 9. TLS Extensions

The Client Hello contains several extensions that provide additional information for TLS negotiation.

Important extensions visible in the screenshot include:

```
```

```
server_name
application_layer_protocol_negotiation
supported_versions
signature_algorithms
key_share
supported_groups
```

These extensions allow the client and server to negotiate capabilities and parameters required for the secure connection.

For example:

```
```

```
server_name
→ Identifies the requested server name.

supported_versions
→ Indicates supported TLS versions.

signature_algorithms
→ Indicates supported signature algorithms.

key_share
→ Provides key-exchange information.
```

---

## 10. HTTPS and TLS Relationship

HTTPS is HTTP communication protected using TLS.

The protocol relationship can be represented as:

```
```

```
HTTP
↓
TLS
↓
TCP
↓
IPv4
↓
Ethernet II
```

The standard server-side port for HTTPS is:

```
```

```
443
```

The TLS layer provides protection for the HTTP application data transported through the connection.

---

## 11. Packet Encapsulation

The HTTPS communication can be viewed through the following protocol stack.

### Packet Dissection Order

```
```

```
Ethernet II
↓
IPv4
↓
TCP
↓
TLS
↓
HTTP
```

### Ethernet II

Provides Layer 2 addressing information such as source and destination MAC addresses.

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

```
```

```
tls
```

This was chosen because the objective was to analyze the TLS layer of HTTPS communication.

Other filters can be used for different levels of analysis:

```
```

```
tcp
```

Shows TCP traffic.

```
```

```
tcp.port == 443
```

Shows TCP traffic associated with port 443.

```
```

```
tls
```

Focuses on traffic identified by Wireshark as TLS.

For this practical, the `tls` filter made it easier to identify:

```
```

```
Client Hello
Server Hello
Change Cipher Spec
Application Data
```

---

## 13. Investigation Thinking

The packet capture demonstrates that encrypted communication can still provide useful information to a network analyst.

Even though the application data is encrypted, the capture reveals metadata such as:

```
```

```
TLS Version Information
Handshake Messages
Cipher Suite
TLS Extensions
Packet Direction
Encrypted Application Data
TCP Port
```

This shows that encryption does not make network traffic invisible. Instead, it protects the contents of the communication while leaving certain protocol metadata observable.

---

## 14. Key Learning

This practical helped me understand HTTPS communication at the TLS layer.

I learned how to:

-  Identify TLS traffic using Wireshark 
-  Identify a Client Hello packet 
-  Identify a Server Hello packet 
-  Examine TLS handshake information 
-  Identify cipher-suite information 
-  Examine TLS extensions 
-  Identify encrypted application-data packets 
-  Understand the relationship between HTTP, HTTPS, TLS, and TCP 
-  Understand the purpose of TCP port 443 
-  Understand why TLS packet metadata remains visible even when application data is encrypted 

The main communication flow observed was:

```
```

```
TCP Connection
      ↓
Client Hello
      ↓
Server Hello
      ↓
TLS Handshake
      ↓
Encrypted Application Data
```

---

## 15. Evidence

The screenshots included with this analysis are:

### Screenshot 1 — TLS Packet Overview

```
```

```
screenshots/01-tls-packets.png
```

Shows:

-  TLS packet list 
-  Client Hello 
-  Server Hello 
-  Change Cipher Spec 
-  Application Data 
-  TLSv1.2 and TLSv1.3 labels displayed by Wireshark 

### Screenshot 2 — Client Hello

```
```

```
screenshots/02-client-hello.png
```

Shows:

-  Client Hello 
-  TLS handshake information 
-  Cipher suites 
-  TLS extensions 
-  Supported versions 
-  Key-share information 

### Screenshot 3 — Server Hello and Encrypted Data

```
```

```
screenshots/03-server-hello.png
```

Shows:

-  Server Hello 
-  Selected cipher suite 
-  Supported versions 
-  Key-share information 
-  Change Cipher Spec 
-  Encrypted Application Data 

Sensitive IP and MAC address information has been redacted before publication.

---

## 16. Conclusion

This exercise helped me move from TCP packet analysis to analyzing secure web communication at the TLS layer.

I observed the Client Hello and Server Hello messages, examined TLS negotiation information, identified the selected cipher suite, and observed encrypted application data.

The practical demonstrated how TLS establishes the security parameters for HTTPS communication and protects application data from being transmitted as readable plaintext.

```
```

````

### One thing to check before committing

Make sure your actual screenshot filenames exactly match:

```text
day-04-https-analysis/
├── README.md
├── analysis.md
└── screenshots/
    ├── 01-tls-packets.png
    ├── 02-client-hello.png
    └── 03-server-hello.png
````

If your filenames are different, **change only those three paths in** **`analysis.md`** is this ok
