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

### Chrome database
  
Diving into  `Chrome database`, we can see a link that took to a website called `Pastebin` contains message including a online document link and a note. By access to that online document, it contain a long paragraph and we found a mega download link was embeded into it.We tried to accessed that link but it requires a key to download.
<img width="1280" height="800" alt="Screenshot 2026-08-12 143717" src="https://github.com/user-attachments/assets/521106b2-23e3-4c63-8535-55a683f51680" />

**Link:** `https://pastebin.com/RSGSi1hk`

<img width="778" height="162" alt="Screenshot 2026-08-12 144025" src="https://github.com/user-attachments/assets/868b13a3-f15d-4006-921c-56c5339f788f" />

After that, we check in `Clipboard` and `Environment` but have no result, then while checking the screenshots, we found that
<img width="1167" height="595" alt="Screenshot 2026-08-12 151753" src="https://github.com/user-attachments/assets/deef0ba3-2aba-4b9c-a835-99b7730113ad" />


It means suspect logged in email via `Firefox` so we continue investigate the Firefox history database to find more clues.

### Firefox database
  
Finding in this, we saw 3 emails were opened, one of them has the title that seems suspicious.
<img width="2559" height="1599" alt="image" src="https://github.com/user-attachments/assets/35d5affa-6f55-4ca1-bdf8-9b346ffcd9d4" />

we think it posibility is the key that we are finding. To verify our prediction, we tried use `strings` on this disk image and grep for the keyword `Mega Drive Key` because raw JSON/HTML data is cached by Firefox in `RAM`.
<img width="2559" height="326" alt="image" src="https://github.com/user-attachments/assets/27e65bb2-0c14-4982-8834-e32fd940a077" />


The higlight text in this image is the content that was sent from `danielbenjamin683@gmail.com` to `davidbenjamin939@gmail.com` and the key is same with the key we found in firefox database.

**Key:** `zyWxCjCYYSEMA-hZe552qWVXiPwa5TecODbjnsscMIU`.

---
By using that key, we can download a file named `flag_.png` but it was corrupted. Checking its hex code, we found an error in the chunk `iHDR` (the correct one is `IHDR`), then we edited that to the correct form a`nd fixed that image.
<img width="500" height="500" alt="flag_fixed" src="https://github.com/user-attachments/assets/38244906-f4b4-4df9-88e0-66aea2f347aa" />

**Part 1:** `inctf{thi5_cH4LL3Ng3_!s_g0nn4_b3_!_`.

By using `cmdscan` plugin, we can see a file named `flag.rar` but it requires a password to extracted. At first, we think the first part is the password but it was wrong so we continue found in this `memory dump` and finally found in process environment variables
<img width="1320" height="53" alt="image" src="https://github.com/user-attachments/assets/7e1a2a11-8142-4872-988c-ddb1f6ecf993" />

After extracting, we got the second part of flag.

<img width="500" height="500" alt="flag2" src="https://github.com/user-attachments/assets/78b69b2e-95a2-479e-b02b-9480878de450" />

**Part 2:** `aN_Am4zINg_!_i_gU3Ss???_}`.

