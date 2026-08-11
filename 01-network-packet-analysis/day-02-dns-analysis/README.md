# Day 02 — DNS Packet Analysis with Wireshark

## Objective

To understand DNS communication at the packet level and analyze DNS query and response packets using Wireshark.

## Traffic Generation

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

##Key Observations

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

##The screenshots show:

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
Add Day 2 DNS packet analysis

After you've uploaded/committed this README.md, don't create anything else yet.

Then we'll do 2️⃣ analysis.md separately.

Today 6:49 PM
analysis .md

Yes. Now create analysis.md inside your day-02-dns-analysis folder and copy this:

# DNS Packet Analysis

## 1. Traffic Generation

I generated DNS traffic using:

```bash
nslookup google.com 8.8.8.8

The traffic was captured using Wireshark.

I applied the display filter:

dns

This allowed me to focus on DNS query and response packets.
2. DNS Query Analysis

The DNS query is sent from my system to the DNS resolver.
UDP Information

    Source Port: 41011

    Destination Port: 53

    Protocol: UDP

The client uses an ephemeral source port (41011) and sends the DNS query to destination port 53.

Port 53 is the standard port used for DNS communication.
DNS Information

    Message Type: Query

    Transaction ID: 0x743b

    Questions: 1

    Answer RRs: 0

    Authority RRs: 0

    Additional RRs: 0

The query contains one question and does not contain answer records because the client is requesting DNS information.
3. DNS Response Analysis

The DNS resolver sends a response back to my system.
UDP Information

    Source Port: 53

    Destination Port: 41011

    Protocol: UDP

The source and destination ports are reversed compared with the DNS query.
DNS Information

    Message Type: Response

    Transaction ID: 0x743b

    Questions: 1

    Answer RRs: 1

    Authority RRs: 4

    Additional RRs: 0

    Response Status: No error

The response contains one answer record and four authority records.

The No error status indicates that the DNS resolver successfully processed the query.
4. Query and Response Relationship

The DNS query and response can be associated using the transaction ID.

DNS Query
Transaction ID: 0x743b
        ↓
DNS Response
Transaction ID: 0x743b

The UDP ports are also reversed:

Query:
41011 → 53

Response:
53 → 41011

This allows the DNS client to associate the response with the corresponding query.
5. Packet Encapsulation

The DNS packet can be viewed through the following protocol layers:

Ethernet II
     ↓
IPv4
     ↓
UDP
     ↓
DNS

Ethernet II

Provides Layer 2 information such as source and destination MAC addresses.
IPv4

Provides Layer 3 information such as source and destination IP addresses.
UDP

Provides transport-layer communication using source and destination ports.
DNS

Contains the DNS query or response information.
6. DNS Query vs Response
Property	DNS Query	DNS Response
Message Type	Query	Response
Protocol	UDP	UDP
Source Port	41011	53
Destination Port	53	41011
Transaction ID	0x743b	0x743b
Questions	1	1
Answer RRs	0	1
Authority RRs	0	4
Additional RRs	0	0
Status	—	No error
7. Investigation Thinking

If a DNS query is sent but no response is received, I should not immediately conclude that the domain or DNS server is unavailable.

Possible areas to investigate include:

    Network connectivity

    DNS resolver availability

    DNS configuration

    Firewall filtering

    UDP port 53 communication

    Local network or interface problems

Packet analysis can help identify where the communication is failing.
8. Key Learning

This practical helped me understand DNS communication at the packet level.

I learned how to identify:

    DNS queries and responses

    UDP source and destination ports

    DNS transaction IDs

    Questions and answer records

    DNS response status

    The relationship between a DNS query and its response

I also learned how DNS fits into the protocol stack:

Ethernet II → IPv4 → UDP → DNS

This helped me move from simply using nslookup to understanding the actual network communication generated by the command.
9. Evidence

The screenshots included with this analysis show:

    DNS Query packet

    DNS Response packet

    UDP source and destination ports

    DNS transaction ID

    DNS response status

    Query and response relationship

Sensitive IP and MAC address information has been redacted before publication.


For this file, use the commit message:

```text
Add detailed DNS packet analysis

After this, your Day 2 documentation is complete. Then we can upload the two DNS screenshots into the screenshots folder.

Today 6:55 PM

Day 01 — ICMP Packet Analysis with Wireshark
Objective

To understand ICMP communication at the packet level and analyze ICMP Echo Request and Echo Reply packets using Wireshark.
Environment

Operating System: Ubuntu Linux
Tool: Wireshark
Network Interface: wlp2s0

Practical Task

I generated controlled ICMP traffic using:

ping -c 4 8.8.8.8

I then captured the traffic using Wireshark and filtered the packets using:

icmp

Concepts Learned

Ethernet II
MAC addresses
IPv4
Source and destination IP addresses
ICMP
Echo Request
Echo Reply
ICMP Type and Code
Sequence numbers
Packet encapsulation

Packet Structure

Ethernet II
↓
IPv4
↓
ICMP

Key Observations

ICMP Echo Request uses Type 8.
ICMP Echo Reply uses Type 0.
Source and destination IP addresses reverse between the request and reply.
Sequence numbers help identify individual ICMP requests and corresponding replies.
Ethernet provides Layer 2 addressing information.
IPv4 provides Layer 3 addressing information.
ICMP provides information about the ICMP message.

Investigation Lesson

A packet capture can contain traffic beyond the traffic intentionally generated by the analyst.

Therefore, packet captures from real networks should be reviewed for sensitive information before being shared publicly.
Conclusion

This exercise helped me move from simply using the ping command to understanding the packet-level communication behind it.

I also learned how Wireshark can be used to inspect network traffic and investigate communication using packet-level evidence. give the readme for dns also like this

Yes. If you want Day 2 README to follow exactly the same simple style and structure as your Day 1 README, use this:

# Day 02 — DNS Packet Analysis with Wireshark

## Objective

To understand DNS communication at the packet level and analyze DNS Query and DNS Response packets using Wireshark.

## Environment

```text
Operating System: Ubuntu Linux
Tool: Wireshark
Network Interface: wlp2s0

Practical Task

I generated controlled DNS traffic using:

nslookup google.com 8.8.8.8

I then captured the traffic using Wireshark and filtered the packets using:

dns

Concepts Learned

DNS
Domain names
DNS Resolver
DNS Query
DNS Response
UDP
Source and destination ports
DNS port 53
Transaction ID
Questions and Answer Records
Packet encapsulation

Packet Structure

Ethernet II
↓
IPv4
↓
UDP
↓
DNS

Key Observations

DNS communication was observed over UDP.
The DNS Query used destination port 53.
The DNS Response used source port 53.
The query used source port 41011.
The response was sent to destination port 41011.
The same transaction ID 0x743b was observed in the query and response.
The DNS Response contained 1 answer record.
The DNS Response contained 4 authority records.
The response status was "No error".

Investigation Lesson

DNS analysis showed how a domain name query results in a DNS response from a resolver.

The transaction ID and UDP port information can be used to associate a DNS response with its corresponding query.

Packet captures also contain addressing information such as IP and MAC addresses, so sensitive information should be reviewed and redacted before sharing captures or screenshots publicly.
Conclusion

This exercise helped me move from simply using the nslookup command to understanding the DNS communication generated by it.

I learned how to identify DNS queries and responses in Wireshark and understand how DNS operates over the network protocol stack.


This is a much better match for your **Day 1 README style**: concise, practical, and focused on what **you actually did and learned**.




