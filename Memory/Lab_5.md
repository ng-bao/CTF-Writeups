# Description
We received this memory dump from our client recently. Someone accessed his system when he was not there and he found some rather strange files being accessed. Find those files and they might be useful. I quote his exact statement,
> The names were not readable. They were composed of alphabets and numbers but I wasn't able to make out what exactly it was.

Also, he noticed his most loved application that he always used crashed every time he ran it. Was it a virus?

Note-1: This challenge is composed of 3 flags. If you think 2nd flag is the end, it isn't!! :P

Note-2: There was a small mistake when making this challenge. If you find any string which has the string "L4B_3_D0n3!!" in it, please change it to "L4B_5_D0n3!!" and then proceed.

Note-3: You'll get the stage 2 flag only when you have the stage 1 flag.

# Tools
* Volatility 2
* Ghidra

# Solve
First we used `pslist` to overview this memory dump, we found some processes `NOTEPAD.EXE` were run and after each of them is a process `WerFault.exe`. It mean the crashes are happenned when victim run `NOTEPAD.EXE`. Even the name is not right (the correct one is `notepad.exe`)
<img width="1626" height="382" alt="image" src="https://github.com/user-attachments/assets/fa4cf3fe-4a8d-4e5a-91ca-a6dcf3bbdbb0" />

So we ran `cmdline` to get more information about this process and found that it inside regular directory instead of system directory,
<img width="1085" height="504" alt="image" src="https://github.com/user-attachments/assets/89590c47-f3e8-4205-98ab-13a209202e4a" />

Beside that unusual file, we also found a compressed file named `SW1wb3J0YW50.rar` that look so suspicious. After that, we downloaded both of them, `NOTEPAD.EXE` and `SW1wb3J0YW50.rar`.

We used `Ghidra` to analyze the executable file first.

<img width="388" height="1157" alt="image" src="https://github.com/user-attachments/assets/17fdf7da-74e8-400a-a9ee-6e520c30a360" />

As we can see in this function, the attacker assigns a `hex code` to register `EAX` and push onto the stack so we decoded whole `hex code` to word and got the flag.
```
62 69 30 73 7b 4d 33 6d 5f 6c 34 42 35 5f 4f 56 65 52 5f 21 7d
```
**Flag:** `bi0s{M3m_l4B5_OVeR_!}`

Next to the compressed file, it requires a password to extract. According to the `Note-1` of the description, we think the password highly is a the first flag. Luckily, we also have the clue to find it which is the quote in the description.
> The names were not readable. They were composed of alphabets and numbers but I wasn't able to make out what exactly it was.

After searching for a while, we found it in the image which was extracted by using the `screenshot` plugin
<img width="801" height="601" alt="session_1 WinSta0 Default" src="https://github.com/user-attachments/assets/5570e320-e3c7-40f5-a8cf-ae4e9e210c17" />
```
ZmxhZ3shIV93M0xMX2QwbjNfU3Q0ZzMtMV8wZl9MNEJfNV9EMG4zXyEhfQ
```
We tried search this name in hopes of finding a file but have no result, and then we realize that name is the `base64 code` so we decoded them and got the flag.

**Flag:** `flag{!!_w3LL_d0n3_St4g3-1_0f_L4B_5_D0n3_!!}`

With that flag, we also got the second flag by using it as a password
<img width="1080" height="1080" alt="Stage2" src="https://github.com/user-attachments/assets/c9e33318-cf78-4f68-8c76-ae5145f154cf" />

**Flag:** `flag{W1th_th1s_$taGe_2_1s_cOmPL3T3 _!!}`
