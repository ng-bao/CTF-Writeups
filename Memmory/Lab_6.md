# Description
We received this memory dump from the Intelligence Bureau Department. They say this evidence might hold some secrets of the underworld gangster David Benjamin. This memory dump was taken from one of his workers whom the FBI busted earlier this week. Your job is to go through the memory dump and see if you can figure something out. FBI also says that David communicated with his workers via the internet so that might be a good place to start.

Note: This challenge is composed of 1 flag split into 2 parts.

The flag format for this lab is: inctf{s0me_l33t_Str1ng}

# Tools
* Volatility 2
* DB Browser for SQLite / sqlite3

# Solve
After listing all processes by using `pslist`, I saw both Chrome and Firefox was accessed.
Based on the description, I conducted a search to find a flie contain information about `Browsing history` from Chrome and Firefox.
* Chrome database






vol2 -f MemoryDump_Lab6.raw --profile=Win7SP1x64_23418
