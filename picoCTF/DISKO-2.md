<img width="918" height="571" alt="image" src="https://github.com/user-attachments/assets/4b4f15fb-c4f6-4356-90ab-933522b99e28" />

---

you have a disk image and need to find the flag in it. First, I tried to use strings and grep to find the flag then saw lots of fake flags. Then I used mmls to show partition table, i saw two partitions.
> <img width="1072" height="269" alt="image" src="https://github.com/user-attachments/assets/1de2987f-34d6-4f24-a8fd-99100c970751" />

following the description, I think the right one is the linux partition. I used the dd command to extract the linux partition 
```bash
dd if=disko-2.dd of=linux_p.img bs=512 skip=2048 count=51200
```
Then I used strings and grep on the linux partition and found the flag.

> picoCTF{4_P4Rt_1t_i5_a93c3ba0}

