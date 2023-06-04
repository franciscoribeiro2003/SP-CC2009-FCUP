# CTF NumberStation3

### Passo 1
Fizemos a conexão ao server com `nc ctf-sp.dcc.fc.up.pt 6002`:

O que nos deu a mensagem:

![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/241eb035-3245-4cc1-9bfc-92bdf8bb418d)


### Passo 2
Depois analisamos o programa `challenge.py` dado e decidimos fazer umas alterações para ir ao encontro do que precisamos:

Em vez de abrir diretamente o arquivo flag.txt e criptografar seu conteúdo, optamos por usar uma string já criptografada chamada c_shell que continha o que nos foi dado no **passo 1**.

Essa string representava a flag criptografada em hexadecimal.

Ao criar um loop com range(2 ** 16), percorremos todas as possíveis combinações de chaves geradas pela função `gen()`.

Em cada iteração do loop, gerávamos uma nova chave `k2` e a usávamos para tentar descriptografar a string `c_shell`.

Com isto o programa modificado é o seguinte

```python
# Módulo Python ciphersuite
import os
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
from binascii import unhexlify

FLAG_FILE = '/flags/flag.txt'

# Use a geração de números aleatórios criptográficos para obter uma chave com comprimento n
def gen():
    rkey = bytearray(os.urandom(16))
    for i in range(16):
        rkey[i] = rkey[i] & 1
    return bytes(rkey)


# Operação de descriptografia
def dec(k, c):
    assert len(c) % 16 == 0
    cipher = Cipher(algorithms.AES(k), modes.ECB(), default_backend())
    decryptor = cipher.decryptor()
    blocks = len(c) // 16
    msg = b""
    for i in range(0, blocks):
        msg += decryptor.update(c[i * 16:(i + 1) * 16])
        msg = msg[:-15]
    msg += decryptor.finalize()
    return msg


c_shell = b"a848c06fd02b96c86a8f1a8c66c346bbc36d1f5a8433065a387ffe7de7058c115ea1cbf1f77f6128e44ad240b75dcdacc2fd2b20dcc7ccbca8e9ae0a561cb0262fce82dc8940fbb57f72cc2ccd6f395426f1d9393d5b3af8b6c706a146e7ffdfe840e039d07c7f755027f3867fb7214d26f1d9393d5b3af8b6c706a146e7ffdf6629dea0c13303fd57f9aa00854262706629dea0c13303fd57f9aa0085426270a848c06fd02b96c86a8f1a8c66c346bbe86579f9218acad464193e7698229b83e840e039d07c7f755027f3867fb7214de121093a1c1b7508e6870e008c854266e86579f9218acad464193e7698229b8326f1d9393d5b3af8b6c706a146e7ffdfe86579f9218acad464193e7698229b83e86579f9218acad464193e7698229b83a411a0bfbd97793e763453e173e7639177bf20a0d90efa412642f698f0e2ad78b1d297dd22b0250c1b350d6b968d492f703312fe86d50336d65ca6cfc4cd5c21703312fe86d50336d65ca6cfc4cd5c21cb1eef3bde5513a3b11654aafa5d58c8a411a0bfbd97793e763453e173e76391b1d297dd22b0250c1b350d6b968d492f8159cdd945f11cdd265a59b9cb1dbc3ea848c06fd02b96c86a8f1a8c66c346bbcb1eef3bde5513a3b11654aafa5d58c8a848c06fd02b96c86a8f1a8c66c346bbe840e039d07c7f755027f3867fb7214d703312fe86d50336d65ca6cfc4cd5c216629dea0c13303fd57f9aa0085426270e840e039d07c7f755027f3867fb7214d703312fe86d50336d65ca6cfc4cd5c2126f1d9393d5b3af8b6c706a146e7ffdf2e02d9f0b1b2205992786248997a3f10d911f5b88f0101736ac4d47a8fde7d9aa33dd0a238375a72a0aa829e5cd5915d"

# Loop para testar todas as combinações possíveis de chaves
for i in range(2 ** 16):
    # Gera uma nova chave aleatória
    k2 = gen()

    # Descriptografa a mensagem usando a chave atual
    decrypted_msg = dec(k2, unhexlify(c_shell)).decode('latin-1')

    # Verifica se os primeiros 4 caracteres da mensagem são "flag"
    if decrypted_msg[0:4] == "flag":
        print(decrypted_msg)
        break
     
```


### Passo 3
Executamos o programa e ele deu-nos a flag:

![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/d5957acb-0bb7-4a3e-a929-3fac1d686f3d)


```
flag{23244f930929981e5578e6f7f354352b}
```

