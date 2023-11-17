# LOGBOOK 7 - Format Strings

## Preparação

Começamos por desligar address space randomization que aleatoriza o início da heap ou stack.

```bash
$sudo sysctl -w kernel.randomize_va_space=0
```

## O programa vulnerável

Analisamos ```program.c``` e identificamos a vulnerabilidade na função ```myprintf(char *msg)``` onde se tenta dar print de msg.
Compilamos utilizando o Makefile, que compila para 32 e 64 bits.

### Criar os containers

Criamos os containers e iniciamos num dado terminal. Utilizamos os seguintes comandos.

```bash
docker-compose build
docker-compose up
```

o ficheiro docker-compose.yml vai entáo fazer que dois servidores sejam iniciados
Após isto, abrimos outro terminal.

## Task 1: Crashing the program

No novo terminal, que será usado para comunicar com os servidores, testamos o envio de uma string para o servidor indicado.

```bash
echo 'hello' | nc 10.9.0.5 9090
```

O servidor retornou o seguinte:

![](../pictures/log7pic2.png) <br>

Enviando a string ```'%s'``` para o servidor vai fazer com que a format string tente ir buscar o endereço acima de si na stack e tente imprimir a string que lá estaria. O servidor "crashou" fazendo isto, não houve a mensagem "Returned Properly".

## Task 2: Printing Out the Server Program’s Memory

### Task 2.A: Stack Data

Para conseguirmos imprimir os primeiros 4 bytes, precisamos que o input seja um valor que facilmente identificamos. Usámos ```"DEAD"```, que em hexadecimal dá ```"44454144"```.

Demos como input ```"DEAD"``` concatenado com vários ```"%08x"```.

```bash
echo "DEAD%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X" | nc 10.9.0.5 9090
```

O servidor retornou o seguinte:

![](../pictures/log7pic3.png) <br>

Como dá para reparar, o final ```"44414544"``` são os bytes invertidos do endereço da string ```"DEAD"``` dado como input.

Sendo que cada endereço é constituído por 8 caracteres e que entre ```"DEAD"``` e ```"44414544"``` existem 504 caracteres, então existem 63 endereços(504/8) na stack entre a format string e o buffer.

Em suma, para imprimir os primeiros 4 bytes do input inicial é necessário criar uma string com 64 ```'%x'```, sendo endereços intermédios nos primeiros 63 e no último os 4 bytes (32 bits) iniciais do input.
