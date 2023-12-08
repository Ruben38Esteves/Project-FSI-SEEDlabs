#SQL Injection

Começamos por observar o ficheiro facultado index.php. Vimos que a query utilizada para fazer login é ```"SELECT username FROM user WHERE username = '".$username."' AND password = '".$password."'"```. Este método de compor uma query é vulnerável, e vamos utilizar isso para tentar entrar na conta com username admin. No forms de login, introduzimos o username ```admin'--``` e qualquer coisa na password, não importa pois estará comentado. A query resultante será:

```sql
SELECT username FROM user WHERE username = 'admin'--' AND password = 'wtv'
```

Utilizamos a aspa e os dois traços para que tudo aquilo que está depois do username fique comentado, fazendo o username ficar como o único critério para ir buscar o user para a sessão. Entramos assim na conta do admin, e a flag estava lá impressa.~

![](../pictures/ctfsql.png)


