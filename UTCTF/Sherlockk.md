# Scenario
We're almost done agent. All we need to do now is identify some Indicators of Compromise (IOCs) left by the threat actor, among other things. The triage is the same as the one in "Landfall" and "Watson". Can you read the briefing and solve your part of the case?

# Solve
### 1. CheckpointA: The threat actor downloaded a file from a online text storage site. Can you identify the complete URL the threat actor downloaded from?
The threat actor downloaded a file from a online text storage site that mean the traces will be preserved in `browser`. To analyze this file, I used a tool named `DB Browser (SQLite)`. First, I analyzed the `chrome's history` and found the online text storage site is `pastes.io`. Then, I proceeded to analyze the file named `Network Action Predictor`. It is a SQLite database file that maps user input (partial strings) to specific web destinations (URLs). In this database, I can see a process the threat actor is typing word by word.
<img width="1322" height="845" alt="image" src="https://github.com/user-attachments/assets/c216df83-fd4f-4865-a71c-4583f3cd0d91" />

Therefor, I can find what URL the threaat actor was acess that is `http://pastes.io/download/nhy8LSzI`.

### 2. CheckpointB:  The threat actor wrote a note on the machine that may or may not by benign. There may be multiple notes, but we want the one concerning the admin user.It's been deleted from the triage, but can you retrieve the contents of the note?
Initially, I checked in `$Recycle_bin` but not found no result. That mean the threat actor was completely deleted it. There is a only way to find this note which is analyze a file named `$MFT` by using the `MFTExplorer` tool. The `$MFT`, or Master File Table, is a critical component of the NTFS file system that stores metadata about all files and directories on an NTFS volume. That means we can recover data was lost form this. In the `$MFT`, I can see lots of data was recovered. Among them, I saw a file named `Adminitrastor Notes.txt` look very supiciously so I tried read the content inside this.  
<img width="160" height="79" alt="image" src="https://github.com/user-attachments/assets/e0cc40d4-f5e2-491b-afd1-1c984f211595" />

I tried combine them together and this is the correct answer.

Password: `Lettuce-Cabbage-Carrots`

### 3. CheckpointC: The threat actor downloaded a file enumeration script. Can you submit the MD5 Hash of that file?
I searched the downloads directory and discovered a script file. After checking the file on VirusTotal, I extracted its MD5 hash to use as the password, and it worked perfectly.

Passoword: `e86475121f231c02c4a63bd0915b9dff`
