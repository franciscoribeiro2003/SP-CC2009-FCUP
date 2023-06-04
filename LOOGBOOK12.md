# Loogbook 12 "Public-Key Infrastructure (PKI) Lab"
## Set-up
Como usual, usamos os ficheiros de Labsetup, e então no diretório `../Crypto_PKI/Labsetup` abrimos o terminal e construimos o container:
|docker build && docher up|
| :-------------------: |
![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/91c5c926-cea3-43cf-8b40-2a7b5de21089)

Com os containers a funcionar, numa nova tab acedemos ao container:

|rootshell do container|
| :-------------------: |
![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/1d2ec016-4174-4570-9a10-58d542affbe9)

- Como o enunciado pediu, também mudamos o seguinte ficheiro `etc/hosts` para algo aleatório no que toca ao nome do server, neste caso usamos `www.tudopago.com`

|Mudar nome do server|
| :-------------------: |
| ![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/146c2835-fe1d-4a16-b766-28a3cfcb334f)|

---
# Task 1 "Becoming a Certificate Authority (CA)"
### Processo
- Primeiro começamos por copiar o ficheiro para o diretório atual `cp /usr/lib/ssl/openssl.cnf .`;
- Descomentamos a área que foi pedida:

| Unedited | Edited |
| :----------:|:--------:|
| ![Task1_unedited](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/2ca08533-8332-48b0-a16c-d4d9a1b5ffbb) | ![Task1_edited](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/4746ae56-4332-456c-8a4f-70f20feb30ab) |

- Como o enunciado pediu, também mudamos o seguinte ficheiro `etc/hosts` para algo aleatório no que toca ao nome do server, neste caso usamos `www.tudopago.com`

|Mudar nome do server|
| :-------------------: |
| ![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/146c2835-fe1d-4a16-b766-28a3cfcb334f)|


- Criamos também `indext.txt` vazio e `serial` com um número random lá dentro, usamos o **38**.

|index.txt & serial|
| :-------------------: |
| ![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/ee3f8604-5ea5-40b9-80d7-0fbb7c919718)|

Agora corremos o seguinte comando:
```shell
openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 -keyout ca.key -out ca.crt
```

|Criaçao do CA|
| :-------------------: |
|![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/d7f464d0-3a73-4ee7-b2f1-ab38e9806274)|


O que nos deu um certificado e uma chave, corremos `cat ca.crt` e `car ca.key`:


<details>
<summary>ca.crt:</summary>
  
  
```
-----BEGIN CERTIFICATE-----
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
fb2dDqcSBwTbuxDeGHdcjKt1xewm8PY5W+ZCTXc=
-----END CERTIFICATE-----
```

</details>

<details>
<summary>ca.key:</summary>
  
  
```
-----BEGIN ENCRYPTED PRIVATE KEY-----
MIIJnDBOBgkqhkiG9w0BBQ0wQTApBgkqhkiG9w0BBQwwHAQIvLEU9YscmYECAggA
MAwGCCqGSIb3DQIJBQAwFAYIKoZIhvcNAwcECBfi1zmYRq1OBIIJSNjtvxS4PaGS
hFKaT3DkOEq4kZxxQzgFRlRl3xL5tWuxnEegh+oS65sHJFV/l2Pek8ds/zb3DR1U
UFUNwl44+vvZi5XnBDZO+2bEah4h0MHaMJ+h5mZSharhH4IfNmzCXapvgJ4slk/h
4eiiySS5VzqdNcZEk/BGvEtn+oNO9skKWSW2INjQbtsO1dt7wMFchlEA+jorzogQ
slGP+k+b5LJWYJzOnwjwfqAt1h4TFg26ZcJTQvoNTuxhEM1unbFpM5XV+C3noWe8
OcykCS749xFnXo5kpa0aG92zTBafdi9twZnmZX86jsxtfcElA70JRRPcrQVaLlsm
fZafKigz7rbwHDV3Xta4KFpMP/dECu3WYsgOrG5Tqga+y2z61I3pCPK9Xgub2G5t
owYEBVC8Gvgp4n5hUphcKd9ekUwE/9pwdWbtGFMNbk2+RUzdgkNdiKkGqfihMy67
zB8/zb4bvRKsyaE/TKiUP4SzcANd7FzUWZ1t3yoCLXeC/TPQ+T6zAUz4aV+SdzRj
JK8kvYhVlIRwa2ul+GQYVXM3c0GSVHIjfl7X0qSTYwLyDhdGHJK4lSHFsB6llx6c
xls7cOS43b2g/dnwQTXpgZOS6yB8owkP39i1Fu6+vT0exuLiptLPNoaugrHGJ3+z
pRzrOilVgygplViMTwldRlji4wWMaHrWUySFl6R5SMtNNZsDHUotPv3bj8WBat5m
Tcy4etIoZvVSMmFCYDFRcHEW9GjSTzQRt7WzCT8qUWTicJ7SUF09zRL9Oi4Vv2Gn
nrFs3JrmYnGAQhTwCgEocyTHUhWJhwVeOMr4IfIDVnyeMiAOQN4l5BIcwaWru+RU
9MDtP3MUixaqJJEl124lS/VW1dg6yMZBXXbq+VpstQHFvmykK1FJ0XTwCQFE+LkG
szNzQFwNM/HkH2RSpi3Oqv/20jMiO6XI+hN5/t8T1OvkgxGn7U4Ae17lKToWixIA
LTERblIKw8fPTDW8Mj5mhNg37BRWzX2PvXgyG0lyJjS2jsZ2MDtwJZWN61J9x+dX
FgJ/EhAHJa5oB7EI2aXhxW8wWfGUBNzBAfgaPksdZvYZW742lv8aRN/KkqKrPG8v
fGO4YEukNwXzZGoH1hADIu5Sj218n84VRT+mZh1ulKcfk6FP+SR/kj26TY0We257
DkefRjOSY5TneF+kBsDisBa0QMShhpxNRYgQvq7eoMfQGkZ/IXDdfKin4Ba3t1Iu
v4SpZJE2DovxPHsOZ11ohPhnhMM7Bg2vdTstqkRUTWIXU4iOC9sYp+xSc8mtDtdN
9KGaUm8C0ebr9Wqd4+M4fAEFr7goDJp2Z+P4oapbC5s7L5t8SikMhxt8OjLxNUKH
TikQ2z77KCxcqBkTvGN3sEmZDic3fbq9Ae8Hj8XYbCc/tFt7BMXa+e2KDC/BJnnZ
gZSvhlXXVglnQJ02lFtIbnXQu6KnzQ9RqS3WnQTHodRrrQtTyn9fEh2ZUYQ1RV/8
GIvPs4vcrheH2ivoy1Sbytr4glY5hkw4Gh801U4VsM+Nnk1GEKJJOmf/fgf4tqYJ
tL54F+z58PLl58dgi/x3J83saXLFBBLf+5bkR6TFTEDQbhWv1aa4JVc6O848tP86
FF3HozHWaOBwSp+6SjgJ7ogc7uo1hKY7eIhvJF2Y/94OUYyJ7lRNPskDJSZtCBCe
nSHo5B8c26vxKYb4Ptwt9WnI2/OCGvOX0Tu9jTopjYE2M6s2kxOKMMj2RuDQFRe3
oa/03K79SgtEbgIGJov3hhd0P3wk/q72/g0IJyBN6DsQ909cal7RATYX3nsu+NNp
wxMl5IBNfV/xah1s+fW7jfjWIHmUc61+7k/sZtUHZA0e9hFvreF2dpcGp2NR/fNC
KtqmBZA6wnsYJ0VR40XacF3uT39QUH+NSCjCckNPAo6v3KQ4z2ixn0a+kytOK2Lz
rjCyz6sfJACJBOvbA7mEF8ag8Vi/UZ641S0L4EJpTzNJhy0akdmfuBu7QggfpDDt
jq+brdngBmihhPfASDcMs3GBLIZwgArnGWguYqOGJdRp/lx79MpA6ykHy1m6CnYj
HtfQ2Atk4MC1BasLgr9SUjaqg6WMDpgkMPCq8/bMZezQh5DtqjipQuKE35n6pbD0
f4AtQ5xj8xsVxkL5sp9yGirmi21i0J8PpZtHFjkQhsM/dd7I9svPMvkjjUz4t3AV
o+Ty6wTdlLw1+SGFey8ZRMFkP2k1sL7zxoouHoIEuFnZIaJdKK8373eVp7Fildlz
DDtt0m2Kfl9CMKJHilmQXzt6KFWrjmCn0LQ6I4aVxFLFcHGArT2bq5I9mv69lwWW
TiBkRd/Qru2FOgVpdDEROUkAFDPs6QPPa0UrPEOCaGqE/VgoaJZ3R2XjOlGsiy6Z
/Q0Sk8bKd4vYNsVA7DYmAKBpO3JUGCNOipfPUvIrurPAdTjTUeGrLPEyvBaote81
zqsVKURu3uhmqhcYsVMx+aweNhMaVbEs5dgU9ZmX3uP6y8Q3IFxXxNjsYoBzZoGc
FHbByTXg2/7ErYL3N5Ls+b+nEwWfi+eRw+CrD4Kp/iF7Ap4KHCDgwnOwwM8TXbUe
SwIa5t5iGATZU++7jDaIiTp7N2NJywhAzgtjU/m/iwoYrMs5Bx6hW3V/F2Pm79ut
VJFwBQtweZH3rVtbaoJaiUk6Jr7e7BedG4iw0sCocyyLZRayqnz3f3VqF3K+eZNb
a0jy8bjpjXmwcqMnMAmEVLBU3iwxSjgq2hJ56k3+MdLIUf9rbCGKbwjRdVtH7tAK
C9H2ilD+W64IfbZ4hc14h6Ij/jq6voxZXrKEuILHd211jMexasrJNOjOVmPWbDGc
IYz4DpRS0mpJZA0FRnskHt5UC7nGLKYLOPxhMDd1NPHyyhYIL8SKPJ83/db/tUb6
4ZectW7Pv0vt4yO1HFydrcKLaVYiQdlc2oM6Ho/1tcoBoyRfZmeWBmCI9hRdLKTh
SXIevOk1BbzCGfPgN6pVYUdNGtlIXW6avHLDRVY4EJz1gWyiowlMv+SjXbdxCvWN
4/b75SfPOl8Yki6up6vIebwxcGE+YgnKsGMLOCjP6IX6TL+8zOTWoQaT68zu7dkJ
23JsE5lbszTO6cSL8yxATIza4eE/7lQRQJl0MLs1e7XY7xRCtnMqWv/feLFj2dQ0
8i/K2xX3RwlsIk6X/+KO6w==
-----END ENCRYPTED PRIVATE KEY-----
```
</details>

### Resultados:

<details>
<summary>`openssl x509 -in ca.crt -text -noout`</summary>

```shell
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            51:f2:b5:cc:86:cb:6b:68:2c:eb:c1:b9:84:2c:b6:39:2e:d0:2e:3c
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C = PT, ST = Vale de Cambra, L = Vila Ch\C3\83\C2\A3, O = Universidade do Porto, OU = DCC, CN = FCUP, emailAddress = up191200000@up.pt
        Validity
            Not Before: Jun  3 02:10:30 2023 GMT
            Not After : May 31 02:10:30 2033 GMT
        Subject: C = PT, ST = Vale de Cambra, L = Vila Ch\C3\83\C2\A3, O = Universidade do Porto, OU = DCC, CN = FCUP, emailAddress = up191200000@up.pt
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                RSA Public-Key: (4096 bit)
                Modulus:
                    00:9a:97:fc:21:83:16:a4:f4:b5:79:7e:bb:55:a4:
                    38:44:c6:55:ca:43:87:d9:a8:18:72:ba:c3:8f:d8:
                    23:c9:d1:89:ed:6e:81:40:68:06:9e:08:6d:55:3a:
                    8c:72:07:c9:52:2b:40:9d:c7:51:91:b5:3a:70:27:
                    c9:41:4d:30:44:fe:72:b6:3f:da:7f:79:20:f2:ca:
                    cb:83:91:5b:c9:d8:72:b8:0e:09:9d:13:2c:a5:c4:
                    17:a2:77:eb:27:af:3f:21:ac:d1:41:2b:f5:7a:2c:
                    97:5a:ec:63:54:9d:18:ca:f2:a7:39:36:05:4f:c5:
                    ce:f9:39:41:a0:f7:39:c1:c9:5e:dd:15:fd:58:36:
                    91:5f:94:43:6b:29:a3:02:cb:27:c1:8d:38:e6:dd:
                    18:95:f3:58:44:6e:18:fe:7d:dd:a6:eb:f7:99:13:
                    72:c8:55:02:5c:e0:8f:89:96:af:8c:d0:cc:bb:9f:
                    f1:d0:d3:65:17:95:db:6b:f0:7a:28:a4:a8:51:86:
                    fe:b7:3c:11:32:9f:01:9b:ab:86:1f:55:ad:02:81:
                    0b:84:24:d7:8d:a6:0e:40:21:8c:b0:d4:f2:ba:70:
                    63:32:07:ae:f8:16:8b:e1:f3:94:72:43:f3:38:e7:
                    81:6e:61:cb:38:6f:12:19:df:87:c5:fc:07:38:86:
                    ec:fb:6d:e2:d1:1f:19:ad:3a:9f:c7:fc:1b:16:de:
                    0d:4f:ec:25:29:6a:ff:95:af:8b:c1:3c:13:e5:d6:
                    39:e3:19:1a:32:62:6f:c7:6e:0a:04:d7:fd:c6:98:
                    a6:50:8a:f3:1e:67:d9:e5:cd:0c:12:65:f1:76:92:
                    b7:39:e4:f9:9e:04:60:e7:e4:c7:ab:4c:f3:f7:2c:
                    aa:02:fa:67:90:c5:6b:3b:ae:f2:78:df:09:9e:2e:
                    0a:ba:c0:9d:7c:21:4a:33:05:a0:fd:85:d2:0c:9f:
                    6e:dd:e7:38:35:6c:46:9d:67:7b:41:5b:db:1c:2e:
                    a5:c9:e7:18:6b:fb:08:5d:5b:27:4a:a1:4a:ec:2a:
                    18:e9:fc:58:62:eb:3b:ed:04:d4:a3:cb:26:a8:4d:
                    ca:7d:fe:b8:b8:d2:f5:86:73:1a:54:2e:0b:10:f5:
                    18:a2:c9:7a:5c:e3:79:e5:41:a7:7a:9f:17:84:30:
                    d6:0a:4c:8c:bc:67:d5:e0:26:b3:f8:c7:af:19:ef:
                    f6:33:3c:f2:a2:8f:0d:63:f1:2e:9c:12:60:44:29:
                    2d:ac:aa:43:77:5b:60:5c:36:1c:2e:44:b6:33:7e:
                    5b:81:81:25:d1:2c:32:4f:05:0f:71:4e:13:30:b2:
                    71:7d:35:83:95:e9:33:9c:06:7f:a7:44:c9:2c:8e:
                    2e:53:4f
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Subject Key Identifier: 
                73:F2:6C:83:94:99:3B:07:A8:35:41:AF:D1:BB:0A:95:C6:A5:24:0F
            X509v3 Authority Key Identifier: 
                keyid:73:F2:6C:83:94:99:3B:07:A8:35:41:AF:D1:BB:0A:95:C6:A5:24:0F

            X509v3 Basic Constraints: critical
                CA:TRUE
    Signature Algorithm: sha256WithRSAEncryption
         91:45:7d:9e:8e:06:17:1e:ce:c4:ce:45:95:a4:6e:e5:7b:08:
         ff:6c:f6:8c:46:42:11:fd:be:b7:0a:74:4d:57:52:4e:bd:f0:
         eb:2d:81:e9:27:72:5f:35:04:e7:f7:42:bc:72:da:87:12:98:
         93:e0:9b:18:45:d2:65:3c:6b:be:36:36:7d:5b:ec:17:28:76:
         70:7c:1a:7e:85:54:bf:97:79:26:e8:49:02:b6:2d:9a:b0:14:
         4e:8f:69:a4:2c:43:84:f9:51:2a:98:bd:42:23:e4:2f:d7:b0:
         2d:f6:f5:52:74:e7:05:c0:13:23:2c:95:41:bd:f9:50:79:d6:
         06:f8:89:59:dd:29:73:e3:5e:38:05:cb:3d:a3:f4:80:31:3f:
         f8:cc:cf:dd:b2:62:6b:fc:aa:eb:1d:6e:ba:95:19:b2:a2:75:
         2b:52:f4:6d:05:dc:87:65:9a:01:91:2e:9c:cb:0b:3b:a3:d4:
         48:97:02:4d:29:f6:42:b8:10:91:29:75:6b:40:5c:0f:dd:d9:
         ab:4c:97:77:a0:5b:87:50:11:7c:82:7d:b9:7e:59:4d:81:d5:
         65:7d:f4:7d:bc:36:eb:35:7e:a6:4e:ae:41:63:8e:4e:dd:57:
         8c:07:8f:a6:27:c6:4a:af:ca:ac:9c:4f:19:65:d6:92:0a:72:
         5d:0d:f8:e6:39:5a:ef:69:5a:4a:1a:fb:2a:a6:8c:6a:88:82:
         6e:d0:12:72:70:fe:23:0c:f8:0d:6c:7d:18:f5:4a:ca:7d:15:
         26:bd:35:86:74:a5:93:9a:28:06:9a:fc:fa:c4:38:b8:ff:90:
         66:b1:06:a1:4e:82:27:48:ea:b2:7a:0b:f7:63:1e:28:71:7b:
         7a:d2:f9:6e:82:b6:37:43:47:4f:b5:cf:59:29:3b:6d:73:8c:
         c2:d4:15:4a:28:fd:e2:ff:b3:b3:14:71:53:f0:89:43:79:c4:
         4b:b0:42:c8:b2:46:c8:27:d1:ac:b8:84:35:d6:76:e6:60:06:
         12:08:db:00:58:dd:b9:70:9f:10:00:e2:ea:ad:86:a3:36:c7:
         3f:50:3c:7e:17:46:0c:f1:24:ab:ad:c4:d1:e4:f5:fe:5f:e0:
         bb:1a:1c:bd:2a:e3:aa:2a:3c:49:84:c1:fe:0f:d4:60:70:d7:
         ef:61:ac:f8:d3:18:81:4c:61:fe:06:5e:10:b1:2a:a1:7e:60:
         35:d7:b5:35:59:a9:be:b3:fb:8f:d7:82:5e:b3:9e:29:ca:18:
         e5:fe:ad:6d:d1:1e:d8:1e:d7:85:d9:d2:fb:2d:3a:7d:bd:9d:
         0e:a7:12:07:04:db:bb:10:de:18:77:5c:8c:ab:75:c5:ec:26:
         f0:f6:39:5b:e6:42:4d:77
```
</details>



<details>
<summary>`openssl rsa  -in ca.key -text -noout`</summary>

```shell
Enter pass phrase for ca.key:
RSA Private-Key: (4096 bit, 2 primes)
modulus:
    00:9a:97:fc:21:83:16:a4:f4:b5:79:7e:bb:55:a4:
    38:44:c6:55:ca:43:87:d9:a8:18:72:ba:c3:8f:d8:
    23:c9:d1:89:ed:6e:81:40:68:06:9e:08:6d:55:3a:
    8c:72:07:c9:52:2b:40:9d:c7:51:91:b5:3a:70:27:
    c9:41:4d:30:44:fe:72:b6:3f:da:7f:79:20:f2:ca:
    cb:83:91:5b:c9:d8:72:b8:0e:09:9d:13:2c:a5:c4:
    17:a2:77:eb:27:af:3f:21:ac:d1:41:2b:f5:7a:2c:
    97:5a:ec:63:54:9d:18:ca:f2:a7:39:36:05:4f:c5:
    ce:f9:39:41:a0:f7:39:c1:c9:5e:dd:15:fd:58:36:
    91:5f:94:43:6b:29:a3:02:cb:27:c1:8d:38:e6:dd:
    18:95:f3:58:44:6e:18:fe:7d:dd:a6:eb:f7:99:13:
    72:c8:55:02:5c:e0:8f:89:96:af:8c:d0:cc:bb:9f:
    f1:d0:d3:65:17:95:db:6b:f0:7a:28:a4:a8:51:86:
    fe:b7:3c:11:32:9f:01:9b:ab:86:1f:55:ad:02:81:
    0b:84:24:d7:8d:a6:0e:40:21:8c:b0:d4:f2:ba:70:
    63:32:07:ae:f8:16:8b:e1:f3:94:72:43:f3:38:e7:
    81:6e:61:cb:38:6f:12:19:df:87:c5:fc:07:38:86:
    ec:fb:6d:e2:d1:1f:19:ad:3a:9f:c7:fc:1b:16:de:
    0d:4f:ec:25:29:6a:ff:95:af:8b:c1:3c:13:e5:d6:
    39:e3:19:1a:32:62:6f:c7:6e:0a:04:d7:fd:c6:98:
    a6:50:8a:f3:1e:67:d9:e5:cd:0c:12:65:f1:76:92:
    b7:39:e4:f9:9e:04:60:e7:e4:c7:ab:4c:f3:f7:2c:
    aa:02:fa:67:90:c5:6b:3b:ae:f2:78:df:09:9e:2e:
    0a:ba:c0:9d:7c:21:4a:33:05:a0:fd:85:d2:0c:9f:
    6e:dd:e7:38:35:6c:46:9d:67:7b:41:5b:db:1c:2e:
    a5:c9:e7:18:6b:fb:08:5d:5b:27:4a:a1:4a:ec:2a:
    18:e9:fc:58:62:eb:3b:ed:04:d4:a3:cb:26:a8:4d:
    ca:7d:fe:b8:b8:d2:f5:86:73:1a:54:2e:0b:10:f5:
    18:a2:c9:7a:5c:e3:79:e5:41:a7:7a:9f:17:84:30:
    d6:0a:4c:8c:bc:67:d5:e0:26:b3:f8:c7:af:19:ef:
    f6:33:3c:f2:a2:8f:0d:63:f1:2e:9c:12:60:44:29:
    2d:ac:aa:43:77:5b:60:5c:36:1c:2e:44:b6:33:7e:
    5b:81:81:25:d1:2c:32:4f:05:0f:71:4e:13:30:b2:
    71:7d:35:83:95:e9:33:9c:06:7f:a7:44:c9:2c:8e:
    2e:53:4f
publicExponent: 65537 (0x10001)
privateExponent:
    63:c4:c8:63:fc:4c:c5:e2:63:a7:e8:21:10:51:2e:
    3b:3c:60:ab:6b:4f:a6:29:74:b5:be:50:6d:69:c9:
    16:fb:52:ef:57:4c:b4:fb:2d:a2:19:c0:2d:ab:de:
    6d:cd:16:a1:1f:e6:d0:ac:8c:b8:e3:63:b6:74:06:
    cf:4c:f8:64:de:6d:bb:ae:93:dd:86:97:bb:f9:22:
    c4:63:30:00:a0:de:d0:67:c6:ce:87:4c:5e:22:a2:
    3a:28:f8:2b:21:4f:35:f5:69:0a:8d:4b:1c:30:e0:
    f1:3d:f3:90:ec:dd:ce:44:31:a5:9b:76:6c:18:35:
    cd:e3:a4:b6:34:37:23:18:49:4f:97:5c:6b:ec:b3:
    7f:22:99:2b:f4:b0:0e:22:7c:22:ce:78:35:8d:e4:
    cb:09:44:22:cd:86:f5:de:d7:08:5d:ea:e9:f0:82:
    38:45:9c:83:c2:2d:00:57:ae:2d:cb:a7:05:43:60:
    f5:2b:28:67:c3:3f:db:76:53:f0:1c:eb:bf:c9:c6:
    63:0c:b1:f0:a0:6b:92:42:6d:09:95:ea:54:de:61:
    47:43:db:94:bf:e1:1c:84:4a:5e:e8:df:49:71:34:
    a3:5a:10:12:6d:c2:92:ec:f7:15:93:42:c9:ab:a6:
    7d:4d:41:b0:c7:05:c0:8a:e0:fd:e5:fb:e7:00:34:
    b9:c2:33:90:ca:c4:5c:8a:8e:e4:42:cf:9d:a8:c0:
    db:ba:51:27:33:3e:de:ff:36:ba:88:8c:80:89:2a:
    d2:12:ef:3e:f0:4e:e2:c7:d3:59:09:30:d5:b0:ca:
    e0:71:47:53:b5:93:a5:0f:2b:49:44:d6:7c:8c:84:
    ce:5d:47:75:73:62:1d:ed:1b:5e:8c:fc:ce:8c:c1:
    9c:7e:e4:65:58:0d:22:92:63:17:53:e1:a1:61:b8:
    ff:ca:fc:03:55:18:82:59:c8:59:f8:a8:65:0e:49:
    21:00:45:12:64:09:d1:cb:5e:a3:65:a3:50:17:09:
    50:72:56:3c:9b:44:6c:61:85:de:39:cd:79:c8:24:
    58:10:e6:ba:de:ad:71:ad:31:ff:00:b0:92:9d:ee:
    8a:54:35:f1:84:e6:19:63:f1:e7:5f:b2:e5:e6:ac:
    58:8f:2f:6c:e5:8e:9e:be:09:38:80:fd:1c:c0:90:
    73:b4:e4:de:1f:b1:dc:ce:f1:02:f2:54:28:60:22:
    3f:fa:b1:d7:18:94:cb:32:5a:42:a2:82:d0:ce:cc:
    6a:e0:75:56:b5:1e:40:c0:31:8f:89:af:45:d1:c2:
    ea:23:db:c4:38:b6:a5:b7:a0:95:62:46:20:6f:11:
    92:3b:eb:d0:cf:ec:17:8b:8c:99:11:f9:d1:c5:9b:
    50:19
prime1:
    00:cd:9b:2b:de:81:aa:e9:24:11:80:62:25:df:59:
    d3:c6:be:e7:e1:42:28:69:d9:cc:5d:36:00:48:d6:
    20:ad:bc:cb:25:c8:25:aa:1f:54:a4:a2:82:06:ab:
    0b:c4:56:b8:e8:22:49:43:a0:2e:1b:ee:79:e1:4b:
    6d:c5:63:22:bc:63:6f:18:cf:50:08:9a:48:14:60:
    85:b5:38:bd:91:4c:de:ad:ff:41:75:95:6f:20:6e:
    8d:24:5a:c4:69:2b:92:35:d5:17:27:70:60:02:15:
    4e:55:26:0d:9a:12:eb:8a:de:2b:91:ce:38:ca:4d:
    f4:80:a6:b6:51:f6:49:d0:45:23:04:ec:dc:de:cf:
    62:1c:be:ff:35:ce:dc:52:63:9b:43:b5:bb:3c:bc:
    22:e4:80:49:77:41:54:b0:c9:28:cc:f1:ec:f1:9d:
    17:e2:4b:87:1b:8a:74:ef:ee:db:ca:0e:a0:62:21:
    b0:ef:80:ab:47:dd:4e:5d:3f:de:9d:da:72:9f:bc:
    f0:29:cd:ad:48:40:b5:c4:d3:a2:6d:4f:62:5d:c6:
    09:bf:83:62:92:0a:fd:fb:10:ba:f6:fd:8b:68:07:
    45:ef:d1:38:7e:26:bc:23:06:b9:ad:a2:0a:f8:12:
    b8:3b:de:c7:bb:00:15:db:d2:df:6f:38:3e:95:b3:
    c9:75
prime2:
    00:c0:7c:05:10:a0:00:ba:17:02:39:0b:dd:ee:c2:
    db:1f:d8:00:71:d3:40:62:7d:57:7a:d9:f1:99:66:
    f5:ba:d5:cf:96:af:21:74:90:93:7e:cd:f4:a2:58:
    3b:25:1e:9b:db:e7:75:e1:11:a1:80:a9:c7:99:38:
    5b:1f:e2:ae:53:25:dd:aa:00:1e:40:dc:9e:8b:16:
    8f:68:3f:3b:86:90:e0:88:45:db:24:f4:95:af:e4:
    b7:8a:3c:9d:55:b0:f1:ac:a1:c7:cb:da:de:a5:fd:
    f5:07:ae:54:7b:c6:17:d1:9b:6d:96:a2:5b:1b:50:
    84:db:d2:0e:a6:8d:70:65:cf:18:a1:df:03:fb:de:
    60:cb:a9:a9:1c:e2:24:14:7d:da:0e:e2:b0:f2:aa:
    15:b8:b7:28:0e:dd:e9:ff:67:ca:e8:45:a6:16:d9:
    8d:56:ce:b1:6f:3a:c0:40:eb:45:bd:a7:c6:85:2f:
    d5:cb:15:0b:00:8e:b2:be:c5:84:a0:f7:40:b3:23:
    bb:56:32:92:6b:c0:88:ac:fd:bd:34:23:35:2d:e1:
    9c:e5:31:4e:fb:0b:73:f8:25:eb:ac:33:0e:cb:b3:
    56:f9:ce:9e:6d:52:ff:d6:1d:4a:5a:08:99:b5:8a:
    43:32:1a:71:14:3c:53:f6:26:04:dc:42:d8:4b:11:
    4d:33
exponent1:
    21:13:78:70:38:2d:f5:89:9e:e3:27:66:ee:52:76:
    16:3f:f9:ef:ac:03:f2:5b:5e:5c:14:66:e1:50:c9:
    3b:09:e7:28:71:d5:55:53:ea:fa:fd:45:ab:aa:f2:
    9e:a8:50:e0:cf:3b:38:0c:d9:be:16:94:36:e1:3a:
    8c:89:91:39:fb:49:11:15:b0:cb:61:7e:7d:00:b6:
    21:dc:39:ea:d9:11:ed:ad:e5:aa:f3:da:47:be:61:
    28:5b:0d:c3:d9:85:90:f1:71:e3:1f:59:e2:9b:e8:
    d9:1b:e1:0e:4b:42:e7:39:2c:8b:2d:40:c7:92:d8:
    e5:a5:6c:29:2d:54:dc:93:72:b9:c5:1d:17:1f:07:
    aa:96:33:5a:45:a7:fd:fd:16:2a:43:5f:16:bb:31:
    65:cf:19:82:7c:d9:12:03:9a:73:b5:eb:a7:46:dd:
    63:c3:40:bd:f6:7d:2f:68:1e:a5:97:ca:c7:5e:a2:
    27:35:6c:d8:87:ca:a6:87:b2:d9:74:1e:02:82:93:
    a4:35:f9:4d:69:53:94:91:73:4c:e0:7b:73:98:7b:
    52:fa:e0:ed:9b:a5:16:31:af:d2:32:21:ae:1f:95:
    69:aa:1f:cb:73:91:ca:94:b7:64:2e:70:53:4e:37:
    a3:32:08:b1:95:a6:ee:91:32:ee:a2:7e:b6:f8:90:
    4d
exponent2:
    00:85:85:42:2a:2f:23:71:f2:c5:fa:f6:6d:63:d3:
    4d:17:40:c9:c6:2b:89:4d:08:af:67:2b:c9:b8:e4:
    bf:ee:73:a4:85:5d:44:b2:cc:1b:54:69:df:99:c1:
    e4:7d:32:47:61:7a:a4:bd:94:72:58:82:ed:4f:2e:
    d5:9f:3f:aa:37:49:5c:03:32:03:ca:70:7f:95:f1:
    f7:56:94:0e:61:a1:1b:ef:cb:ad:61:6c:3c:6d:80:
    15:85:51:d8:bd:f9:79:1b:8c:39:c1:02:39:52:21:
    e1:18:0b:e8:d4:d7:2b:ec:9d:89:9e:65:4f:17:9a:
    7a:1e:d6:0e:8d:a0:2d:68:a2:08:ef:38:79:55:fd:
    03:5e:23:79:88:ec:a1:a4:89:4c:7a:e8:eb:5f:d9:
    76:29:e4:fb:67:9c:ae:56:1a:14:99:9c:be:c2:b8:
    f4:9f:53:cf:a7:5d:b4:c1:13:b3:14:05:4e:e3:52:
    b4:56:24:69:b6:60:46:43:a9:56:00:26:a3:7d:e0:
    7a:72:27:b0:84:08:51:86:d6:98:a9:93:06:af:a6:
    02:24:c6:58:88:32:80:a6:90:ff:73:3b:13:b7:5f:
    d7:24:80:1c:4b:af:b8:82:39:5d:3b:29:1e:1e:dd:
    23:11:9d:5f:f5:6e:87:bb:86:d1:f6:4c:74:27:56:
    80:1b
coefficient:
    33:0a:05:36:e4:1b:f0:56:4c:ee:37:da:d2:db:0d:
    3c:b4:c2:5c:12:f4:bb:e2:ef:5a:a5:b6:f9:08:39:
    c3:0f:9f:f0:0a:f3:77:fa:1e:d4:29:ca:c9:27:0b:
    50:df:ea:6f:c2:c8:0e:0d:0d:ef:ce:97:b7:81:f6:
    38:98:ff:53:ba:4b:cf:67:26:06:51:dc:3b:54:97:
    15:7a:e8:31:9a:75:76:3e:ee:13:27:09:cf:2a:7a:
    c4:ec:aa:f5:18:69:3d:37:89:d8:cd:e4:5d:ab:c8:
    3e:09:18:36:a8:8a:fd:27:5e:80:ae:33:44:ca:3b:
    0c:46:dc:3c:66:43:8f:b7:4f:b6:e4:6b:0a:4f:cf:
    7a:56:aa:bb:5a:2d:91:46:42:b4:01:74:e4:77:39:
    7f:08:18:98:af:90:99:98:3f:fa:d8:97:b8:81:a0:
    a9:74:ab:67:34:c3:eb:f1:ef:49:0e:aa:cc:8d:40:
    8e:63:76:56:79:de:f9:63:32:23:44:e2:7d:81:86:
    92:c3:f7:a9:a6:3f:4c:7d:2f:1e:94:6a:2c:47:00:
    5b:d6:33:60:e8:00:be:45:52:b8:b5:50:cc:82:3d:
    6b:39:e2:f3:99:9f:3a:14:1c:bc:d3:9c:f4:9b:8e:
    90:97:ce:36:10:cd:ab:c4:c3:d3:3f:aa:c8:e9:84:
    7a
```
</details>

### Questões a responder:
```
  •Q1  What part of the certificate indicates this is a CA’s certificate?
  •Q2  What part of the certificate indicates this is a self-signed certificate?
  •Q3  In the RSA algorithm, we have a public exponente, a private exponentd, a modulusn, and two secretnumberspandq, such thatn=pq.  Please identify the values for these elements in your certificateand key files.
```

**Q1**

`Basic Constraints` é o que indica se "CA:TRUE" ou CA:FALSE
```shell
(...)
            73:F2:6C:83:94:99:3B:07:A8:35:41:AF:D1:BB:0A:95:C6:A5:24:0F
            X509v3 Authority Key Identifier: 
                keyid:73:F2:6C:83:94:99:3B:07:A8:35:41:AF:D1:BB:0A:95:C6:A5:24:0F

            X509v3 Basic Constraints: critical
                CA:TRUE
    Signature Algorithm: sha256WithRSAEncryption
         91:45:7d:9e:8e:06:17:1e:ce:c4:ce:45:95:a4:6e:e5:7b:08:
         ff:6c:f6:8c:46:42:11:fd:be:b7:0a:74:4d:57:52:4e:bd:f0:
         eb:2d:81:e9:27:72:5f:35:04:e7:f7:42:bc:72:da:87:12:98:
(...)
```

**Q2**

`Issuer` e `subject` ou `certificate`

```shell
(...)
   Serial Number:
            51:f2:b5:cc:86:cb:6b:68:2c:eb:c1:b9:84:2c:b6:39:2e:d0:2e:3c
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C = PT, ST = Vale de Cambra, L = Vila Ch\C3\83\C2\A3, O = Universidade do Porto, OU = DCC, CN = FCUP, emailAddress = up191200000@up.pt
        Validity
            Not Before: Jun  3 02:10:30 2023 GMT
            Not After : May 31 02:10:30 2033 GMT
        Subject: C = PT, ST = Vale de Cambra, L = Vila Ch\C3\83\C2\A3, O = Universidade do Porto, OU = DCC, CN = FCUP, emailAddress = up191200000@up.pt
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption

(...)
```


**Q3**

n -> modulus

p -> prime1

q -> prime2

|modulus|primes|exponent|
|:------:|:-------:|:------:|
|![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/cf986e9d-6fb8-4d35-9565-854ba4e9de35) | ![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/e73b4ddc-2bfc-48d0-855c-04b306b6be61) | ![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/ff2b870a-f12c-4521-953c-e2497e4806f6) |

---
# Task 2 "Generating a Certificate Request for Your Web Server"
Corremos o seguinte comando:
```shell
openssl req -newkey rsa:2048 -sha256 -keyout server.key -out server.csr -subj "/CN=www.tudopago.com/O=Tudo Pago/C=US" -passout pass:dees
```

<details>
<summary>root@96708f960e7a:/# openssl req -in server.csr -text -noout</summary>
  
  ```shell
  Certificate Request:
    Data:
        Version: 1 (0x0)
        Subject: CN = www.tudopago.com, O = Tudo Pago, C = US
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                RSA Public-Key: (2048 bit)
                Modulus:
                    00:a2:76:1b:5f:20:d0:c7:70:69:b0:93:9e:d8:f1:
                    cd:53:dd:12:8e:f8:b0:e6:6f:ee:ae:b7:5e:c9:79:
                    90:9d:0a:3a:a6:f5:15:db:e9:78:13:28:0a:fa:b6:
                    39:d0:f5:ec:67:00:8e:f2:5f:17:b1:6a:2a:43:e5:
                    d1:c6:19:51:60:ec:42:3b:31:a0:33:f3:ca:f2:40:
                    95:fa:49:d9:ee:28:6c:72:15:4e:d4:a7:b8:e7:96:
                    90:c9:41:16:38:ec:23:ac:31:1b:57:25:87:06:f2:
                    22:4b:0b:31:c3:81:d5:04:b7:f3:61:32:f2:99:8a:
                    8f:81:75:91:08:40:d8:5c:79:08:41:a7:d0:1e:f0:
                    55:df:32:52:f9:78:37:b1:f8:ac:98:ff:a2:0d:d9:
                    a5:4e:c8:f1:1d:91:86:a7:1a:cd:b3:3f:6d:05:9f:
                    e4:5d:66:4d:c7:cd:ba:5a:a7:8d:6c:64:41:65:02:
                    db:3d:9f:3d:d6:3f:07:98:95:b1:70:64:02:91:80:
                    ce:51:83:e4:04:d1:73:84:c3:0a:d2:cc:99:32:1d:
                    82:4c:51:12:56:8d:4d:ea:a7:45:95:9d:10:18:19:
                    e7:19:0e:23:a9:2c:47:0a:b0:d2:cc:7e:81:1f:5f:
                    22:ed:6b:59:2d:00:c8:6e:af:fb:36:fe:15:bb:af:
                    c2:2d
                Exponent: 65537 (0x10001)
        Attributes:
            a0:00
    Signature Algorithm: sha256WithRSAEncryption
         67:83:2f:dc:ec:01:4e:ea:3c:e2:69:9d:6d:5b:10:0f:f7:b7:
         74:5a:fb:bf:9f:56:14:27:f2:73:f9:16:46:c3:3e:77:98:e2:
         b5:09:db:c9:83:b9:b4:d0:05:72:8a:5c:4c:6a:5d:d6:50:74:
         6c:7f:08:c9:6b:47:fd:eb:fe:cc:40:5f:47:34:97:e5:f3:99:
         5f:5d:c2:2a:a4:43:1b:83:4b:9c:5f:60:1a:45:9c:d1:0c:db:
         8c:a9:6f:90:94:81:d2:11:3e:60:a7:19:d1:60:d8:df:e0:c8:
         e9:a9:c1:42:70:ed:d2:fb:40:32:68:1f:1c:24:a2:24:ef:89:
         9d:7e:e2:57:7c:08:42:25:0c:6f:85:3a:a7:a9:ea:8a:f7:28:
         ba:02:93:ab:1e:8c:ec:52:11:b4:ab:31:8b:29:36:07:b2:69:
         ca:2f:38:b1:80:62:6f:37:53:eb:33:98:9c:10:e0:b1:14:b7:
         16:66:2c:1f:99:8d:60:59:a0:20:4a:9b:0f:55:a7:c5:9d:8d:
         de:f7:b0:36:c0:d5:39:fc:0c:41:06:78:a2:e1:80:53:33:b8:
         f8:39:52:03:39:8d:16:d0:e3:18:68:d2:35:15:ad:fc:77:a0:
         bf:02:4a:da:4e:c6:b7:6a:d7:86:9d:83:e2:94:94:99:9f:0f:
         9a:0e:33:4a

  ```
  
</details>





<details>
<summary>root@96708f960e7a:/# openssl rsa -in server.key -text -noout</summary>
  
  
```shell
Enter pass phrase for server.key:
RSA Private-Key: (2048 bit, 2 primes)
modulus:
    00:a2:76:1b:5f:20:d0:c7:70:69:b0:93:9e:d8:f1:
    cd:53:dd:12:8e:f8:b0:e6:6f:ee:ae:b7:5e:c9:79:
    90:9d:0a:3a:a6:f5:15:db:e9:78:13:28:0a:fa:b6:
    39:d0:f5:ec:67:00:8e:f2:5f:17:b1:6a:2a:43:e5:
    d1:c6:19:51:60:ec:42:3b:31:a0:33:f3:ca:f2:40:
    95:fa:49:d9:ee:28:6c:72:15:4e:d4:a7:b8:e7:96:
    90:c9:41:16:38:ec:23:ac:31:1b:57:25:87:06:f2:
    22:4b:0b:31:c3:81:d5:04:b7:f3:61:32:f2:99:8a:
    8f:81:75:91:08:40:d8:5c:79:08:41:a7:d0:1e:f0:
    55:df:32:52:f9:78:37:b1:f8:ac:98:ff:a2:0d:d9:
    a5:4e:c8:f1:1d:91:86:a7:1a:cd:b3:3f:6d:05:9f:
    e4:5d:66:4d:c7:cd:ba:5a:a7:8d:6c:64:41:65:02:
    db:3d:9f:3d:d6:3f:07:98:95:b1:70:64:02:91:80:
    ce:51:83:e4:04:d1:73:84:c3:0a:d2:cc:99:32:1d:
    82:4c:51:12:56:8d:4d:ea:a7:45:95:9d:10:18:19:
    e7:19:0e:23:a9:2c:47:0a:b0:d2:cc:7e:81:1f:5f:
    22:ed:6b:59:2d:00:c8:6e:af:fb:36:fe:15:bb:af:
    c2:2d
publicExponent: 65537 (0x10001)
privateExponent:
    36:8b:d9:1c:d3:73:c5:c2:a4:79:b8:d6:b8:98:57:
    0c:35:49:a9:df:2d:e5:f0:e6:fe:9a:6a:a4:d2:c0:
    0f:3a:03:ff:52:82:88:57:97:0d:37:80:98:34:de:
    ac:9e:25:45:60:16:9e:a1:f6:de:86:7a:b2:59:53:
    59:63:de:c2:e0:10:4c:b7:98:c6:58:b0:67:2d:f0:
    2d:1c:e6:a2:e3:c7:a5:76:2a:05:94:5a:ac:c7:0c:
    cd:c5:a0:a4:74:04:76:27:f6:ba:07:cb:92:35:71:
    f6:28:48:09:10:6a:69:2f:29:f4:14:9b:07:9a:52:
    1c:71:50:c4:a8:a0:fc:4a:cd:da:06:e0:83:40:de:
    e6:f7:ab:d0:e6:8c:a7:04:18:40:a8:d4:92:7b:84:
    1b:ef:3b:a9:d0:cd:39:32:cb:06:51:30:6d:dc:27:
    1c:f5:da:ca:46:43:83:97:f6:9b:cf:03:68:e3:f3:
    84:e1:28:9a:3c:e5:32:7a:14:36:72:3b:50:c6:e7:
    de:e3:49:0e:c4:b2:16:25:21:01:da:d5:44:16:49:
    dc:ac:19:3a:34:8f:30:f3:da:a0:b2:bb:10:ae:e6:
    88:60:94:c0:a1:cc:9c:dc:f4:03:18:94:0b:ec:3e:
    d8:6f:3c:20:26:8d:ab:28:3e:58:6f:63:53:83:6f:
    31
prime1:
    00:ce:90:9d:65:96:16:d4:6e:7f:c7:2b:24:e3:1a:
    ce:41:b3:39:c0:ab:82:71:9b:15:53:59:f5:98:80:
    0e:73:52:f8:f5:96:2d:ea:99:bd:1d:5c:e0:f0:8d:
    35:22:5d:cd:1b:69:cf:d0:48:73:78:ee:f4:f7:59:
    78:0e:bb:f8:29:3c:05:b4:01:e6:f1:28:cd:e1:6f:
    e3:9f:b6:71:e1:4c:9b:31:9c:fb:90:69:e9:40:7b:
    1e:dd:02:91:5f:f3:52:96:af:de:a9:db:5c:29:da:
    e5:79:5e:b5:ce:f6:42:95:19:78:5d:6b:53:b6:af:
    14:aa:f9:b4:de:f8:c0:0d:9b
prime2:
    00:c9:57:72:d0:cc:4f:ea:cd:46:b0:01:4d:72:de:
    2c:25:df:d9:a4:f0:ca:12:57:61:40:1e:95:61:37:
    1a:73:7e:3e:1a:be:a4:f8:07:8c:0f:54:10:22:b6:
    8e:8c:25:bf:39:ce:9f:d7:6e:7c:2a:f7:53:bf:ee:
    56:01:d1:fd:89:29:86:2b:c9:bc:68:87:10:43:e9:
    b2:0e:00:86:f0:4f:d9:eb:f1:c4:1a:95:e0:05:bd:
    4e:a9:35:36:16:05:1c:4c:be:2c:1c:ab:58:8b:c6:
    2f:05:79:bc:b3:09:f9:92:91:19:0b:4e:9a:fd:e7:
    5f:a2:b1:ef:1e:fe:6c:cf:d7
exponent1:
    61:37:76:9c:64:f3:01:af:af:bb:90:f6:9f:5b:f2:
    4e:c1:87:20:c2:97:75:d8:43:45:23:45:8f:2c:55:
    a0:b9:20:2a:95:2f:af:06:04:17:59:ab:14:0b:a1:
    42:37:5b:5c:d7:83:d2:c7:06:71:98:24:fa:74:c5:
    28:4a:17:15:71:06:4e:1d:c7:0f:20:e1:24:84:80:
    60:9e:81:22:43:e7:96:86:07:6f:84:29:1f:0b:d3:
    0e:b9:32:aa:31:de:60:c5:0c:ca:6c:4f:07:f3:d9:
    a4:31:8a:e7:88:c6:f5:5d:33:64:e4:2a:56:04:38:
    79:ef:63:cc:bc:cd:c6:99
exponent2:
    08:e0:22:fe:93:53:1e:8d:a8:05:10:39:a2:cb:aa:
    74:8c:f6:a2:2f:bf:28:a9:d3:6e:a9:2a:7a:9b:9c:
    3d:e5:1d:c5:be:0b:b5:a7:57:84:41:77:68:a0:55:
    8e:56:07:a4:fb:b7:ce:1e:5f:b8:e1:28:3e:f8:b9:
    af:e0:da:e5:56:00:45:23:e2:7b:55:20:e1:6d:86:
    dc:d9:27:19:a6:db:7b:6c:2f:f1:e0:13:7d:0d:48:
    4f:6b:3a:14:24:6b:87:f2:86:2d:49:5c:60:e4:fe:
    a3:a6:27:2c:59:4f:38:27:cf:4b:d1:4e:41:cd:16:
    49:2c:c0:d6:c3:ee:76:07
coefficient:
    08:e4:55:33:9b:0b:ed:52:88:cf:77:e4:5c:d4:c0:
    71:4e:ce:e4:93:54:b4:0e:6a:2a:7a:74:36:8d:66:
    d9:bc:e2:35:9c:ba:9b:82:73:17:56:ab:36:16:d5:
    22:9a:62:8b:4c:f5:30:f0:4c:10:4f:cb:70:0d:2d:
    ff:8c:83:14:7a:07:f3:ef:1b:cc:91:2f:38:e4:97:
    a8:80:a8:1b:fe:11:99:81:19:03:dc:62:13:f2:38:
    d3:1c:00:dc:d2:f1:8e:86:11:7a:89:db:ba:49:63:
    a2:f8:9e:12:0c:ce:7d:a7:97:4b:70:3c:79:a0:bc:
    5f:45:80:10:80:59:14:12
```
</details>

---
# Task 3 "Generating a Certificate for your server"

Tornar o certificado self-signed:
```shell
openssl ca -config ./openssl.cnf -policy policy_anything -md sha256 -days 3650 -in server.csr -out server.crt -batch -cert ca.crt -keyfile ca.key
```

Para este prompt correr sem erros, é preciso um diretório `demoCA` com (./newcerts , index.txt , serial);
```shell
mkdir demoCA && cd demoCA && touch index.txt && touch serial && mkdir newcerts && cd ..
```
só depois é que se executa o comando.

| Transformar request num certificate |
|:-----------------------------------:|
|![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/ecbe43f9-f71c-4ed3-8d2f-92d1eb6f4464)|

| Editar openssl |
|:--------------:|
|![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/aa1d70b0-d8a9-4fee-a621-853a0d3fbfb1)|




<details>
<summary>openssl x509 -in server.crt -text -noout</summary>

```shell
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 56 (0x38)
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C = PT, ST = Vale de Cambra, L = Vila Ch\C3\83\C2\A3, O = Universidade do Porto, OU = DCC, CN = FCUP, emailAddress = up191200000@up.pt
        Validity
            Not Before: Jun  3 14:07:47 2023 GMT
            Not After : May 31 14:07:47 2033 GMT
        Subject: C = US, O = Tudo Pago, CN = www.tudopago.com
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                RSA Public-Key: (2048 bit)
                Modulus:
                    00:a2:76:1b:5f:20:d0:c7:70:69:b0:93:9e:d8:f1:
                    cd:53:dd:12:8e:f8:b0:e6:6f:ee:ae:b7:5e:c9:79:
                    90:9d:0a:3a:a6:f5:15:db:e9:78:13:28:0a:fa:b6:
                    39:d0:f5:ec:67:00:8e:f2:5f:17:b1:6a:2a:43:e5:
                    d1:c6:19:51:60:ec:42:3b:31:a0:33:f3:ca:f2:40:
                    95:fa:49:d9:ee:28:6c:72:15:4e:d4:a7:b8:e7:96:
                    90:c9:41:16:38:ec:23:ac:31:1b:57:25:87:06:f2:
                    22:4b:0b:31:c3:81:d5:04:b7:f3:61:32:f2:99:8a:
                    8f:81:75:91:08:40:d8:5c:79:08:41:a7:d0:1e:f0:
                    55:df:32:52:f9:78:37:b1:f8:ac:98:ff:a2:0d:d9:
                    a5:4e:c8:f1:1d:91:86:a7:1a:cd:b3:3f:6d:05:9f:
                    e4:5d:66:4d:c7:cd:ba:5a:a7:8d:6c:64:41:65:02:
                    db:3d:9f:3d:d6:3f:07:98:95:b1:70:64:02:91:80:
                    ce:51:83:e4:04:d1:73:84:c3:0a:d2:cc:99:32:1d:
                    82:4c:51:12:56:8d:4d:ea:a7:45:95:9d:10:18:19:
                    e7:19:0e:23:a9:2c:47:0a:b0:d2:cc:7e:81:1f:5f:
                    22:ed:6b:59:2d:00:c8:6e:af:fb:36:fe:15:bb:af:
                    c2:2d
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Basic Constraints: 
                CA:FALSE
            Netscape Comment: 
                OpenSSL Generated Certificate
            X509v3 Subject Key Identifier: 
                5D:E3:B5:B3:11:AF:24:03:19:86:ED:C3:2B:7A:58:76:99:9C:79:9F
            X509v3 Authority Key Identifier: 
                keyid:73:F2:6C:83:94:99:3B:07:A8:35:41:AF:D1:BB:0A:95:C6:A5:24:0F

    Signature Algorithm: sha256WithRSAEncryption
         98:12:59:8f:99:76:32:9a:d5:3b:66:c4:4b:09:e7:d2:7b:2e:
         82:3b:ee:0c:e7:57:cd:49:87:50:d7:95:f8:27:a2:45:18:42:
         ef:7d:ed:71:4a:a6:ac:72:6c:ee:f6:dc:cf:32:a1:18:df:61:
         6e:bc:74:b5:2f:3d:8e:e1:52:f0:db:68:ba:7f:37:72:de:e7:
         90:66:53:1d:6c:ce:1a:74:5b:7f:a4:4c:a8:8d:6d:d2:65:3b:
         e0:57:29:b8:4c:35:01:e3:df:22:43:d7:68:50:05:d6:5f:4a:
         78:2b:4b:83:76:b9:d2:e7:c2:f0:c6:19:16:c4:33:4a:dc:6a:
         dc:e8:1b:93:a8:44:47:c3:48:6a:16:de:fe:75:92:93:35:7c:
         e1:b1:a3:b9:0c:9d:98:07:3a:5b:b6:df:26:7d:6b:f9:ed:e0:
         37:e3:16:1e:08:7b:6a:c0:91:cc:ca:39:cf:38:99:b0:1b:83:
         d5:eb:89:92:bb:89:b4:81:1c:1f:ba:ba:b0:56:ac:b6:7e:f4:
         dc:5d:18:1b:82:bc:20:82:fb:01:21:b0:f3:6d:34:74:4c:85:
         69:8c:5a:c6:d6:96:a7:6f:1f:42:4d:58:28:cb:36:5b:df:2e:
         9b:e1:c4:9e:4b:b4:a9:c2:4c:90:8f:f0:29:f4:a5:b9:84:97:
         af:4f:84:40:55:28:fd:29:15:e5:e0:cf:00:0e:b5:b7:5b:1a:
         73:e6:b3:e2:84:7e:57:6b:be:7c:65:9b:f1:ea:de:ec:6d:8e:
         f5:4f:1f:e1:4a:71:05:2a:d4:08:e7:b3:f1:56:9e:43:72:8a:
         a9:c0:e1:a1:eb:72:f1:0d:41:e4:cc:1f:54:6d:32:5e:77:ad:
         ad:d9:d6:70:ae:f1:e8:88:75:01:0e:89:03:ca:c4:dd:00:07:
         e2:72:65:68:ad:26:f4:2b:3a:34:b5:f9:59:5a:41:8e:f6:26:
         1e:85:9c:dd:c2:01:77:68:14:7d:25:84:91:a5:51:6e:2f:27:
         ff:56:0f:47:9a:fd:b5:d0:34:fe:f1:28:75:19:fe:0c:63:e5:
         90:6a:ad:96:fd:fc:9b:10:69:c8:ae:96:99:3d:8c:6a:60:5a:
         bb:57:e9:e8:c0:20:49:56:9c:f5:7d:d3:a6:c8:94:76:5f:96:
         09:56:11:35:e0:6b:85:ec:69:8c:00:6b:62:34:63:92:4c:6d:
         78:f4:b8:29:80:87:29:d6:8f:c3:48:40:d7:59:6a:7e:df:8c:
         c4:86:49:f5:a4:8a:87:5a:ea:82:c4:0c:1d:e2:7a:6c:06:1c:
         19:ca:50:e4:c6:0c:c5:27:c8:b2:4b:1a:37:81:e8:68:34:d4:
         b2:a8:37:41:41:e6:5c:ed
```

</details>

---
# Task 4  "Deploying Certificate in an Apache-Based HTTPS Website"

- Editar `nano /etc/apache2/sites-available/bank32_apache_ssl.conf`, guardamos num nome diferente `tudopago_apache_ssl.conf`, modificamos a porta e os nomes do server:
```
<VirtualHost *:443>
    DocumentRoot /var/www/tudopago
    ServerName www.tudopago.com
    ServerAlias www.tudopagoA.com
    ServerAlias www.tudopagoB.com
    ServerAlias www.tudopagoW.com
    DirectoryIndex index.html
    SSLEngine On
    SSLCertificateFile /server.crt
    SSLCertificateKeyFile /server.key
</VirtualHost>

<VirtualHost *:443>
    DocumentRoot /var/www/tudopago
    ServerName www.tudopago.com
    DirectoryIndex index_red.html
</VirtualHost>

# Set the following gloal entry to suppress an annoying warning message
ServerName localhost

```

- copiamos `cp /var/www/bank32/ -r /var/www/tudopago`
- corremos os seguintes comandos para ligar o server:
  ```shell
  $ a2enmod ssl && a2ensite tudopago_apache_ssl
  $ service apache2 reload && service apache2 start
  ```
 
O que deu:
```
 * Starting Apache httpd web server apache2
 Enter passphrase for SSL/TLS keys for www.tudopago.com:443 (RSA):
 * 
```

|site|
|:---:|
|![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/a738aa41-21a2-4c9e-ab1e-6b9772195442)|

---
# Task 5 "Launching a Man-In-The-Middle Attack"
Vamos tentar intermediar uma ligação, neste caso o `sigarra.up.pt`:
Para isso fizemos umas mudanças nos seguintes ficheiros:
- /etc/apache2/sites-available/tudopago_apache_ssl.conf 
- /etc/hosts

|tudopago_apache_ssl-conf|hosts|
|:---------------------:|:-----------------:|
|![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/a615557a-f474-4a29-874f-f8a1fa564bfb)|![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/2dc582ee-ca7b-41ea-9cf2-1d4fb819a8d9)|

and when we tried to connect to the website we got this error:
| Error |
|:---:|
|![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/3d56ae65-bced-4403-9827-56c4e4ca789b)|

Isso ocorre porque o nome do servidor não corresponde ao esperado, o que só aconteceria se o servidor tivesse um certificado válido de uma autoridade de certificação.


# Task 6 " Launching a Man-In-The-Middle Attack with a Compromised CA"





