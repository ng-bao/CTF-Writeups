# Desciption
My system was recently compromised. The Hacker stole a lot of information but he also deleted a very important file of mine. I have no idea on how to recover it. The only evidence we have, at this point of time is this memory dump. Please help me.

**Note**: This challenge is composed of only 1 flag.

The flag format for this lab is: **inctf{s0me_l33t_Str1ng}**

# Tools/Softwares
* Volatility 2

# Analyze the description
The main goal of this challenge is recover the very important file that was delete to find the flag. 

vol2.exe -f MemoryDump_Lab4.raw --profile=Win7SP1x64_23418
inctf{1_is_n0t_EQu4l_7o_2_bUt_th1s_d0s3nt_m4ke_s3ns3}
