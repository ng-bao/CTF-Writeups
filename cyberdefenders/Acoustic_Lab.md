# Description
<img width="646" height="490" alt="image" src="https://github.com/user-attachments/assets/1aedae49-c7f6-42ff-93a6-f103cf9593f8" />

# Solve

## Question 1: What is the transport protocol being used?
By checking any `RTP` packets, we can see which transport protocol was used

<img width="1280" height="383" alt="image" src="https://github.com/user-attachments/assets/ce2aff98-ae75-487c-a476-68f366f4ca19" />

Answer: `UDP`

## Question 2: The attackers used a bunch of scanning tools that belong to the same suite. Provide the name of the suite.
In `log.txt`, we can see that almost all requests have the `User-Agent` field set to `friendly-scanner`. Looking this up online reveals that this User-Agent belongs to the `SIPVicious` suite.
<img width="1119" height="580" alt="image" src="https://github.com/user-attachments/assets/cb783376-00c2-4590-ad4c-2ef57ea814f6" />

Answer: `SIPVicious`

## Question 3: What is the User-Agent of the victim system?
the first two packets in the `pcap`file that we were given are belong to the `SIP` protocol. The machine `172.25.105.43` sent a request to check the status of the victim machine that is `172.25.105.40` and the victim sent back a packet to verify the status, in this packet we can find the `user-agent` of the victim machine.

<img width="533" height="323" alt="image" src="https://github.com/user-attachments/assets/ecac8ca2-57f2-42b5-9eae-e97bfa909480" />

Answer: `Asterisk PBX 1.6.0.10-FONCORE-r40`

## Question 4: Which tool was only used against the following extensions: 100,101,102,103, and 111?
With these extensions, we can see each of them has `REGISTER` requests with `Authorization` information except extension 100. It looks like a password `brute-force` attack, indicating that the attackers used `svcrack.py` in this situation. As for why extension 100 lacks `Authorization` packets, we suspect this extension does not require a password, so the tool didn`t send any Authorization packets.

Answer: `svcrack.py`

## Question 5: Which extension on the honeypot does NOT require authentication?
As mentioned in Q4, extension 100 is the only one didn't has `Authorization` packets when it was `brute-forced` by `svcrack`.

Answer: `100`

## Question 6: How many extensions were scanned in total?
We used this command to count how many extensions were scanned in total

<img width="503" height="50" alt="image" src="https://github.com/user-attachments/assets/7082d03b-e8d4-4e72-9d54-d663db5c2c75" />

Answer:`2652`

## Question 7: There is a trace for a real SIP client. What is the corresponding user-agent? (two words, once space in between)
Beside the `User-Agent: friendly-scanner` belongs to `SIPvicios` suite, we also have another `User-Agent` which is real SIP client.
<img width="499" height="65" alt="image" src="https://github.com/user-attachments/assets/4f9f77e1-8b67-4c33-94f3-920b412c6155" />

Answer: `Zoiper rev.6751`

## Questiom 8: Multiple real-world phone numbers were dialed. What was the most recent 11-digit number dialed from extension 101?
We can find the most recent number dialed by determining the most recent `INVITE` packet was sent by extension 101 at `2010-05-05 10:00:46.147670`.

<img width="583" height="433" alt="image" src="https://github.com/user-attachments/assets/9b9080c6-c3c9-452e-9b63-f711bd0f875d" />

Answer: `00112524021`

## Question 9: What are the default credentials used in the attempted basic authentication? (format is username:password)
<img width="814" height="164" alt="image" src="https://github.com/user-attachments/assets/fa27a207-d6cb-46ac-958a-b6d078a22dcf" />

The attackers initially attempted to access the `/maint` page but was blocked by a password prompt. They then navigated back to the base IP address and were automatically redirected to users (packets `16`, `18`, `26`, `39`). After another failed attempt without credentials (packets `50`, `52`), the attackers retried with a password and successfully gained access, as seen in packets `60`, `62`, `71`, and `92`.
<img width="637" height="392" alt="image" src="https://github.com/user-attachments/assets/c215ed37-e6bc-4d5e-b219-cad6260d971d" />

Answer: `maint:password`
## Question 10: Which codec does the RTP stream use? (3 words, 2 spaces in between)
Just checking `RTP` packet. We can see codec type in `PT`(Payload Type) field. 
<img width="746" height="15" alt="image" src="https://github.com/user-attachments/assets/151ca8c7-14bf-432d-b9c7-4fee79ccabad" />

Answer: `ITU-T G.711 PCMU`

## Question 11: 

## Question 12: What was the password for the account with username 555?
Continuing from our analysis of the `Q9`, after the attackers accessed successfully into the `/maint` page.
<img width="952" height="169" alt="image" src="https://github.com/user-attachments/assets/91a59051-996f-42ed-a433-f183a59e3ff8" />

At packets `1208` and `1211` refer the attackers sent a `POST` request to activate `configEdit` module and a `GET` request to load administrator interface. After having permission to edit configuration file, they processed exploitation to `sip_custom.conf` as we can see in packet `1279` and server responded with `200 OK`.

<img width="521" height="208" alt="image" src="https://github.com/user-attachments/assets/f06dedaf-70ac-4e6a-8cc7-abc2ffe4b391" />


Passwords of extensions `555` and `556` were contained in there and they both are `1234`.

Answer: `1234`

## Question 13: Which RTP packet header field can be used to reorder out of sync RTP packets in the correct sequence?
<img width="805" height="21" alt="image" src="https://github.com/user-attachments/assets/e07522c6-3605-4359-aa7e-520ced71e86d" />

In any `RTP` packet like this one in image, after `Payload Type` field we have 3 ways to synchronization. The `SSRC`(Synchronization Source) field helps us identify packets originating from an unknown source; The `Seq`(Sequence Number) field is used to detect packet loss and to restore packet sequence; Finally, with the `Time`(Timestamp) field, we can achieve precise timing synchronization for media delivery, ensuring that packet sampling instants are preserved regardless of transmission order.

So the field be responsible for reorder out of sync `RTP` packets is `Time`.

Answer: `Timestamp`

## Question 14: The trace includes a secret hidden message. Can you hear it?
<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/fe4e7a6f-51fa-4c64-9ed4-e5194937e20f" />

<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/e02d674a-65bf-4287-9e1f-bd35fe7a3d8f" />

By this way, we can hear the secret message at nearly the end of audio.

Answer:`Mexico`
