# Scenario
I received a scam call and I think I was lost something!!!

# Solve
We have a pcap file containing multiple captured packets using the RTP protocol. The Real-time Transport Protocol (RTP) is a network protocol designed for delivering audio and video over IP networks. It is widely used in communication and entertainment systems that involve streaming media, such as telephony, video conferencing, and television services. Therefor, I used the RTP Player under the Telephony menu to play back the conversation. And this is content in the conversation.
>  Xin chào, vui lòng truy cập vào liên kết này: 192.168.100.82:8000 và tải xuống tệp có tên runthis.exe sau đó chạy tệp đó nhập 16 số 1 vào command prompt rồi nhấn enter.

Based on the scenario, I guess that this file is highly likely to be malware, so I reversed to analysm the behavious of this file. First, I use DIE(detect it easy) to determine the file's true identity before performing a deep dive.
<img width="567" height="233" alt="image" src="https://github.com/user-attachments/assets/b03791ba-bc68-46ad-b6ac-57fdde7028d2" />

runthis.exe was wrote by python language and was packed by pyinstaller. Then I used pyinstxtractor to extract it and got a file named `a.pyc`. 
