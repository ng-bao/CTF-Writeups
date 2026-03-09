# Scenario
I received a scam call and I think I was lost something!!!

# Solve
We have a pcap file containing multiple captured packets using the RTP protocol. The Real-time Transport Protocol (RTP) is a network protocol designed for delivering audio and video over IP networks. It is widely used in communication and entertainment systems that involve streaming media, such as telephony, video conferencing, and television services. Therefor, I used the RTP Player under the Telephony menu to play back the conversation. And this is content in the conversation.
>  Xin chào, vui lòng truy cập vào liên kết này: 192.168.100.82:8000 và tải xuống tệp có tên runthis.exe sau đó chạy tệp đó nhập 16 số 1 vào command prompt rồi nhấn enter.

Based on the scenario, I guess that this file is highly likely to be malware, so I will reverse to analyze the behavious of this file. First, I use DIE(detect it easy) to determine the file's true identity before performing a deep dive.
<img width="567" height="233" alt="image" src="https://github.com/user-attachments/assets/b03791ba-bc68-46ad-b6ac-57fdde7028d2" />

runthis.exe was wrote by python language and was packed by pyinstaller. Then I used pyinstxtractor to extract it. After extracting with pyinstxtractor, I obtained the main malware executable as Python bytecode in the form of a.pyc. However, the .pyc file cannot be read directly, so I use the strings command to perform a preliminary check for sensitive information within the .pyc file.
<img width="368" height="1033" alt="image" src="https://github.com/user-attachments/assets/2199b36f-4670-4cb0-b357-02d273b89189" />

in this image, we can see some keywords like `encrypt_aes_cbc`, `172.27.59.132`. `Enter what you was hearing:`, `b64encode` and some socket Parameters like `AF_INET`, `SOCK_STREAM`. I think that mean this malware will do something on victim machine and send data back to the attacker machine. Following the ip address, I applied the `ip.addr == 172.27.59.132` filter and found some thing interested here.
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/6c5a2933-074e-43a8-9633-465cf45a82e1" />

Look at the hex in packet 9623, we can see the text `some_thing_you_need.txt`. Then I follow TCP stream this packet and found this.
<img width="1194" height="54" alt="image" src="https://github.com/user-attachments/assets/836a73d1-80f9-44b0-b76f-952302543cbd" />

Because of the `b64encode` keyword, I think before send the data back to the attacker machine, it has been converted to Base64. From there, I will use the cyberchef tool to try decode this data that attacker was get. Based on my guess, I used the `From Base64` recipe is the first to convert it to raw data format, then I used the `AES Decrypt` recipe but I have a problem, where can I get the key. 
<img width="325" height="161" alt="image" src="https://github.com/user-attachments/assets/023843f4-00f5-44ae-b31c-e7d683719abc" />

When I see this image, I remember the call conservation because the AES-128 algorithms has the size of key is 16 bytes. It matches the character length provided in the scam call so I used a string of sixteen 1s for the key, with IV, I enter the same with key, and the mode I choosed is CBC(based on the `encrypt_aes_cbc` clue). After decode, we have a string is `2b5xMetBcZfFfFwz4OjAJr2ZIpzPzF7qDyvU9il29D3uzTCKdMypHnqTBmZr43Wyj`. I think this string is highly likly to be a flag, then I tried to convert this data to raw form by using recipt in the data format menu.
Finlly I found this data is a base62 and after convert I got the flag.
<img width="1511" height="986" alt="image" src="https://github.com/user-attachments/assets/3d9840d4-b621-485e-97b7-b6860b39268f" />

> W1{udp_pr0t0col_1s_re4lly_unc0mfort4bl3_r1gh7??}
