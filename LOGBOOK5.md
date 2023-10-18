# LogBook 5

## Antes de começar
Começamos por desativar algumas funcionalidades de segurança do OS. Executando "sudo sysctl -w kernel.randomize_va_space=0" desativamos uma feature do Ubuntu que torna aleatório partes de endereços. Isto dificultaria um ataque de buffer overflow

![](../pictures/)

Após isto, configuramos /bin/shpara funcionar como desejamos. Tentar mexer num programa Set-UID na shell resultaria numa remoção dos privilégios.
Neste Seedlabs o programa a atacar será um Set-UID, e utilizaremos bin/sh, que é um symbolic link, interpretado como bin/dash, onde esta medida de segurança está implementada. Então temos de dar link de bin/sh a outra shell que não tenha esta medida de segurança. Utilizamos o comando "$ sudo ln -sf /bin/zsh /bin/sh" direcionamos bin/sh para uma outra shell zsh previamente instalada.

# IMAGEM2

## Task 1 - ShellCode
Shellcode é essencialmente código que permite lançar uma shell. Neste SeedLabs facultam-nos uma shellcode de 32 e 64 bits, sem dar grande ênfase a como funciona o código assembly.



