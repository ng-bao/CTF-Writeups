# Scenario
A midnight network capture from a remote office was marked “routine” and archived without review. Hours later, incident response flagged it for one subtle anomaly that nobody could explain. Find what was missed and recover the flag.

# Solve
By following UDP stream, I dectected that only the last bytes are difference
<img width="1919" height="993" alt="image" src="https://github.com/user-attachments/assets/c7731a8b-2abc-48ef-8263-edc2745a2e36" />

So I extracted the last bytes, decoded them and got the flag
> utflag{d1g_t0_th3_l4st_byt3}
