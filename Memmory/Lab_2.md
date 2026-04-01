# Description
One of the clients of our company, lost the access to his system due to an unknown error. He is supposedly a very popular "environmental" activist. As a part of the investigation, he told us that his go to applications are browsers, his password managers etc. We hope that you can dig into this memory dump and find his important stuff and give it back to us.

**Note**: This challenge is composed of 3 flags.

# Tools/Software
* volatility 2
* keepass 2
* SQLite

# Prepare
We first start by using `imageinfo` plugin to determine the image profile.
<img width="1630" height="83" alt="image" src="https://github.com/user-attachments/assets/03ff3168-2cfa-46db-a374-e52274b1c89d" />

Profile: `Win7SP1x64_23418`

# Analyze the description
The client is supposedly a very popular "environmental" activist so we will focus on the keyword `environmental`. The next clues are `password managers` and `browers`. About `password managers`, we will determine it by review the process list. As for the `broswer` clue, I will analyze the `history` or `network action predictor` file. 

# Solve
Based on the keyword `"environmental" activist`, I Checked the environment variables and found some base64 strings in the paths.
<img width="1867" height="178" alt="image" src="https://github.com/user-attachments/assets/ed474c40-69ea-4c52-98ba-47898bcbd0d2" />

By decoded them, we got the first flag.

Flag 1: `flag{w3lc0m3_T0_$T4g3_!_Of_L4B_2}`

To find some processes relatived to `password managers`, I have reviewed the list of processes that have been running and found a process named `KeePass.exe`.
> KeePass is a free open source password manager, which helps you to manage your passwords in a secure way. You can store all your passwords in one database, which is locked with a master key. So you only have to remember one single master key to unlock the whole database. Database files are encrypted using the best and most secure encryption algorithms currently known (AES-256, ChaCha20 and Twofish).

By default, the `Keepass` database is stored on a local file system. So I used `filescan` plugin combined with `findstr` command to find it. I will find with the keyword `.kdb` because I don't know what keypass version does the victim is using. 
<img width="1720" height="116" alt="image" src="https://github.com/user-attachments/assets/2855f772-4824-4faf-be3a-7b638f21f9c7" />

Extension `kdbx` means the victim is using the Keepass 2, so I used it to read the database. But we have a problem, we need to find the password to go inside it. I think a normal person would put the password in some file so I tried search with some keywords like `secret`, `hidden`, `password`,... and I luckily found that.
<img width="1877" height="377" alt="image" src="https://github.com/user-attachments/assets/e2335f13-fa3f-42a8-9638-8ce996bde52f" />

Then I extracted this file and got the password.

<img width="538" height="717" alt="image" src="https://github.com/user-attachments/assets/ab2f27c9-4928-4551-a8cc-8124494d243d" />

password: `P4SSw0rd_123`

After got the password, we can find the flag is the password of username `Flag`.
<img width="863" height="630" alt="image" src="https://github.com/user-attachments/assets/dacb7083-2367-4f40-8b86-3e1a02618914" />

Flag 2: `flag{w0w_th1s_1s_Th3_SeC0nD_ST4g3_!!}`

By parsing `history`, we can see a download link.
<img width="1915" height="1130" alt="image" src="https://github.com/user-attachments/assets/e4df3729-2a8c-4ad6-8af8-cfed03f6ecae" />

That means the victim downloaded something on the internet, then I search this link on browser and got a file named `Important.zip`. But it require password to extract and they give the way to find this password.
<img width="885" height="70" alt="image" src="https://github.com/user-attachments/assets/a051ba5e-54c2-4b51-856f-16bb209e0cd7" />




vol2.exe -f MemoryDump_Lab2.raw --profile=Win7SP1x64_23418
>flag{w3lc0m3_T0_$T4g3_!_Of_L4B_2}
>
