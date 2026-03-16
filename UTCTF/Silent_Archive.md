# Scenario
Incident response recovered a damaged archive from an isolated workstation. The bundle split into two branches during transfer: one looks like duplicate camera captures, and the other is an absurdly deep archive chain.
Follow both trails, reconstruct the hidden message, and recover the token.

# Solve
We are given two files. The first set consists of two images that appear identical. By using the strings command to compare them, I discovered that the difference lies in the AUTH_FRAGMENT_B64 value. After decoding the Base64 strings from both images:

* cam_300.jpg revealed the message: `Always_check_both_images`

* The second image revealed: `0r4ng3_ArCh1v3_T4bSp4ce!`

The second file was a recursive archive, compressed 999 times. I wrote a Python script to automatically extract all layers:
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
After extraction, a file named `noo.txt` was obtained. Examining the file's magic bytes revealed `PK`, indicating it was actually a `ZIP` file. I changed the extension to `.zip` and attempted to extract it. It prompted for a password, and the second message found in the images `0r4ng3_ArCh1v3_T4bSp4ce!` worked successfully. Inside the zip file contain two files, one naned notes.md and the others is Notaflag.txt. First I open notes.md

<img width="622" height="182" alt="image" src="https://github.com/user-attachments/assets/2ea406d3-c2d7-44b6-b295-f82238165ffe" />

While Notaflag.txt appeared empty at first glance, highlighting the text (Ctrl + A) revealed hidden characters (tabs and spaces).
<img width="1179" height="1008" alt="image" src="https://github.com/user-attachments/assets/da56a27d-f831-4efe-b94a-529617dacc97" />

This is a classic Whitespace Steganography challenge. I used the following Python script to decode the message by treating spaces as 0 and tabs as 1:
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
> utflag{d1ff_th3_tw1ns_unt4r_th3_st0rm_r34d_th3_wh1t3sp4c3}
