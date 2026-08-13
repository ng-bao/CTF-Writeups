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
Diving into  `Chrome database`, we can see a link that took to a website called `Pastebin` contains message including a online document link and a note. By access to that online document, it contain a long paragraph and we found a mega download link was embeded into it.We tried to accessed that link but it requires a key to download.

After that, we check in `Clipboard` and `Environment` but have no result, then while checking the screenshots, we found that


It means owner's device logged in email via `Firefox` so we continue investigate the Firefox history database to find more clues.





vol2 -f MemoryDump_Lab6.raw --profile=Win7SP1x64_23418
