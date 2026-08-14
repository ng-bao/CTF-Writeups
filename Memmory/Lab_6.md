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
<img width="1280" height="800" alt="Screenshot 2026-08-12 143717" src="https://github.com/user-attachments/assets/521106b2-23e3-4c63-8535-55a683f51680" />

<img width="778" height="162" alt="Screenshot 2026-08-12 144025" src="https://github.com/user-attachments/assets/868b13a3-f15d-4006-921c-56c5339f788f" />

After that, we check in `Clipboard` and `Environment` but have no result, then while checking the screenshots, we found that
<img width="1167" height="595" alt="Screenshot 2026-08-12 151753" src="https://github.com/user-attachments/assets/deef0ba3-2aba-4b9c-a835-99b7730113ad" />


It means suspect logged in email via `Firefox` so we continue investigate the Firefox history database to find more clues.
* Firefox database
Finding in this, we saw 3 emails were opened, one of them has the title that seems suspicious.



vol2 -f MemoryDump_Lab6.raw --profile=Win7SP1x64_23418
