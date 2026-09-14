# Description
Author: s3asick5

We found these packets, but we were quite... skeptical about their true meanings. Are those just normal web server enumeration or there are something else going on?

Attachments: [forensic_enum_or_exfil.zip](https://ctf.uithacking.club/e49a8a38-56a4-42d3-a666-8af905f967e6)

# Solve
We were given a `pcap` file. First things we can see is too many `GET` request to server.
<img width="1280" height="459" alt="image" src="https://github.com/user-attachments/assets/828c1ffb-414e-43bf-a143-2c3f6a2befe2" />


In each packets, the request and respone look very similar except one thing which is the `Authorization` field. Then we tried to decode the credentials was sent in the first packet.
<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/d7dbeb1c-cc5b-42c1-81dc-fefacac63eac" />

We have user: `firefly` with a very long password. Then we realised that this password also was a `base64` encoded so we decoded this again and got something interested.
<img width="1007" height="634" alt="image" src="https://github.com/user-attachments/assets/01ccb247-b65c-4aed-9575-40a5e607e574" />

As we can see a `JFIF` magic header, that means the machine '192.169.247.1' is sending a image via `Authorization` field. After that, we also checked two request packets but can't see any magic header so we think whole request packets in here was used to transfer only one picture.

We processed extract all `credentials` content by extract all request packets to the `json` file first, then using `grep` to keep only `authorization` field. By `base64` decoding that credential, we got a image.
<img width="544" height="600" alt="flag" src="https://github.com/user-attachments/assets/6441ec78-d8bc-478b-9f8c-2f462b1f123e" />

This image look very usual. So we tried to extract `LSB` and `MSB` but have no result. Then we checked if this image contained any embedded files or information by using `binwalk` and `exiftool` but didn't get anything. Therefore, only one posible way we thinking, the flag maybe was embedded by the `steghide` tool, but we didn't know where is the password. First, we tried use the message on the image to extract but  it failed, then we overviewed the network capture file again and saw that besides the data transmitted over HTTP, there is also data was sent via `ICMP`. After that, we extracted that data and using `Cyperchef` to decode them.
<img width="1006" height="686" alt="image" src="https://github.com/user-attachments/assets/ee7f03d4-1990-4df4-83a1-b819703fde74" />

At first glance, it seems look like a garbage data, only one `ASCII` in each lines. But when we combined whole data then we got a message that looks like the password.

Password: `sneaky_network`

By using that passowrd, finally we can extract the hidden file which contain the flag.

Flag: `W1{1t's_n0t_that_hard_t0_solve_th1s_r1ght?_(*^_^*)}`
