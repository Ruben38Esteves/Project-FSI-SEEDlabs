# LOGBOOK 7 - Format Strings

## Preparação

Começamos por desligar address space randomization que aleatoriza o início da heap ou stack.

```bash
$sudo sysctl -w kernel.randomize_va_space=0
```

## O programa vulnerável

Analisamos ```program.c``` e identificamos a vulnerabilidade na função ```myprintf(char *msg)``` onde se tenta dar print de msg.
Compilamos utulizando o Makefile, que compila para 32 e 64 bits.

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

