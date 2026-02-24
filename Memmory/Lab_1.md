# Scenario
My sister's computer crashed. We were very fortunate to recover this memory dump. Your job is get all her important files from the system. From what we remember, we suddenly saw a black window pop up with some thing being executed. When the crash happened, she was trying to draw something. Thats all we remember from the time of crash.

# Initial Thoughts
Based on the scenario, we can see some clues:
+ A black window pop up with some thing being executed.
+ The crash happened when she was trying to draw something.
+ Get all her important files from the system.

# Solve
The first thing I do is determine the profile we are going to use. So I use the imageinfo plugin to get this.
> vol2.exe -f MemoryDump_Lab1.raw imageinfo

<img width="1458" height="408" alt="image" src="https://github.com/user-attachments/assets/6c85dc8f-66df-4257-a8a1-f76e75bf68df" />

After determine the profile, following the first clue, I used the cmdscan plugin to extract command history to find some unusual command
> vol2.exe -f MemoryDump_Lab1.raw --profile=Win7SP1x64_23418 cmdscan

<img width="1450" height="464" alt="image" src="https://github.com/user-attachments/assets/a73edb82-d93a-4962-b666-1511dfc85cd9" />
Focus on the highlight, we can see this command:

> Cmd #0 @ 0x1de3c0: St4G3$1

So I use the consoles plugin to see more detail about this command.
<img width="903" height="200" alt="image" src="https://github.com/user-attachments/assets/e79306da-b79d-44d6-b84d-cd4cb4691bae" />

We can see the command return a base64 code `ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0=`. Then I tried to decode this and get the first flag.

<img width="1497" height="563" alt="image" src="https://github.com/user-attachments/assets/085b992c-b88e-4459-bc73-05ecc80a6e40" />
> flag{th1s_1s_th3_1st_st4g3!!}

Next to the second clue, 

