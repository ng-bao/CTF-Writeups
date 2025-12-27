# BlueSky Ransomware 
<img width="1074" height="296" alt="image" src="https://github.com/user-attachments/assets/87b616fd-0f74-40bb-8c4e-ef03ffa720fc" />

## Solve
Q1: Knowing the source IP of the attack allows security teams to respond to potential threats quickly. Can you identify the source IP responsible for potential port scanning activity?

I researched port scanning and found the following results.
<img width="992" height="651" alt="image" src="https://github.com/user-attachments/assets/32af942e-cffb-49d5-abd4-412fa58d11c8" />

Focus on port scanning type 2, it has some keywords like [SYN], [ACK, SYN], [ACK]. They resemble the packets we captured.
<img width="1890" height="514" alt="image" src="https://github.com/user-attachments/assets/493e26da-e377-46ce-9f0e-b53c643e27ba" />

As we can see, host 87.96.21.84 continuously sent [SYN] packets to various ports on 87.96.21.81. So host 87.96.21.84 is source IP responsible for potential port scanning activity
> 87.96.21.84

---
Q2: During the investigation, it's essential to determine the account targeted by the attacker. Can you identify the targeted account username?

<img width="1698" height="173" alt="image" src="https://github.com/user-attachments/assets/efccb236-b103-4bc2-88ba-185711d048ae" />


We could clearly see that host 87.96.21.84 (attacker) tried connect over port 1433 (SQL Server default) and initiate communication. So I checked in event log and saw that
<img width="1273" height="326" alt="image" src="https://github.com/user-attachments/assets/d6649c5f-e572-4ea1-8200-d7ad277e2e79" />

There are many events from MSSQLSERVER .The event ID 18454 means login successfull so I checked in Eventdata and found account username.
<img width="1209" height="185" alt="image" src="https://github.com/user-attachments/assets/a9fd3109-d556-41a3-98e8-487b0779741e" />

> sa

---
Q3: We need to determine if the attacker succeeded in gaining access. Can you provide the correct password discovered by the attacker?

I saw the attacker communicating with the SQL Server via TDS. So I filter TDS for search convinient. We could clearly see that many burst of packets like this sent to SQL server.
<img width="1781" height="122" alt="image" src="https://github.com/user-attachments/assets/4b38e597-619d-43b1-8fe7-23da65252015" />

I think it was a brute force attack, I tried to find the packet which contain correct password and find this.
<img width="1211" height="130" alt="image" src="https://github.com/user-attachments/assets/284a012d-3f9c-4185-8b5a-c83a65c414d0" />

Look insigh packet 2641 and we can find the password
<img width="992" height="438" alt="image" src="https://github.com/user-attachments/assets/68094760-8989-4b50-89c8-8d1d4d203175" />
> cyb3rd3f3nd3r$

---
Q4: Attackers often change some settings to facilitate lateral movement within a network. What setting did the attacker enable to control the target host further and execute further commands?

After login into SQL server, attacker run this command like this
<img width="1022" height="444" alt="image" src="https://github.com/user-attachments/assets/1c348a29-c6d0-4e89-99f4-f59cf9570d26" />

It sets the configuration value of xp_cmdshell to 1 which allow attacker to run any CMD command with the privileges of the SQL Service account.
> xp_cmdshell

---
Q5: Process injection is often used by attackers to escalate privileges within a system. What process did the attacker inject the C2 into to gain administrative privileges?

When check in Event Viewer, we could see multiple PowerShell processes like this
<img width="1266" height="415" alt="image" src="https://github.com/user-attachments/assets/a9ae65b9-ab1c-40a0-9adb-3017af07189e" />

Look at the HostName: MSFConsole. This stands for MetaSploit Framework Console. The attacker is controlling this computer via a session in Metasploit—the most popular hacking toolkit used for penetration testing and malware deployment. Its also have the same HostApplication: winlogon.exe. This mean attacker injected the malicious payload directly into the critical system process: winlogon.exe because it has the highest level of authority so it impossible to kill via Task Manager or trigger Antivirus.
> winlogon.exe

---
Q6: Following privilege escalation, the attacker attempted to download a file. Can you identify the URL of this file downloaded?
 
I applied filter for http and focus on GET method. And the first file that the attacker dowloaded was checking.ps1 via URL: http://87.96.21.84/checking.ps1
<img width="1199" height="34" alt="image" src="https://github.com/user-attachments/assets/eb62e076-3fbc-4c2f-a0e8-855d85c4ffa3" />

> http://87.96.21.84/checking.ps1

---
Q7: Understanding which group Security Identifier (SID) the malicious script checks to verify the current user's privileges can provide insights into the attacker's intentions. Can you provide the specific Group SID that is being checked?

<img width="984" height="48" alt="image" src="https://github.com/user-attachments/assets/4cd947a1-4670-48d3-8b17-3a8fe9073fa0" />

Following the picture and Q6, the attacker downloaded a file named checking.ps1 so I used follow HTTP stream to analyze the response packet and know that purpose of this file is check

