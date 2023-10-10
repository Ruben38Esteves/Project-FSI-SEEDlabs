#Environment Variable and Set-UID Program Lab

## Task 1

Aprendemos que todas as variáveis do sistema podem ser listadas com o comando "printenv". É possível especificar as variáveis que queremos mostrar, por exemplo "printenv PWD".
Podemos usar "export" e "unset" para dar set e unset a varáveis de ambiente.

## Task 2

Em Unix, fork() cria um novo processo duplicando um determinado processo pai dando origem a um processo filho quase idêntico. Nem tudo é herdado, mas nesta task pretendemos saber se as variáveis de ambiente são herdadas pelo processo filho após a sua criação.

Guardamos em ficheiros diferentes as variáveis de ambiente do processo pai e do processo filho (child e parent respetivamente). Ao analisar as diferenças, concluimos que o resultado é vazio, significando assim que as variáveis de ambiente do processo pai são herdadas pelo processo filho após o comando fork(). <br>
![](../pictures/Captura_de_ecrã_2023-10-10_182434.png) <br>

## Task 3
