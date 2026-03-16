# Scenario
A workstation in the design lab crashed during an overnight maintenance window. By morning, a critical desktop artifact was gone and the user swore they never touched it. You only have a memory snapshot from shortly before reboot. Recover what was lost.

# solve
First, I used strings combined with grep command to preliminarily check this memory snapshot with `flag` keyword
<img width="1350" height="121" alt="Image" src="https://github.com/user-attachments/assets/16d9447d-3880-4ae0-9df3-7efce9583f5e" />

Focus on 4th line, a new file named flag.enc was created using the content of the environment variable `$env:ENCD`. Then I use grep again but now the keyword is `ENCD` and the `-A 5` parameter(because I don't want to miss some important information after this)
```bash
strings -a cold-workspace.dmp | grep -iA 5 "ENCD"
```
<img width="1893" height="530" alt="Image" src="https://github.com/user-attachments/assets/bdaf6a00-8592-4754-a28f-795de0c1eba0" />

The first three lines indicate that the ENCD value is base64, ENCK value is key and ENCV is IV. They look like the AES algorithm. So I tried base64-decode the ENCD value and use ENCK and ENCV as the key and IV in AES decryption, respectively. 
<img width="1499" height="669" alt="Image" src="https://github.com/user-attachments/assets/8687b0b9-0775-4fe3-afb5-ce42551af51e" />

> FLAG:utflag{m3m0ry_r3t41ns_wh4t_d1sk_l053s}
