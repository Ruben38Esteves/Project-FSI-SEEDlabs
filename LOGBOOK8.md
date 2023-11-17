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

