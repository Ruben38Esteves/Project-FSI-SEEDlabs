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



