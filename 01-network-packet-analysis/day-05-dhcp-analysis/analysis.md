

# DHCP Packet Analysis

## 1. Traffic Capture

DHCP traffic was captured using Wireshark on the `wlp2s0` network interface.

The following display filter was used:

```text
dhcp

The filter was used to isolate DHCP packets from other network traffic.

2. Objective

The objective of this analysis was to understand DHCP communication at the packet level and examine the DHCP packets observed during the capture.

The analysis focused on:

DHCP message types
DHCP Request
DHCP ACK
DHCP client-server communication
DHCP packet filtering
The relationship between the observed packets and the normal DHCP process
3. Expected DHCP Process

A typical DHCP address-assignment process follows the DORA sequence:

DHCP Discover
      |
      v
DHCP Offer
      |
      v
DHCP Request
      |
      v
DHCP ACK

The four stages are:

DHCP Discover — The client searches for available DHCP servers.
DHCP Offer — A DHCP server offers network configuration to the client.
DHCP Request — The client requests the offered configuration.
DHCP ACK — The server acknowledges the request and confirms the configuration.
4. Packets Observed

The actual capture did not contain the complete DORA sequence.

The packets observed were:

DHCP Request
      |
      v
DHCP ACK

The DHCP Discover and DHCP Offer packets were not present in the capture.

Therefore, this analysis focuses only on the packets that were actually observed.

5. DHCP Request Analysis

The DHCP Request packet represents communication from the DHCP client to the DHCP server.

A DHCP Request can be used by a client to request or confirm network configuration.

The DHCP Request packet can contain information such as:

DHCP message type
Transaction ID
Client identifier
Requested IP address
DHCP server identifier
Other DHCP options

The observed Request packet demonstrates that the client was communicating with the DHCP service as part of the DHCP lease process.

Evidence
screenshots/01-dhcp-request.png
6. DHCP ACK Analysis

The DHCP ACK packet represents an acknowledgment from the DHCP server.

The server uses the ACK message to confirm the client's DHCP request and provide or confirm network configuration.

A DHCP ACK can contain configuration information such as:

Assigned IP address
Subnet mask
Default gateway
DNS server
Lease duration

The observed ACK packet demonstrates server-side confirmation of the DHCP request.

Evidence
screenshots/02-dhcp-ack.png
7. Why Discover and Offer Were Not Observed

The complete DORA sequence was not captured during this practical.

The most likely reason is that the network interface already had an active DHCP configuration when packet capture was performed. DHCP Discover and Offer occur during the initial address-acquisition process, so they may not appear in a capture that begins after that exchange has already occurred.

The important point is that the missing packets were not treated as captured evidence.

The analysis therefore distinguishes between:

Expected
```text
Discover
   |
   v
Offer
   |
   v
Request
   |
   v
ACK
Actually observed
Request
   |
   v
ACK ```
8. DHCP Packet Filtering

The Wireshark display filter used was:

dhcp

This filter displays packets identified by Wireshark as DHCP traffic.

Filtering is useful because a network interface can capture many different protocols simultaneously.

For example:

dhcp

Focuses on DHCP traffic.

A more specific filter can also be used when investigating particular DHCP message types.

9. Investigation Thinking

The capture provided an important practical lesson about packet analysis.

A protocol diagram describes the expected communication sequence, but a live packet capture only contains packets that actually occurred during the capture period.

In this practical:

Expected:
Discover → Offer → Request → ACK

Observed:
Request → ACK

Therefore, the analysis was based on the available packet evidence rather than assuming that the complete DORA exchange had been captured.

This is important in real network analysis because an analyst must distinguish between:

What is expected to happen
What actually happened
What was captured
What cannot be confirmed from the available evidence
10. Key Observations

The main observations from the capture were:

DHCP traffic was successfully identified using Wireshark.
A DHCP Request packet was observed.
A DHCP ACK packet was observed.
The complete DORA sequence was not present.
DHCP Request represents client-side DHCP communication.
DHCP ACK represents server-side acknowledgment.
Packet captures depend on the network state and the time at which the capture begins.
11. Key Learning

Through this practical, I learned:

The purpose of DHCP.
The DHCP DORA process.
How DHCP traffic can be identified in Wireshark.
The purpose of a DHCP Request.
The purpose of a DHCP ACK.
How DHCP client-server communication appears in packet captures.
Why a live capture may not contain the complete protocol exchange.
The importance of distinguishing observed evidence from expected protocol behavior.
12. Evidence

The screenshots used for this analysis show the DHCP packets that were actually observed during the capture.

Screenshot 1 — DHCP Request
screenshots/01-dhcp-request.png

Shows the DHCP Request packet and its packet details in Wireshark.

Screenshot 2 — DHCP ACK
screenshots/02-dhcp-ack.png

Shows the DHCP ACK packet and its packet details in Wireshark.

Sensitive network information has been redacted before publication.

13. Conclusion

This practical provided hands-on experience analyzing DHCP traffic using Wireshark.

The capture did not contain the complete DHCP DORA sequence. However, the DHCP Request and DHCP ACK packets were successfully observed and analyzed.

Rather than assuming that the missing Discover and Offer packets were captured, this analysis documents only the traffic that was actually observed.

This reinforced an important network-analysis principle:

Packet analysis should be based on captured evidence rather than assumptions about what should have appeared.


