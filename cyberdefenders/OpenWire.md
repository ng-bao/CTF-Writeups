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
First we can see a machine `146.190.21.92` set up a connection with machine `134.209.197.3`, then 
<img width="853" height="89" alt="image" src="https://github.com/user-attachments/assets/bf4ee3d2-c3e9-438e-b267-020c2d4f9c40" />
