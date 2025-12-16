# PsExec Hunt Lab
<img width="1103" height="267" alt="image" src="https://github.com/user-attachments/assets/94d15c65-c7a6-42ff-aa3c-ebc372e58107" />
I have a pcapng file and following the scenario. Following the clue is PsExec, I can guess it relative to MSB2 protocol(This protocol is typically used by Windows to share files or printers within a LAN) so I focus on it to answer questions.

## Solve
Q1) To effectively trace the attacker's activities within our network, can you identify the IP address of the machine from which the attacker initially gained access?

After filt the MSB protcol, I could see that machine 10.0.0.130 is sending a request to write data to machine 10.0.0.133. Data is being written to a file named PSEXESVC.exe
<img width="1249" height="201" alt="image" src="https://github.com/user-attachments/assets/3a864c25-345a-4c0e-a75c-67711e6554c6" />
I have then perfomed a research on the PSEXESVC.exe and finding that this is the characteristic behavior of the PsExec tool. PsExec allows a user to execute command line instructions on another computer remotely. And hacker can use this tool to take control of another machine. So 10.0.0.130 clearly is the IP address of the machine from which the attacker initially gained access.

---
Q2) To fully understand the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted?

The connect process has three steps. In step two, it use n 
