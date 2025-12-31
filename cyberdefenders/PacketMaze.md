# PacketMaze 
<img width="1072" height="228" alt="image" src="https://github.com/user-attachments/assets/c4cea25e-60d8-4b3b-b3ed-e41573cf6825" />

## Solve
Q1: What is the FTP password?

I applied a filter for the FTP protocol. At we can see, the ip 192.168.1.26 is the attacker's machine. The attacker is trying to connect to FTP service. By analyze internet traffic, we can see the attacker used username: kali and password: AfricaCTF2021 to login this.
<img width="1215" height="186" alt="image" src="https://github.com/user-attachments/assets/09391125-1eac-46b7-b623-96134d96e9fa" />

> AfricaCTF2021
---
Q2: What is the IPv6 address of the DNS server used by 192.168.1.26?

I applied a filter for the DNS protocol. We can see client 192.168.1.26 communicating with host 192.168.1.20. Starting from packet 474, the two machines switch to communicating over IPv6., so I cross-referenced the host's MAC address to identify its corresponding IPv6 address..

> fe80::c80b:adff:feaa:1db7
---
Q3: What domain is the user looking up in packet 15174?

Looking at the tag queries, we could see the domain name.

<img width="388" height="141" alt="image" src="https://github.com/user-attachments/assets/91cea6d5-e5ab-42e0-9104-6494f72ecf40" />

> www.7-zip.org
---
Q4: How many UDP packets were sent from 192.168.1.26 to 24.39.217.246?

I applied the following filter: `udp && ip.src == 192.168.1.26 && ip.dst == 24.39.217.246` and counted the display packets.
<img width="1067" height="281" alt="image" src="https://github.com/user-attachments/assets/a0832e9a-be36-405f-a0c2-ed9e62eb86a5" />

> 10
---
Q5: What is the MAC address of the system under investigation in the PCAP file?

We have identified the host under investigation as 192.168.1.26 in question 1. Therefore, we can easily check its MAC address in any packet where the source or destination is 192.168.1.26

<img width="1076" height="137" alt="image" src="https://github.com/user-attachments/assets/f9909751-bbad-453a-9bb0-60dcdeb89296" />

in this packet, the host 192.168.1.26 is destination.
> c8:09:a8:57:47:93
---
Q6: What was the camera model name used to take picture 20210429_152157.jpg?

Just download this picture and use ExifTool to check the metadata to get the answer.
<img width="534" height="30" alt="image" src="https://github.com/user-attachments/assets/1c3f8c25-6466-445c-ab8c-81f3126cc585" />

> LM-Q725K
---
Q7: What is the ephemeral public key provided by the server during the TLS handshake in the session with the session ID: ?da4a0000342e4b73459d7360b4bea971cc303ac18d29b99067e46d16cc07f4ff

I found the packet containing this session ID by using the filter: 'tls.handshake.session_id == da4a0000342e4b73459d7360b4bea971cc303ac18d29b99067e46d16cc07f4ff' and located the public key inside.
> 04edcc123af7b13e90ce101a31c2f996f471a7c8f48a1b81d765085f548059a550f3f4f62ca1f0e8f74d727053074a37bceb2cbdc7ce2a8994dcd76dd6834eefc5438c3b6da929321f3a1366bd14c877cc83e5d0731b7f80a6b80916efd4a23a4d
---
Q8:What is the first TLS 1.3 client random that was used to establish a connection with protonmail.com?

Based on keywords TLS 1.3 and protonmail.com, I applied the following filter `tls && frame contains "protonmail.com"`.
<img width="1281" height="231" alt="image" src="https://github.com/user-attachments/assets/58891839-8d28-4471-a370-b71e77d230f1" />

As we can see, the first TLS 1.3 packet that was used to establish a connection with protonmail.com is packet 17992. Then, I checked the random value to get the answer.
> 24e92513b97a0348f733d16996929a79be21b0b1400cd7e2862a732ce7775b70
---
Q9: Which country is the manufacturer of the FTP server’s MAC address registered in?

We can easily identify the MAC address of FTP server as 08:00:27:a6:1f:86. Then, I used the MAC Lookup tool to find the answer.
<img width="656" height="157" alt="image" src="https://github.com/user-attachments/assets/37330ebc-b3cc-4abf-9802-9df5d3fa0af8" />

> United States
---
Q10: What time was a non-standard folder created on the FTP server on the 20th of April?
