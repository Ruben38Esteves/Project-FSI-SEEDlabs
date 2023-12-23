# LOGBOOK 13 - Sniffing and Spoofing

Começámos por criar os containers:

```bash
docker-compose build
docker-compose up
```

De seguida descobrimos as shells existentes, com o comando `dockps`:

![](../pictures/log13pic1.png)

Assim, com o comando `docksh` conseguimos usar a shell de cada uma das opções acima `seed-attacker`, `hostA` e `hostB`.

Antes de iniciar as tasks, é necessário saber o nome da network interface e usar scapy.

Para isso utilizámos o comando:

```bash
docker network ls
```

Que retorna:

![](../pictures/log13pic2.png)

O nome é `net-10.9.0.0` e o seu ID é `2a5723331b97`.


Para usar o scapy é dado um programa em python:

![](../pictures/log13pic3.png)

Podemos tornar o `mycode.py` num executável, com o comando:

```bash
chmod a+x mycode.py
```

Assim podemos correr o ficheiro ao escrever apenas `mycode.py`:

![](../pictures/log13pic4.png)

## Task 1.1: Sniffing Packets

Nesta tarefa vamos usar o scapy para conseguir fazer packet sniffing em ficheiros python.

Com o ficheiro `task1.1.py` é possível fazer packet sniffing:

![](../pictures/log13pic5.png)

### Task 1.1A

Começámos por tornar o `task1.1.py` num executável com o comando `hmod a+x task1.1.py`.

Abrimos dois terminais, acedemos a shell do `seed-attacker` num dos terminais e acedemos a shell do `hostA`, com o comando `docksh`:

![](../pictures/log13pic6.png)

![](../pictures/log13pic7.png)

Na shell do `seed-attacker` corremos o ficheiro `task1.1.py`.

De seguido vamos à shell do `hostA` e corremos o comando `ping 10.9.0.6`:

![](../pictures/log13pic8.png)

Os packets são retornados na shell do `seed-attacker`:

![](../pictures/log13pic9.png)
![](../pictures/log13pic10.png)
![](../pictures/log13pic11.png)
![](../pictures/log13pic12.png)
![](../pictures/log13pic13.png)

Isto tudo foi feito com os privilégios `root`.

Ao utilizarmos o comando:

```bash
su seed
```

E de seguida corrermos o ficheiro `task1.1.py`, obtemos:

![](../pictures/log13pic14.png)

Percebemos que não nos é permitido  pois não temos permissões para isso.

### Task 1.1B

Quando fazemos packet sniffing procurámos certos tipos de packets, para conseguirmos filtrar usámos os filters, assim como dá para ver na tarefa anterior em que apenas capturávamos ``ICMP packets``.

#### - Capture any TCP packet that comes from a particular IP and with a destination port number 23.

Desta vez, queremos todos os `TCP packets` que provêm de um IP, em particular, e tenha destino na ``porta 23(telnet)``.

Começámos por editar o ficheiro `task1.1.py`:

![](../pictures/log13pic15.png)

De seguida abrimos um terminal e acedemos a shell do `hostB` com o comando `docksh 48`.

Na shell do `seed-attacker` corremos o ficheiro `task1.1.py`.

De seguido vamos à shell do `hostB` e corremos o comando `telnet 10.9.0.5`:

![](../pictures/log13pic16.png)

Os packets são retornados na shell do seed-attacker:

![](../pictures/log13pic17.png)

#### - Capture  packets  comes  from  or  to  go  to  a  particular  subnet.

Agora, queremos packets que provêm ou vão para um subnet particular.

Começámos por editar o ficheiro `task1.1.py`:

![](../pictures/log13pic18.png)

Na shell do `seed-attacker` corremos o ficheiro `task1.1.py`.

De seguido vamos à shell do `hostB` e corremos o comando `ping 128.230.0.11`:

![](../pictures/log13pic19.png)

Os packets são retornados na shell do seed-attacker:

![](../pictures/log13pic20.png)

## Task 1.2: Spoofing ICMP Packets

O objetivo desta task é dar spoof a IP packets com uma arbitrary source IP address. Vamos dar spoof ICMP echo request packets e enviá-los para outra VM na mema network. Para isso utilizámos Wireshark para observar se nossa request será aceite pelo receiver. Se for aceite, um echo reply packet será enviado para o spoofed IP address.

Utilizámos o ficheiro `task1.2.py` para desempenhar a tarefa:

![](../pictures/log13pic21.png)

Abrimos o Wireshark:

![](../pictures/log13pic22.png)

Abrimos no ``host 1.2.3.4``:

![](../pictures/log13pic23.png)

Na shell do `seed-attacker` corremos o ficheiro `task1.2.py`:

![](../pictures/log13pic24.png)

Voltamos ao Wireshark:

![](../pictures/log13pic25.png)

Recebemos um echo reply packet será enviado para o spoofed IP address.