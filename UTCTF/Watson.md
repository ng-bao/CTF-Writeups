# Scenario
We need your help again agent. The threat actor was able to escalate privileges. We're in the process of containment and we want you to find a few things on the threat actor. The triage is the same as the one in "Landfall". Can you read the briefing and solve your part of the case?

# Solve
This chall is same to the "Landfall" but it has two checkpoints.

* CheckpointA: The threat actor deleted a word document containing secret project information. Can you retrieve it and submit the name of the project?

  Based on CheckpointA, I checked in `$Recycle.bin` with the hope that the attacker has not completely deleted it. In `$Recycle.Bin`, I saw some file were deleted and a folder, I moved inside this folder and saw a file with original name that mean this file wasn't deleted by threat actor. I opened this file and saw that this is a project named `HOOKEM` which is also a password for checkpointA.

* CheckpointB: The threat actor installed a suspicious looking program that may or may not be benign. Retrieve the SHA1 Hash of the executable.
  
  Moving on to checkpointB. Initially, I searched all the executable and check them on the `VirusTotal` tool but found no results. After doing some research, I found out that there is another way to find the SHA-1 of the executable that is check a file named `Amcache.hve`. This is a file which contain all of log about the plugins, drivers, and of course, it also have a list of the executables were executed on this device. To analyze this file, we need the `AmcacheParser` tool.
  <img width="767" height="232" alt="image" src="https://github.com/user-attachments/assets/2232a0f6-d3b7-489d-95d6-011651c2b1ab" />

  After used this tool, we have some `.csv` files, we need focus on the file named `20260318213946_Amcache_UnassociatedFileEntries.csv`. We have too many the executables in this file so I tried some supicious files like `cookie_exporter (is used to export cookie from broswer)` but all of them aren't the correct password. I tried to anlyze the hint of checkpointB again and highlight this text `may or may not be benign`, which mean the executable is look like a normal system file so beside check file name, I also check the path of them. Finally, I found this.
  <img width="1160" height="58" alt="image" src="https://github.com/user-attachments/assets/f8c66d8c-88f0-4c60-851c-b75cecd73d19" />

  Normally, it is located in path `C:\Windows\System32\calc.exe`, but it wasn't right in this case. And looking at the `ProductName` column, it's named `helloword` so I was certain that it's SHA-1 which is `67198a3ca72c49fb263f4a9749b4b79c50510155` is the correct password. Then I enter the password to get part 2 of flag and join the two parts of the flag together.
> utflag{pr1v473_3y3-m1551n6_l1nk} 
