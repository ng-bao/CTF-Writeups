# Desciption
My system was recently compromised. The Hacker stole a lot of information but he also deleted a very important file of mine. I have no idea on how to recover it. The only evidence we have, at this point of time is this memory dump. Please help me.

**Note**: This challenge is composed of only 1 flag.

The flag format for this lab is: **inctf{s0me_l33t_Str1ng}**

# Tools/Softwares
* Volatility 2

# Analyze the description
* The main goal of this challenge is recover the very important file that was deleted to find the flag. I will determine the deleted file by using the `filescan` plugin. Because in Windows, when a file is deleted or a process is closed, the operating system does not immediately erase the related data structures in RAM. Instead, it only marks those memory areas as 'Free' (ready to be overwritten). That means I can ever find the deleted file although it was deteted.

* After that, I can use the `mftparser` plugin to scans for and parses potential MFT entries or review the clipboard content with hopes can find something supicious here.

# Prepare
We first start by using imageinfo plugin to determine the image profile.
<img width="1635" height="76" alt="image" src="https://github.com/user-attachments/assets/7565cc9b-ac13-416d-9d70-84fd3afa30e7" />

Profile: `Win7SP1x64_23418`

# Solve
First, I determined the important file named `Important.txt ` and actually, we can't extract it because this file was corrupted.

When I was reviewing the process list, I detected a process named `StickyNot.exe`.
> Sticky Notes is a desktop notes application included in Windows 7,[2] Windows 8, Windows 8.1, Windows 10 and Windows 11.[3] The app loads quickly and enables users to quickly take notes using post-it note–like windows on their desktop.

I think it maybe contain something interested so I tried to find the file that contains note by scan with keyword `.snt` (`snt` is the extension of data storage file of the Sticky Notes application)
<img width="1660" height="76" alt="image" src="https://github.com/user-attachments/assets/e828f1a1-9e78-49ba-9e44-c0f04a82d954" />

We got a note after extracted it.
> the clipboard plugin works well but it doesn't give the flag

We got a hint(I think so). Then I reviewed the clipboard content but have no result. 

I shifted my focus to `Master File Table` and finally found the content of `Important.txt` that was deleted.
<img width="1856" height="433" alt="image" src="https://github.com/user-attachments/assets/a6340f4d-0b28-4a97-823a-659b17c5b7af" />

Flag: `inctf{1_is_n0t_EQu4l_7o_2_bUt_th1s_d0s3nt_m4ke_s3ns3}`
