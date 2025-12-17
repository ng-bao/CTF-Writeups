# PsExec Hunt Lab
<img width="1103" height="267" alt="image" src="https://github.com/user-attachments/assets/94d15c65-c7a6-42ff-aa3c-ebc372e58107" />
I have a pcapng file and following the scenario. Following the clue is PsExec, I can guess it relative to MSB2 protocol(This protocol is typically used by Windows to share files or printers within a LAN) so I focus on it to answer questions.

## Solve
Q1: To effectively trace the attacker's activities within our network, can you identify the IP address of the machine from which the attacker initially gained access?

After filt the MSB protcol, I could see that machine 10.0.0.130 is sending a request to write data to machine 10.0.0.133. Data is being written to a file named PSEXESVC.exe
<img width="1249" height="201" alt="image" src="https://github.com/user-attachments/assets/3a864c25-345a-4c0e-a75c-67711e6554c6" />
I have then perfomed a research on the PSEXESVC.exe and finding that this is the characteristic behavior of the PsExec tool. PsExec allows a user to execute command line instructions on another computer remotely. And hacker can use this tool to take control of another machine. So 10.0.0.130 clearly is the IP address of the machine from which the attacker initially gained access.

> 10.0.0.130

---
Q2: To fully understand the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted?

The connect process has three steps. In step two, we have the NTLM handshake, the server(victim device) will send a random number to the client(attacker device) it called Server Challenge. In this process, the server will introduce about itself included hostname. I use filter to filt ntlm challenge and we will get answer in tag Target Name.
<img width="830" height="195" alt="image" src="https://github.com/user-attachments/assets/98e7f22b-37e8-4b17-b77f-3a2e0efb28f2" />
> SALES-PC

---
Q3: Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What is the username utilized by the attacker for authentication?

In this question, we need to find username hacker used to authentication. return to ntlm handshake, after the victim device send challenge to hacker device, attacker must send username + hash(ntlm responde), so I tried find a responde and find that.
<img width="1387" height="29" alt="image" src="https://github.com/user-attachments/assets/a199914e-6450-42b8-991e-58c3cfbf14b5" />
>ssale

---
Q4: After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What's the name of the service executable the attacker set up on the target?

in Q1, we already find that is PSEXESVC.exe
<img width="1083" height="49" alt="image" src="https://github.com/user-attachments/assets/43f183ca-f17a-4caf-a9d1-a6c33aafb4b5" />
> PSEXESVC 

---
Q5: We need to know how the attacker installed the service on the compromised machine to understand the attacker's lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machine?

To install a executive file to control device, we must have administrator rights. So PsExec must use ADMIN$ to install the service on target machine 
