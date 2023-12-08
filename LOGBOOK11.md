# Public-Key Infrastructure

## Setup

Começamos por adicionar novas entradas nos hosts conhecidos pela VM usando ´´´sudo nano etc/hosts´´´ e iniciamos os containers do lab.

## Task 1

Uma certificate authority (CA) é uma entidade de confiança que emite certificados digitais. Neste lab não vamos usar nenhuma CA comercial, então vamos fazer a nossa própria.

Copiamos o ficheiro openssl.cnf para a pasta de trabalho local para podermos fazer alterações. Depois descomentamos a linha unique_subject.
Criamos os ficheiros e diretórios necessários e fizemos serup da CA:

![](../pictures/log11pic1.png)

Podemos ver que se trata de uma CA por:

![](../pictures/log11pic2.png)

Podemos ver que é self-signed pois issuer e subject são iguais:

![](../pictures/log11pic3.png)

## Task 2

Nesta task geramos um certificate request para o servidor. Executamos o comando facultado:

![](../pictures/log11pic4.png)

## Task 3

Agora vamos tornar o request num certificado. Executamos o comando ´´´$ openssl ca -config openssl.cnf -policy policy_anything -md sha256 -days 3650 -in server.csr -out server.crt -batch -cert ca.crt -keyfile ca.key´´´. Editamos o ficheiro openssl  descomentando copy_extensions.

![](../pictures/log11pic5.png)

Executamos o comando ´´´openssl x509 -in server.crt -text -noout´´´ e verificamos que os nomes alternativos estão abrangidos.

![](../pictures/log11pic6.png)

## Task 4

Copiamos "server.ctf" e "server.key" para o diretorio /volumes e mudamos os nomes para bank32. Modificamos o ficheiro "etc/apache2/sites-available/bank32_apache_ssl.conf" dentro do container, para que o certificado e chave usados sejam os desejados

![](../pictures/log11pic7.png)

para iniciar o servidoor Apache abrimos uma shell no container e inserimos ´´´service apache2 start´´´

![](../pictures/log11pic8.png)

Acedendo a https://bank32.com vimos que a ligaçao não era segura. Para corrigir isto, importamos o certificado CA que geramos.

![](../pictures/log11pic9.png)
