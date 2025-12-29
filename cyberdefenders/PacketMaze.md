# PacketMaze 
<img width="1072" height="228" alt="image" src="https://github.com/user-attachments/assets/c4cea25e-60d8-4b3b-b3ed-e41573cf6825" />

## Solve
Q1: What is the FTP password?

I applied a filter for the FTP protocol. At we can see, the ip 192.168.1.26 is the attacker's machine. The attacker is trying to connect to FTP service. By analyze internet traffic, we can see the attacker used username: kali and password: AfricaCTF2021 to login this.
<img width="1215" height="186" alt="image" src="https://github.com/user-attachments/assets/09391125-1eac-46b7-b623-96134d96e9fa" />

> AfricaCTF2021
---
Q2: What is the IPv6 address of the DNS server used by 192.168.1.26?

I applied a filter for the DNS protocol. We can see client 192.168.1.26 communicating with host 192.168.1.20. Starting from packet 474, the two machines switch to communicating over IPv6., so I checked the MAC address of host 192.168.1.20 to find its IPv6 address.

> fe80::c80b:adff:feaa:1db7
