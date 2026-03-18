# Scenario
We're almost done agent. All we need to do now is identify some Indicators of Compromise (IOCs) left by the threat actor, among other things. The triage is the same as the one in "Landfall" and "Watson". Can you read the briefing and solve your part of the case?

# Solve
### 1. CheckpointA: The threat actor downloaded a file from a online text storage site. Can you identify the complete URL the threat actor downloaded from?

The threat actor downloaded a file from a online text storage site that mean the traces will be preserved in `browser`. To analyze this file, I used a tool named `DB Browser (SQLite)`. First, I analyzed the `chrome's history` and found the online text storage site is `pastes.io`. Then, I proceeded to analyze the file named `Network Action Predictor`. It is a SQLite database file that maps user input (partial strings) to specific web destinations (URLs). In this database, I can see a process the threat actor is typing word by word.
<img width="1322" height="845" alt="image" src="https://github.com/user-attachments/assets/c216df83-fd4f-4865-a71c-4583f3cd0d91" />

Therefor, I can find what URL the threaat actor was acess that is `http://pastes.io/download/nhy8LSzI`.

### 2. CheckpointB:  The threat actor wrote a note on the machine that may or may not by benign. There may be multiple notes, but we want the one concerning the admin user.It's been deleted from the triage, but can you retrieve the contents of the note?

Initially, I checked in `$Recycle_bin` but not found no result. That mean the threat actor was completely deleted it. There is a only way to find this note which is analyze a file named `$MFT` by using the `MFTExplorer` tool. This file 
