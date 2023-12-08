# LOGBOOK 10 - Secret Key Encryption

## Task 1: Frequency Analysis

Esta tarefa, baseia-se em tentativa erro. Uma vez que quantas mais letras se descobre, mais rápido se descobre a próxima.

Começámos por saber as single-letter, bigram (2-letter sequence), e trigram frequencies (3-letter sequence) presentes no ficheiro `ciphertext.txt`, para isso usámos o comando `./freq.py`, que retornou:

![](../pictures/log10pic1.png)
![](../pictures/log10pic2.png)
![](../pictures/log10pic3.png)

Ocorreu-nos que, em inglês, a letra mais utilizada seria `'E'` e que a palavra mais utilizada seria `'THE'`. Ao usar o comando:

```bash
tr ´ytn´ ´THE´ < ciphertext.txt > out.txt
```

Podemos verificar algumas alterações no ficheiro `out.txt`:

![](../pictures/log10pic4.png)

Passado algum tempo de tentativa erro, ao utilizar o comando:

```bash
tr ´ytnmhqlacxvufprsibekdzgjo´ ´THEIRSWCMOANVDGKLFPXYUBQJ´ < ciphertext.txt > out.txt
```

Conseguimos obter o artigo decifrado, no ficheiro `out.txt`: 

![](../pictures/log10pic5.png)


## Task 2: Encryption using Different Ciphers and Modes

### `-aes-128-cbc`

Primeiramente, utilizamos a cifra `aes-128-cbc` e encriptamos o artigo da tarefa 1, presente no ficheiro `out.txt` com o comando:

```bash
openssl enc -aes-128-cbc -e -in out.txt -out aes_en.txt -K  00112233445566778889aabbccddeeff -iv 0102030405060708
```
Ficheiro encriptado, `aes_en.txt`:

![](../pictures/log10pic6.png)

De seguida desincriptamos com o comando:

```bash
openssl enc -aes-128-cbc -d -in aes_en.txt -out aes_de.txt -K  00112233445566778889aabbccddeeff -iv 0102030405060708
```

Ficheiro desincriptado, `aes_de.txt`:

![](../pictures/log10pic7.png)

### `-camellia-256-ecb`

Em segundo, utilizamos a cifra `camellia-256-ecb` e encriptamos o artigo da tarefa 1, presente no ficheiro `out.txt` com o comando:

```bash
openssl enc -camellia-256-ecb -e -in out.txt -out camelia.bin
```

Foi necessário criar uma palavra-passe:

![](../pictures/log10pic8.png)

Ficheiro encriptado, `camelia.bin`:

![](../pictures/log10pic9.png)

```bash
openssl enc -camellia-256-ecb -d -in camelia.bin -out camelia_de.txt 
```

Utilizamos a palavra-passe criada anteriormente:

![](../pictures/log10pic10.png)

Ficheiro desincriptado, `camelia_de.txt`:

![](../pictures/log10pic11.png)

### `-bf-cbc`

Por fim, utilizamos a cifra `-bf-cbc` e encriptamos o artigo da tarefa 1, presente no ficheiro `out.txt` com o comando:

```bash
openssl enc -bf-cbc -e -in out.txt -out bf_enc.txt -K  00112233445566778889aabbccddeeff -iv 0102030405060708 
```

Ficheiro encriptado, `bf_enc.txt`:

![](../pictures/log10pic12.png)

De seguida desincriptamos com o comando:

```bash
openssl enc -bf-cbc -d -in bf_enc.txt -out bf_de.txt -K  00112233445566778889aabbccddeeff -iv 0102030405060708

```

Ficheiro desincriptado, `bf_de.txt`:

![](../pictures/log10pic13.png)


