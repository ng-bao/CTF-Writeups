# Scenario
We need your help again agent. The threat actor was able to escalate privileges. We're in the process of containment and we want you to find a few things on the threat actor. The triage is the same as the one in "Landfall". Can you read the briefing and solve your part of the case?

# Solve
This chall is same to the "Landfall" but it have two checkpoint.

* CheckpointA: The threat actor deleted a word document containing secret project information. Can you retrieve it and submit the name of the project?
* CheckpointB: The threat actor installed a suspicious looking program that may or may not be benign. Retrieve the SHA1 Hash of the executable.

Based on CheckpointA, I checked in `$Recycle.bin` with the hope that the attacker has not completely deleted it. In `$Recycle.Bin`, I saw some file were deleted and a folder, I moved inside this folder and see a file with original name that mean this file wasn't deleted by threat actor. I opened this file and saw that this a project named `HOOKEM` which is also a password for checkpointA.
