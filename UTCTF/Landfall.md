# Scenario
You are a DFIR investigator in charge of collecting and analyzing information from a recent breach at UTCTF LLC. The higher ups have sent us a triage of the incident. Can you read the briefing and solve your part of the case?

# Solve
This challenge provides a folder representing a restored C: drive. To solve it, we need to find the password to extract CheckpointA.zip. The hint for the password is: `"What command did the threat actor attempt to execute to obtain credentials for privilege escalation?"`

First, I checked `console_history` to see which commands the attacter ran. 
<img width="1919" height="515" alt="image" src="https://github.com/user-attachments/assets/d03fc888-1c7d-472b-82e4-0b394f101c6a" />

While decoding the commands, I saw a `wget` command that was used to download a file name `mimikatz_trunk.zip`.
<img width="1485" height="561" alt="image" src="https://github.com/user-attachments/assets/a05687c2-8127-4645-adba-103590887a86" />

I did some research on the file online and discovered that Mimikatz is a powerful tool used for extracting credentials from Windows systems; In other words, this tool will help for privilege escalation. Then I continued extract the remaining commands and found the command which the threat actor was use to run this tool.
<img width="1493" height="584" alt="image" src="https://github.com/user-attachments/assets/f8b9d3a9-48f0-41b4-a625-14d6b5eefd3a" />

After found the command, we need to convert to the correct password form which is MD5 hash of the encode portion(base64). Therefor, the password for checkpointA is `00c8e4a884db2d90b47a4c64f3aec1a4`
> utflag{4774ck3r5_h4v3_m4d3_l4ndf4ll}
