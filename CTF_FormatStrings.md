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