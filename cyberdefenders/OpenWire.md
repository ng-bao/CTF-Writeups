# Description
During your shift as a tier-2 SOC analyst, you receive an escalation from a tier-1 analyst regarding a public-facing server. This server has been flagged for making outbound connections to multiple suspicious IPs. In response, you initiate the standard incident response protocol, which includes isolating the server from the network to prevent potential lateral movement or data exfiltration and obtaining a packet capture from the NSM utility for analysis. Your task is to analyze the pcap and assess for signs of malicious activity.

# Tools
* Wireshark
* Browser

# Overview
* This lab is talking about a critical vulnerability in Apache ActiveMQ.
* Announced on Oct. 25, the flaw scored a maximum of 10.0 on the CVSS v3 scale.

# Solve
### Question 1: By identifying the C2 IP, we can block traffic to and from this IP, helping to contain the breach and prevent further data exfiltration or command execution. Can you provide the IP of the C2 server that communicated with our server?
First we can see in packets 1-3, the 3 ways handshake from `146.190.21.92` to `134.209.197.3`. Next, we can see machine `134.209.197.3` sent a `WireFormatInfo` packet over the `OpenWire` protocol to negotiate the data transmission settings.

<img width="853" height="56" alt="image" src="https://github.com/user-attachments/assets/3162241d-c0c0-4952-a1b5-beca59c84bb7" />

And in `packet 6`, the machine `146.190.21.92` sent back a `ExceptionResponse` marking as `Malformed Packet` packet.

<img width="448" height="113" alt="image" src="https://github.com/user-attachments/assets/0f437f4b-6878-417d-ac5d-0888222d3508" />

Anwser: `146.190.21.92`

###
