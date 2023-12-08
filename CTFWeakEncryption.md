# CTF Weak Encryption

## Parte 1

Para esta ctf foi nos dado o comando ```nc ctf-fsi.fe.up.pt 6003``` e o ficheiro ```cipherspec.py```

Começamos então por analisar o ficheiro e encontramos três funções, uma para gerar chaves, uma para encryptar as mensagens e uma para desencriptar as mensagens. Olhando para a primeira encontramos rapidamente o seguinte:

```python
KEYLEN = 16

def gen(): 
	offset = 3 # Hotfix to make Crypto blazing fast!!
	key = bytearray(b'\x00'*(KEYLEN-offset)) 
	key.extend(os.urandom(offset))
	return bytes(key)
```

Aparentemente alguem quis tornar o processo de geração das chaves mais rapido e a sua maneira de o fazer foi reduzir o numero de bytes da chave que sao gerados aleatoriamente, preenchendo o resto com `x00`.

Isto causa um grande problema pois reduz o numero de chaves possiveis de `3,5x10^38` para `16 777 216`, que é uma reduçao bastante grande.

De seguida corremos o comando no terminal e encontramos o seguinte:

// FOTO DO COMANDO

## Parte 2

### Como consigo usar esta ciphersuite para cifrar e decifrar dados?

Para cifrar dados, podemos utilizar a função `enc()` que leva como argumentos:
>k - A chave a ser utilizada

>m - A mensagem a ser encriptada

>nonce - o nonce a ser utilizado

Para decifrar dados, temos a função `dec()` que leva como argumentos:
>k - A chave a ser utilizada

>c - A mensagem encriptada a ser desencriptada

>nonce - o nonce a ser utilizado

### Como consigo fazer uso da vulnerabilidade que observei para quebrar o código?

Dado o numero relativamente pequeno de chaves possiveis e sendo que temos o nonce e a mensagem encriptada, podemos ter uma abordagem de brute force em que testamos todas as combinações possiveis ate encontrar a correta

### Como consigo automatizar este processo, para que o meu ataque saiba que encontrou a flag?

Sendo que nos sabemos que a flag começa com `flag`... podemos dizer ao nosso codigo para parar quando encontra um resultado que comece com isso. Para tal, escrevemos o seguinte pedaço de codigo:

```python
def newgen():
    for combination in product(range(256), repeat=3):
        key = bytearray(b'\x00'*(KEYLEN-3))
        key.extend(combination)
        yield bytes(key)

result_string = "aaaaa"
nonce = unhexlify("b4cbcf196bc638cf24823ec552eb9735")
text = unhexlify("07f48ee89c2de2b229d93caf018bdde2b6987393df688550a8bd45ecfd6b34b829b78bca72d9ea")

key_generator = newgen()
for key in key_generator:
    result_bytes = dec(key, text, nonce)
    result_string = result_bytes.decode("UTF-8", errors='ignore')
    if result_string[0:4] == "flag":
        break
    

print(result_string)
```

Passado algum tempo de termos colocado o codigo a correr, lemos o seguinte output na consola:

// FOTO DA FLAG

### Encontramos a flag!