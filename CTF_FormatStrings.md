# Format Strings 

## Desafio 1

Começámos por usar o comando `checksec --file=program` para assim saber as proteções ativas.

![](../pictures/CTF7_pic1.png)

Como vemos, na imagem em cima, não temos a proteção PIE(Position Independent Executable), assim percebemos que não há aleatoriedade dos endereços no executável.

- Qual é a linha do código onde a vulnerabilidade se encontra?

A vulnerabilidade está na linha 25 `scanf("%32s", &buffer);`.

- O que é que a vulnerabilidade permite fazer?

Permite que o atacante ultrapasse os limites do buffer e possa sobrescrever áreas adjacentes da memória, visto que o buffer contém menos bytes do que a função load_flag() carrega.

![](../pictures/CTF7_pic2.png)

- Qual é a funcionalidade que te permite obter a flag?

Para obter a flag, exploramos uma format string vulnerability e também com a capacidade de obter o endereço da variável global, onde está guardada a flag, é nos permitido retornar o valor da string.

Usámos o comando `gdb program`, seguido do comando `p &flag` e descobrimos o endereço da variável flag:

![](../pictures/CTF7_pic3.png)

De seguida, utilizámos o programa fornecido e alteramos a linha 21 para o endereço da flag e `'%s'`, `"\x60\xc0\x04\x08%s"`e LOCAL para `False`:

![](../pictures/CTF7_pic4.png)

Ao executar o programa com o comando `python3 exploit_example.py` obtemos a flag:

![](../pictures/CTF7_pic5.png)

## Desafio 2

Assim como no Desafio 1, começámos por usar o comando `checksec --file=program` para saber as proteções ativas.

![](../pictures/CTF7_pic6.png)

Como vemos, na imagem em cima, não temos a proteção PIE(Position Independent Executable) está outra vez desativada.

- Qual é a linha do código onde a vulnerabilidade se encontra? E o que é que a vulnerabilidade permite fazer?

![](../pictures/CTF7_pic7.png)

A vulnerabilidade está na linha 13 `scanf("%32s", &buffer);`, pois a função scanf não verifica adequadamente os limites do buffer quando  lê uma string do usuário. Assim, o atacante está livre para sobrescrever dados na memória, incluindo a variável key. A partir do momento em que o atacante tem o controlo da variável key e define o seu valor como `0xbeef`, ativa a "Backdoor".

- A flag é carregada para memória? Ou existe alguma funcionalidade que podemos utilizar para ter acesso à mesma.

Sim, e ao utilizar o comando `p &key` no GDB, temos acesso ao valor guardado nela.

- Para desbloqueares essa funcionalidade o que é que tens de fazer?

Pelo código fornecido, não é possível abrir o nosso código, pois não há uma função que o faça diretamente, mas se a variável key for igual a 0xBEEF, conseguiremos executar o bash e, assim abrir o código.

Começámos por abrir o debugger com o comando `gdb program` e utilizámos o comando `p &key` no GDB e obtemos o endereço da key.

![](../pictures/CTF7_pic8.png)

Sabemos que `0xBEEF` em decimal é `48879`.

O comando `%n`, como vimos anteriormente, vai escrever o número de caracteres escritos até ali, assim o input até `%n` terá `48879` caracteres. Para um input tão extenso, utilizamos a notação `%.NX`, com `N = 48879 - 4 - 4 = 48871`, para escrever os 48871 caracteres restantes com o valor 0.

![](../pictures/CTF7_pic9.png)

Ao executar o programa com o comando `python3 mad_exploit.py` obtemos a flag no arquivo flag.txt. De seguido usámos o comando `cat flag.txt` para assim vermos a flag:

![](../pictures/CTF7_pic10.png)