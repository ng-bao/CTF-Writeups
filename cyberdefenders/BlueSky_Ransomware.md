# BlueSky Ransomware 
<img width="1074" height="296" alt="image" src="https://github.com/user-attachments/assets/87b616fd-0f74-40bb-8c4e-ef03ffa720fc" />
## Solve

---
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


We could clearly see that host 87.96.21.84 (attacker) connected over port 1433 (SQL Server default) and initiated communication. So I checked in event log and saw that
<img width="1273" height="326" alt="image" src="https://github.com/user-attachments/assets/d6649c5f-e572-4ea1-8200-d7ad277e2e79" />

There are many events from MSSQLSERVER .The event ID 18454 means login successfull so I checked in Eventdata and found account username.
<img width="1209" height="185" alt="image" src="https://github.com/user-attachments/assets/a9fd3109-d556-41a3-98e8-487b0779741e" />

> sa

---
Q3: We need to determine if the attacker succeeded in gaining access. Can you provide the correct password discovered by the attacker?

After 


