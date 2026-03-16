# Scenario
Incident response recovered a damaged archive from an isolated workstation. The bundle split into two branches during transfer: one looks like duplicate camera captures, and the other is an absurdly deep archive chain.
Follow both trails, reconstruct the hidden message, and recover the token.

# Solve
We have two files, the first one has two pictures which seem very similar, I used strings command to see what diference inside them and found that the difference is `AUTH_FRAGMENT_B64` value. So I decoded-base64 both of them.

At the first image named `cam_300.jpg`, we have this message `Always_check_both_images` and the second one is `0r4ng3_ArCh1v3_T4bSp4ce!`.

I don't know the message in the 2nd picture is the hint or a part of flag so I processed solve the 2nd file. This file was compressed 999 times so I code a tool to extract all of them
```python
import tarfile
import os

def extract_tar(file_path, extract_path='.'):
    current_file = file_path
    try:
        while tarfile.is_tarfile(current_file):
            with tarfile.open(current_file, 'r') as tar:
                member_names = tar.getnames()
                print(current_file)
                tar.extractall(path=extract_path)
                current_file = os.path.join(extract_path, member_names[0])              
    except Exception as e:
        print(f"error: {e}")

target_file = 'File2.tar'
extract_tar(target_file, './extracted_result')
```
After extracted all, we have a file named `noo.txt`. I open this file and see that this is a binary file with the `PK` magic bytes. I sure that this is a zip file so I changed it's extension from `.txt` to `.zip` and extract them. But it require the password to extracted, then I look back on the messages from two pictures that I think highly is a password so I tried them one by one and the password is the second message. Inside the zip file contain two files, one naned notes.md and the others is Notaflag.txt. First I open notes.md
<img width="622" height="182" alt="image" src="https://github.com/user-attachments/assets/2ea406d3-c2d7-44b6-b295-f82238165ffe" />

When I open another file, I don't see anything in this. But when I use `ctrl + A` to highlight all text to check the whitespace language possible then I see that
<img width="1179" height="1008" alt="image" src="https://github.com/user-attachments/assets/da56a27d-f831-4efe-b94a-529617dacc97" />

It highly is the whitespace language so I use the following python program to decode it by transform space character to 0 and tab to 1
```python
with open('NotaFlag.txt', 'r') as f:
    raw_bin = "".join(['0' if c == ' ' else '1' for c in f.read() if c in ' \t'])
ascii_text = ""
for i in range(0, len(raw_bin), 8):
    byte = raw_bin[i:i+8]
    if len(byte) == 8:
        ascii_text += chr(int(byte, 2))

print(ascii_text)
```
After run this, I got the flag
> utflag{d1ff_th3_tw1ns_unt4r_th3_st0rm_r34d_th3_wh1t3sp4c3}
