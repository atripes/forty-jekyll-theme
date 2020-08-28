---
layout: post
title: Wireshark and analyzing network traffic
description: An online course on udemy, and what I learned from it.
permalink: :title
---

Kali Linux is a recommended Linux distro for network analysis.

# Wireshark

  * general network analysis
  * background (net bios, dns) traffic identifiable
  * host-wise bandwith analysis
  * analyse faulty (slow) network connections
  * detect malicious applications
  * security analysis
  * detect/identify malformed packets (packages?)

Wireshark chunks of 'packets' are called frames.

icmp = internet control message protocol

icmp message consists of the fields:
  * internet frame header 
  * ip 
  * icmp



arp = address resolution protocol
Deals with hardware addresses.

# Filter examples
## udp dst port 53 and dst host 8.8.8.8
Filter DNS queries

## arp or icmp


IP communication is end to end
ARP, or MAC address communication is only done to your next routing node.

rfc791 = Internet Protocol



ISO OSI modell layer 4 holds tcp/udp and relies upon the layer 3 (network)

DNS is the highest protocol appearing in layer 4 (iso osi).


# TCP Stack

1. SYN
2. SYN, ACK
3. ACK
4. Payload, WindowSize (how long until ACK)
5. ACK
6. FIN, ACK
7. FIN, ACK
8. FIN
