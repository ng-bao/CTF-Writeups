# Description
Author: s3asick5

Deep inside the forbidden library, two workstations were constantly communicating with each other in secret. At some point, we managed to intercept their network traffic. Can you analyze the captured communication and uncover the hidden data these two machines were trying to keep locked away?

Attachments: [forensics_locked_girl.zip](https://ctf.uithacking.club/cf6cbbad-38c8-4f3b-bddf-fde272f335c7)

# Recommend Tools
* Wireshark
* Tshark
* Cyperchef
* dnspy.exe
* Code editor
  
# Solve
We were given two files, `challenge.pcapng` and `sslkey.log`. Starting with the `pcapng` file, we can see `http` streams have been encrypted by the `TLS` 
> Transport Layer Security, or TLS, is a widely adopted security protocol designed to facilitate privacy and data security for communications over the Internet. A primary use case of TLS is encrypting the communication between web applications and servers, such as web browsers loading a website.

Then we used `sslkey.log` giving before to decrypt conversation. After decrypting, we can see that the machine `192.168.247.152` is trying to download a `zip` file at packet `600`.
<img width="1280" height="775" alt="image" src="https://github.com/user-attachments/assets/bbc7f6ed-86f8-4a14-bcec-0fbd8183c297" />

Exporting that file we got `C#` source code and `instruction.txt`.

<img width="559" height="99" alt="image" src="https://github.com/user-attachments/assets/3e7517ea-1134-43e6-bd3a-06324f0476c4" />

By opening that text file, we can see the `ip address` and `port` which the client will contact, then we using the `filter` on `Wireshark` to explore but the conversation has been encrypted so we can't read it. About the `C#` source code,  it show us the algorithm to encrypted messages.

We will start with the `main` function first.

<img width="670" height="252" alt="image" src="https://github.com/user-attachments/assets/ef9e2f9f-b232-48ee-b78e-6944a2b2b85d" />

From line `38` at `main` function, we can see how the two machines exchange the `session key`. First, the client send random 8 bytes contained in `array` to server and the server will also send back random 8 bytes containing in `array2`. After that, they will `xor` 2 arrays together and assign to `array3`.
