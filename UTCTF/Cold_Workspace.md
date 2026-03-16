# Scenario
A workstation in the design lab crashed during an overnight maintenance window. By morning, a critical desktop artifact was gone and the user swore they never touched it. You only have a memory snapshot from shortly before reboot. Recover what was lost.

# solve
First, I used strings combined with grep command to preliminary check this memory snapshot with `flag` keyword
<img width="1350" height="121" alt="Image" src="https://github.com/user-attachments/assets/16d9447d-3880-4ae0-9df3-7efce9583f5e" />

Focus on 4th line, a new file named flag.enc was created by use content from enviroment variable `$env:ENCD`. Then I use grep again but now the keyword is `ENCD` and `-A 5` parameter(because I don't want to miss some important information after this)
```bash
strings -a cold-workspace.dmp | grep -iA 5 "ENCD"
```
