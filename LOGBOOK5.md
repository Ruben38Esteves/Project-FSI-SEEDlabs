# LogBook 5

## Antes de começar
Começamos por desativar algumas funcionalidades de segurança do OS. Executando "sudo sysctl -w kernel.randomize_va_space=0" desativamos uma feature do Ubuntu que torna aleatório partes de endereços. Isto dificultaria um ataque de buffer overflow. <br>

![](../pictures/log5pic1.png)
<br>

Após isto, configuramos /bin/shpara funcionar como desejamos. Tentar mexer num programa Set-UID na shell resultaria numa remoção dos privilégios.
Neste Seedlabs o programa a atacar será um Set-UID, e utilizaremos bin/sh, que é um symbolic link, interpretado como bin/dash, onde esta medida de segurança está implementada. Então temos de dar link de bin/sh a outra shell que não tenha esta medida de segurança. Utilizamos o comando "$ sudo ln -sf /bin/zsh /bin/sh" direcionamos bin/sh para uma outra shell zsh previamente instalada.


## Task 1 - ShellCode
Shellcode é essencialmente código que permite lançar uma shell. Neste SeedLabs mostram-nos uma shellcode de 32 e 64 bits, sem dar grande ênfase a como funciona o código assembly. 
Após compilarmos call_shellcode.c, um programa que gera o binario para as shellcodes, foram generados dois ficheiros a32.out e a64.out que quando corridos, ambos abriram uma shell no diretório onde se encontravam. <br>
![](../pictures/shells.png)
<br>
## Task 2 - O Programa Vulnerável
O programa vulnerável utilizado neste SeedLabs é stack.c. Tem uma vulnerabilidade de buffer overflow, da qual nos vamos aproveitar para ganhar root privilege.
No programa em questão o bufffer overflow acontece quando se tenta fazer um strcpy(buffer, str) tendo buffer apenas 100 bytes e str 517. Como strcpy não verifica limites, ocorre buffer overflow. Compilamos o programa utilizando o MakeFile que já tem determinadas configurações (imagem abaixo) que desligam o StackGuard, mudam a ownership do programa para root e ativamos o Set-UID. <br>
![](../pictures/log5pic3.png)

## Task 3 - Atacar o programa 32 bits.~

Para nos podermos aproveitar da vulnerabilidade buffer overflow deste programa precisamos de saber o endereço inicial do buffer e onde está o return address. Utilizando gdb, um método de debugging, conseguimos encontrar estes valores. <br>
![](../pictures/log5pic4.png)







