# HoneyBOT
<img width="1086" height="381" alt="image" src="https://github.com/user-attachments/assets/39353604-58c3-4c1a-815d-49adef82f18d" />

## What is HoneyBot
A honeypot is a decoy system or bait designed to trick a hacker into infiltrating a system so that security professionals can observe and study their behavior. 

## solve
Q1: What is the attacker's IP address?

By analyzing internet traffic, we can see the machine 98.114.205.102 is establishing a connection to machine 192.150.11.111
> 98.114.205.102

---
Q2: What is the target's IP address?
based on question 1, the target's IP address is 192.150.11.111 and this also is HoneyBot's IP address.
> 192.150.11.111

---
Q3: Provide the country code for the attacker's IP address (a.k.a geo-location).

Just use the ip-lookup tool via followìng the below link to find the answer.
`cmd
https://whatismyipaddress.com/ip-lookup
`
<img width="1037" height="623" alt="image" src="https://github.com/user-attachments/assets/c2282125-0a03-4528-8b2d-4b87b5bb4b1c" />

> US

---
Q4: How many TCP sessions are present in the captured traffic?

Conversations from the Statistics menu gives us this:
<img width="1662" height="217" alt="image" src="https://github.com/user-attachments/assets/eda98fc8-6739-43da-9c33-bc7d849378f0" />

> 5
---
Q5: How long did it take to perform the attack (in seconds)?

Use Second Since First Captured Packet format in view menu and see the final packet.
<img width="1268" height="34" alt="image" src="https://github.com/user-attachments/assets/0f2769d5-d990-48de-a86b-7a9a128a240f" />

> 16
---
Q6: 

---
Q7: Which protocol was used to carry over the exploit?

By analyze the internet traffic, we can see the attacker established a connect to victim machine(HoneyBot) via SMB protocol.
<img width="1918" height="284" alt="image" src="https://github.com/user-attachments/assets/1d8c4068-bfad-4b1d-b600-8ff1f64f3ab3" />

> SMB
---
Q8: Which protocol did the attacker use to download additional malicious files to the target system?

By using Zui, this tool is detected a suspicious file like this.
<img width="1289" height="394" alt="image" src="https://github.com/user-attachments/assets/a39e8071-549a-40dc-9ec9-4cad8132247e" />

This is a PE(Portable Executable) is sended via FTP-data. Determined by `source: "FTP_DATA"`.
> FTP
---
Q9: What is the name of the downloaded malware?

We knew that the attacker is downloaded a malicious file via FTP protocol. So I checked `_path: ftp` and saw `arg: "ftp://98.114.205.102/ssms.exe"`. This mean the machine 98.114.205.102(attacker) downloaded a file name ssms.exe. A executable file which matched the clue we found in question 8.
> ssms.exe
