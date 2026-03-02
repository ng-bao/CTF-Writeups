# Scenario
My sister's computer crashed. We were very fortunate to recover this memory dump. Your job is get all her important files from the system. From what we remember, we suddenly saw a black window pop up with something being executed. When the crash happened, she was trying to draw something. Thats all we remember from the time of crash.
Note: This challenge is composed of 3 flags.

# Initial Thoughts
Based on the scenario, we can see some clues:
+ A black window pop up with some thing being executed.
+ The crash happened when she was trying to draw something.
+ Get all her important files from the system.

# Solve
The first thing I do is determine the profile we are going to use. So I use the imageinfo plugin to get this.
```
vol2.exe -f MemoryDump_Lab1.raw imageinfo
```
<img width="1458" height="408" alt="image" src="https://github.com/user-attachments/assets/6c85dc8f-66df-4257-a8a1-f76e75bf68df" />

After determining the profile, following the first clue, I used the cmdscan plugin to extract command history to find some unusual commands.
```
vol2.exe -f MemoryDump_Lab1.raw --profile=Win7SP1x64_23418 cmdscan
```
<img width="1450" height="464" alt="image" src="https://github.com/user-attachments/assets/a73edb82-d93a-4962-b666-1511dfc85cd9" />
Focusing on the highlighted part, we can see this command:

> Cmd #0 @ 0x1de3c0: St4G3$1

So I use the consoles plugin to see more detail about this command.
<img width="903" height="200" alt="image" src="https://github.com/user-attachments/assets/e79306da-b79d-44d6-b84d-cd4cb4691bae" />

We can see the command returned a base64 code `ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0=`. Then I tried to decode this and get the first flag.
<img width="1497" height="563" alt="image" src="https://github.com/user-attachments/assets/085b992c-b88e-4459-bc73-05ecc80a6e40" />
> flag{th1s_1s_th3_1st_st4g3!!}

Next to the second clue: `The crash happened when she was trying to draw something`. She’s drawing on her computer, so I think it might have some paint's processes on it. I used pslist plugin to list all running processes.
```
vol2.exe -f MemoryDump_Lab1.raw --profile=Win7SP1x64_23418 pslist
```
<img width="1383" height="101" alt="image" src="https://github.com/user-attachments/assets/467aaa2c-3340-49af-b795-7a6749a82041" />

Just as I thought, we have a process related to "paint". Then I dumped memory of this process and get a file named 2424.dmp
```
vol2.exe -f MemoryDump_Lab1.raw --profile=Win7SP1x64_23418 memdump -P 2424 -D .
```
In RAM, an image doesn't exist as a complete file, but rather as a continuous sequence of pixels. Paint holds these pixels in memory to display them on the screen. By adjusting the Width in GIMP, we are helping the software determine after how many pixels it should wrap to the next line to form a complete picture. So I will use GIMP(GNU Image Manipulation Program) and renamed this file to 2424.data so that GIMP can read it.
<img width="740" height="782" alt="image" src="https://github.com/user-attachments/assets/ed6d18ee-9664-400e-8327-53117fa54d65" />

After several minutes, I finally dialed in the parameters that allowed me to render the drawn content. But it had a small issue, the picture is inverted vertically and mirrored horizontally, I solved it by apply the Flip vertically function to correct the photo and get the second flag.
<img width="1216" height="942" alt="image" src="https://github.com/user-attachments/assets/78583e76-8589-4930-be02-49769edb9ad6" />
> flag{Good_Boy_good_girl_}

To the final clue, we have to recover all her important files. Before that, while I was checking processes list, I saw WinRAR.exe process
<img width="1364" height="62" alt="image" src="https://github.com/user-attachments/assets/4a05c5a8-84f9-40a2-b217-97b5aea32903" />

Therefor, I used the cmdline to display process command-line arguments
```
vol2.exe -f MemoryDump_Lab1.raw --profile=Win7SP1x64_23418 cmdline
```
<img width="1455" height="74" alt="image" src="https://github.com/user-attachments/assets/840264b0-343b-4885-9145-0a340f0b0412" />

We have a file named Important.rar. It seemed to contain the flag inside, so I'm going to dump this file by use the dumpfiles plugin. First, I use the filescan plugin combined with findstr to find the physical offset of this file.
```
vol2.exe -f MemoryDump_Lab1.raw --profile=Win7SP1x64_23418 filescan | findstr /I "important.rar"
```
<img width="1455" height="156" alt="image" src="https://github.com/user-attachments/assets/64272a09-784b-4b18-b828-8eb13bd2560b" />

After I have it, I proceeded to dump that file.
```
vol2.exe -f MemoryDump_Lab1.raw --profile=Win7SP1x64_23418 dumpfiles -Q 0x000000003fa3ebc0 -D .
```
We have a file named `file.None.0xfffffa8001034450.dat`. Then I renamed it to Important.rar and extract it
<img width="828" height="236" alt="image" src="https://github.com/user-attachments/assets/71cfb240-865b-4287-acf3-e2e239f60704" />

It request password to extract and we can see the pop up message tell us the password is NTLM hash(in uppercase) of Alissa's account passwd. I used the hashdump plugin to dumps passwords hashes (LM/NTLM) from memory
```
vol2.exe -f MemoryDump_Lab1.raw --profile=Win7SP1x64_23418 hashdump
```
<img width="1457" height="192" alt="image" src="https://github.com/user-attachments/assets/215f7421-4b0d-4c46-a5c8-6b67fe7def59" />

The hash `f4ff64c8baac57d22f22edc681055ba6` is NTLM hash of Alissa's account password. Then we just convert this hash to uppercase and enter password for flag3.png.
After extracted, I open the flag3.png file to get the 3rd flag.
<img width="506" height="502" alt="image" src="https://github.com/user-attachments/assets/2ea84e5d-0466-41dc-b5f9-f1d14dde7f19" />

> flag{w3ll_3rd_stage_was_easy}
