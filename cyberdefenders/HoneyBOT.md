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
`
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
Q6: Provide the CVE number of the exploited vulnerability.

While I'm analyzingthe internet traffic, I found some suspicious packets like this.
<img width="1525" height="135" alt="image" src="https://github.com/user-attachments/assets/fdd18918-7ff1-433d-b22e-5bfb52e5fbf6" />

Focus on packet 33,  there is DsRoleUpgradeDownlevelServer was send via DSSETUP(Directory Service Setup Remote Protocol). I searched on google with this keyword.
<img width="1193" height="332" alt="image" src="https://github.com/user-attachments/assets/0c5dbced-c4f2-4827-8c6d-79b75cf260c0" />

> CVE-2003-0533
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
---
Q10: The attacker's server was listening on a specific port. Provide the port number.

<img width="831" height="460" alt="image" src="https://github.com/user-attachments/assets/c4b8c2b9-882d-440f-b0ef-69f85f45d47c" />

Look at `event_type: alert (3)`, we can see SURICATA - a high performance, open source network analysis and threat detection software, detected protocol only one direction from the attacker machine to victim machine via FTP protocol. In question 8, we knew that ftp is the protocol which the attacker was used to send malicious file. In addition, based on FTP protocol, I think the attacker set up an FTP server and listen on port 8884 and this port is the correct answer.
> 8884
---
Q11: When was the involved malware first submitted to VirusTotal for analysis? Format: YYYY-MM-DD

Return to the question 9, we found a malware name `ssms.exe`. So I tried to find md5 hash and found this:

`
md5: "14a09a48ad23fe0ea5a180bee8cb750a"
`
I checked this hash on VirusTotal and it was flagged as malicious.
<img width="1638" height="218" alt="image" src="https://github.com/user-attachments/assets/d79a2dc8-3382-4d71-ac44-a247269c2f58" />

And then, I just see history in details and get the answer.
<img width="690" height="149" alt="image" src="https://github.com/user-attachments/assets/3131d57b-c554-4a9d-a804-13911ce8866a" />

> 2007-06-27
---
Q12: What is the key used to encode the shellcode?

The key used to encode shellcode is typically a random byte chosen from a range of possible values. This random byte is XORed with the shellcode byte to produce an encoded byte. The process of encoding shellcode involves reading the shellcode in a specific format, such as \xFF\xEE\xDD, and applying the XOR operation to each byte. This method helps to obfuscate the shellcode, making it harder to detect by antivirus engines or other security software. 
By follow tcp steam at packet which call DsRoleUpgradeDownlevelServer function, I can see shellcode the attacker was inject into to exploit a vulnerability of this function. I saw many value like 0x90. In Intel x86 assembly, the hex code 0x90 represents the NOP(No Operation) instruction. I think because the attacker cannot determine the precise EIP address, they inject a NOP sled. This ensures the execution flow 'slides' into the shellcode. So when I decode hex data to assembly, I will examine the shellcode after the NOP sled.
<img width="946" height="298" alt="image" src="https://github.com/user-attachments/assets/5ac3e8a4-dfe8-4d6f-915a-2ea4f90fe091" />

in offset 49f, It performs a XOR operation with 0x99. So 0x99 is a answer for this question
> 0x99
---
Q13: What is the port number the shellcode binds to?

. And then, I used scdbg to analyze and see that:
<img width="711" height="730" alt="image" src="https://github.com/user-attachments/assets/57b450ee-1cc7-406c-9a80-3aab83eab2e9" />

As we can see, this shellcode binds to port 1957 and it is the answer.
> 1957
---
Q14: The shellcode used a specific technique to determine its location in memory. What is the OS file being queried during this process?

Checking this malicious file in VirusTotal, I observed that it imports the kernel32.dll library. I don't know what this is, then I do some research on it and know that the Kernel32.dll file is an essential component of the Windows operating system. It is a dynamic link library file that contains various functions and resources required for the proper functioning of the Windows kernel. So this is a OS file we need to find.
<img width="902" height="441" alt="image" src="https://github.com/user-attachments/assets/dbb16dd0-4e8a-4ff6-a272-d4075581f9be" />

> kernel32.dll
