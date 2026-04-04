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
* Cyberchef  
* Steghide

# Prepare
We first start by using imageinfo plugin to determine the image profile.
<img width="1013" height="75" alt="image" src="https://github.com/user-attachments/assets/7252a966-3954-4df2-8d76-43756837e2b7" />

Profile: `Win7SP1x86`

# Analyze the description
* We need to identify the `secret piece of information` (the encrypted text) and the `malicious script` to understand the encryption algorithm used.
* Combined with `Note-2` and the `steghide` tool, it is likely that the second part of the flag is embedded within an image.

# Solve
First, I reviewed the process list to find for any unusual activity but found nothing suspicious. Next, I used the `cmdline` plugin to display process command-line arguments and found some files seem suspicious. 
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
* And with `vip.txt`, we have a base64 string `am1gd2V4M20wXGs3b2U=`.

We can see the encryption algorithm used in this script which is encoding to `Base64` and  `XOR with 3`.Based on that, we can recover the plaintext, which is the first part of flag

Part-1: `inctf{0n3_h4lf`

After getting the first part, I searched some file with keywords `jpg`, `png`, `jpeg` and found this.
<img width="1681" height="86" alt="image" src="https://github.com/user-attachments/assets/ba4e7fe5-d54d-42f4-962d-82b1f6b83a2a" />

After extracted it, I tried open to check but there is nothing. After that, I extracted by using the `steghide` tool with the password is the first part and got a file named `secret text`, which contain the second part of flag.

part-2: `_1s_n0t_3n0ugh}`

Flag: `inctf{0n3_h4lf_1s_n0t_3n0ugh}`
