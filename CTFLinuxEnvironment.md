#Linux Environment CTF

Começamos por correr o comando indicado para nos conectarmos ao servidor:

```bash
$ nc ctf-fsi.fe.up.pt 4006
```
Estando conectados, utilizamos o comando ls para ver os ficheiros do diretorio e encontramos o seguinte:

![](../pictures/ctf4ls.png)

Acreditando que admin_note.txt seria uma pista, decidimos ver o que ele continha.

![](../pictures/admin_note.png)

Graças a essa dica, fomos à pasta /tmp e após tentar criar um ficheiro percebemos que esta era a unica pasta que nao é read-only.

![](../pictures/catmain.png)
