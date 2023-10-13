#Environment Variable and Set-UID Program Lab

## Task 1

Aprendemos que todas as variáveis do sistema podem ser listadas com o comando "printenv". É possível especificar as variáveis que queremos mostrar, por exemplo "printenv PWD".
Podemos usar "export" e "unset" para dar set e unset a varáveis de ambiente.

## Task 2

Em Unix, fork() cria um novo processo duplicando um determinado processo pai dando origem a um processo filho quase idêntico. Nem tudo é herdado, mas nesta task pretendemos saber se as variáveis de ambiente são herdadas pelo processo filho após a sua criação.

Guardamos em ficheiros diferentes as variáveis de ambiente do processo filho e do processo pai (child e parent respetivamente). Ao analisar as diferenças, concluimos que o resultado é vazio, significando assim que as variáveis de ambiente do processo pai são herdadas pelo processo filho após o comando fork(). <br><br>
![](../pictures/Captura_de_ecrã_2023-10-10_182434.png) <br>

## Task 3
Quando o código do arquivo "myenv.c" é executado com o terceiro argumento de execve() definido como NULL, as variáveis de ambiente não são transmitidas para esse pointer, uma vez que possui um valor nulo, dando um output vazio.
Se substituirmos NULL por environ, esse array será utilizado para transmitir as variáveis de ambiente.

## Task 4
Utilizando o comando system(), podemos executar o comando na shell, ao contrário de execve(). Verificamos que utilizando system() é criado um novo processo em que todas as variáveis de ambiente são passadas do  processo anterior.
Com execve() o comando é executado mantendo o processo atual e as varáveis de ambiente. O processo em execução é substituido pelo comando passado como argumento, que apenas mantém as variáveis de ambiente se as mesmas forem passadas como argumento.

## Task 5
Set-UID é um mecanismo de segurança do Unix no qual um user pode executar um programa com permissões de criador.
No Step 1 criou-se um programa que dá print a todas as variáveis de ambiente no processo atual.
Compilamos, mudamos o owner para root e tornamo-lo num programa Set-UID.
Depois definimos algumas variáveis de ambiente utilizando export.
Executando novamente verificamos que as variáveis definidas anteriormente estão na lista de variáveis de ambiente do programa, à exceção de LD_LIBRARY_PATH. Isto porque esta variável permite definir um caminho onde o programa vai procurar as bibliotecas partilhadas, que seria vulnerável.

## Task 6
Criamos um programa Set-UID que chama o comando "ls" do Linux. Mundando a variavel PATH para um outro diretorio faremos com que esse programa corra um outro programa malicioso (tambem chamado "ls") criado por nos. Apos correr o codigo, verificamos que a funçao "ls" executada foi a nossa.


