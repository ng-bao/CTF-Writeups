# Scenario
Our SOC captured suspicious traffic from a lab VM right before dawn. Most packets look like ordinary client chatter, but a few are pretending to be something they are not.

# Solve
They give me a pcap file, so I will use wireshark to analyze this. The first seven packets are the hint was provided by the author
> Read this slowly:
>  1) mDNS names are hints: alert.chunk, chef.decode, key.version
>  2) Not every 'TCP blob' is really what it pretends to be
>  3) If you find a payload that starts with PK, treat it as a file

In the 3rd hint, we have keyword `PK`. It means we will have a `.zip` file is being sent through network. Then I tried find the packet contains `PK` and found this in the packet No.36. This packet not only contain a zip file but also contain some TCP blob which seem not related to this zip file so I copy from the file header and skip TCP blob before the zip file by show this blob as raw and extracting the content starting from the ZIP's magic bytes.

After extracted the zip file, we got a new hint and stage2.bin file
> hint: not everything here is encrypted the same way

Then I ran strings on the new file and see this.

<img width="503" height="34" alt="Image" src="https://github.com/user-attachments/assets/57e03331-6a6f-4c03-8e26-77f03a333af6" />

Combined with new hint, I think only even-index characters is encrypted. We all know the flag format is `utflag{` so  I xor it with `t, l, g` to find the key and use the encrypted chars xor with key to get the full flag.

> utflag{h4lf_aw4k3_s33_th3_pr0t0c0l_tr1ck}
