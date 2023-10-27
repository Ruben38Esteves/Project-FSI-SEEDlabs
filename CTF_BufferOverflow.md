# Buffer Overflow CRF

## Setup

Começamos por instalar o recomendado no guião, a biblioteca pwntools e o programa checksec para analisar as proteções com que o programa foi compilado.

## Desafio 1

Analisamos os ficheiros fornecidos. Com o programa que instalamos checksec verificamos que program tem arquitetura x86, as posições do binário não estão randomizadas, existem regiões de memória com permissões de leitura, escrita e execução, referindo-se à stack neste caso.
<br><br>
![](../pictures/CTF_BO_pic1.png)
<br><br>
Analisando main.c vemos que o código aloca memória para meme_file um buffer (8 e 32 bytes respetivamente). Vemos também que pode ocorrer overflow na seguinte linha, que permite um buffer overflow, permitindo a cópia de 40 bytes para o buffer de apenas 32.
```c
scanf("%40s", &buffer);
```
Se introduzirmos um input que exceda a capacidade do buffer, vamos começar a escrever na zona de memória alocada para meme_file. Em main.c vai ser lido o conteúdo de meme_file, pelo que podemos reescrever o nome do ficheiro a ser lido. Para isso no exploit.py bastou introduzirmos 32 caracteres seguidos de flag.txt.
<br><br>
![](../pictures/CTF_BO_pic2.png)
<br><br>
Executando, deu display à nossa flag, que introduzimos na plataforma.

![](../pictures/CTF_BO_pic3.png)

## Desafio 2

Analisamos o código de main.c. Alocam-se 9 bytes para meme_file, 4 bytes para val e 32 bytes para um buffer. Vamos utilizar o mesmo método do desafio anterior, um buferoverflow occore no scanf() mas agora só podemos ver o conteúdo do ficheiro se val = 0xfefc2324. Correndo o programa vemos o valor de val declarado.
<br><br>
![](../pictures/CTF_BO_pic4.png)
<br><br>
Deste modo vemos de que método dispor os bytes que compõe val para obtermos o valor que queremos.<br>
```
"\xef\xbe\xad\xde" --> 0xdeadbeef
0xfefc2324 --> "\x24\x23\xfc\xfe"
```
Se enchermos o buffer, começamos a escrever em val, e introduzindo o valor desejado, conseguimos passar essa condição. Depois é só introduzir de novo o nome do ficheiro desejado (flag.txt). Introduzimos então no script 32 caracteres seguidos de \x24\x23\xfc\xfeflag.txt.
<br>
![](../pictures/CTF_BO_pic5.png)
<br>
Corremos e deu-nos a flag.
<br>
![](../pictures/CTF_BO_pic6.png)



