# Description
A malicious script encrypted a very secret piece of information I had on my system. Can you recover the information for me please?

Note-1: This challenge is composed of only 1 flag. The flag split into 2 parts.

Note-2: You'll need the first half of the flag to get the second.

You will need this additional tool to solve the challenge,
```
$ sudo apt install steghide
```
The flag format for this lab is: inctf{s0me_l33t_Str1ng}

# Tools/Software
* Cyperchef  
* Steghide

# Prepare
We first start by using imageinfo plugin to determine the image profile.
<img width="1013" height="75" alt="image" src="https://github.com/user-attachments/assets/7252a966-3954-4df2-8d76-43756837e2b7" />

Profile: `Win7SP1x86`

# Analyze the description
We need to find the `secret piece of information` to get the encrypted text and the `malicious script` to know what algorithms is used to encrypted them. Combined from `Note-2` and the `steghide` tool, I think another part of flag is embeded inside a picture.

# Solve
First, I reviewed the process list to find some unusual process but have no result. Next, I used the `cmdline` plugin to display process command-line arguments and found some files seem suspicious. 
<img width="1094" height="152" alt="image" src="https://github.com/user-attachments/assets/62a802f9-c6ba-4e59-b033-973e009b5279" />

Both of them are from `notepad.exe` That's the reason why I couldn't find anything suspicious in process list. After that I proceeded extract them.

* We have a python scrip like this in `evilscrips.py.py`.
```python
import sys
import string

def xor(s):

        a = ''.join(chr(ord(i)^3) for i in s)
        return a


def encoder(x):

        return x.encode("base64")


if __name__ == "__main__":

        f = open("C:\\Users\\hello\\Desktop\\vip.txt", "w")

        arr = sys.argv[1]

        arr = encoder(xor(arr))

        f.write(arr)

        f.close()
```
* And in `vip.txt` we have a base64 string `am1gd2V4M20wXGs3b2U=`.

We can see this scrip 
vol2.exe -f MemoryDump_Lab3.raw --profile=Win7SP1x86
Win7SP1x86_23418
inctf{0n3_h4lf_1s_n0t_3n0ugh}
