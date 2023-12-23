# CTF RSA

## Parte 1 - Investigação

Ao correr o comando **nc ctf-fsi.fe.up.pt 6004** recebemos o seguinte no terminal:

![](../pictures/CTFRSA1.png)

Sendo que se trata de encriptaçao RSA e nos é dado o **n** e o **e (expoente)**, precisamos de calcular os **primos** e o **d**.

É nos dada a informaçao que os primos estão proximos de 2^512 e 2^513, ora, sabendo que o produto dos dois tem de ser igual a **n**, procedemos ao seu calculo:

Para isso, utilizamos a implementação do algoritmo Miller-Rabin encontrado em Geeksforgeeks:

```py
# Credits: https://www.geeksforgeeks.org/primality-test-set-3-miller-rabin/
# Python3 program Miller-Rabin primality test
import random 
 
# Utility function to do
# modular exponentiation.
# It returns (x^y) % p
def power(x, y, p):
     
    # Initialize result
    res = 1; 
     
    # Update x if it is more than or
    # equal to p
    x = x % p; 
    while (y > 0):
         
        # If y is odd, multiply
        # x with result
        if (y & 1):
            res = (res * x) % p;
 
        # y must be even now
        y = y>>1; # y = y/2
        x = (x * x) % p;
     
    return res;
 
# This function is called
# for all k trials. It returns
# false if n is composite and 
# returns false if n is
# probably prime. d is an odd 
# number such that d*2<sup>r</sup> = n-1
# for some r >= 1
def miillerTest(d, n):
     
    # Pick a random number in [2..n-2]
    # Corner cases make sure that n > 4
    a = 2 + random.randint(1, n - 4);
 
    # Compute a^d % n
    x = power(a, d, n);
 
    if (x == 1 or x == n - 1):
        return True;
 
    # Keep squaring x while one 
    # of the following doesn't 
    # happen
    # (i) d does not reach n-1
    # (ii) (x^2) % n is not 1
    # (iii) (x^2) % n is not n-1
    while (d != n - 1):
        x = (x * x) % n;
        d *= 2;
 
        if (x == 1):
            return False;
        if (x == n - 1):
            return True;
 
    # Return composite
    return False;
 
# It returns false if n is 
# composite and returns true if n
# is probably prime. k is an 
# input parameter that determines
# accuracy level. Higher value of 
# k indicates more accuracy.
def isPrime( n, k):
     
    # Corner cases
    if (n <= 1 or n == 4):
        return False;
    if (n <= 3):
        return True;
 
    # Find r such that n = 
    # 2^d * r + 1 for some r >= 1
    d = n - 1;
    while (d % 2 == 0):
        d //= 2;
 
    # Iterate given number of 'k' times
    for i in range(k):
        if (miillerTest(d, n) == False):
            return False;
 
    return True;
 
# Driver Code
# Number of iterations
```
Adicionamos a seguinte função para calcular o proximo numero primo:

```py
def getNextPrime(n):
    n += 1 if n%2==0 else 2
    while not isPrime(n, 8):
        n+=2
    return n
```

Com isto bastou correr o seguinte codigo para obter o **p** e **q**:

```py
k = 8
n = 359538626972463181545861038157804946723595395788461314546860162315465351611001926265416954644815072042240227759742786715317579537628833244985694861278997871052684504401596494019122799039009989371808178352060966438579515613360227960308969957859170183912581829276320781354104234997246446865042463594219967161283

origp = getNextPrime(2**512)
origq = getNextPrime(2**513)
p = origp
q = origq

while True:
    while p*q < n:
        q = getNextPrime(q)
        continue
    if p*q == n:
        break
    p = getNextPrime(p)
    q = origq

print("p = ", p)
print("q = ", q)
```
![](../pictures/CTFRSA2.png)
Com p e q, facilmente calculamos o valor que falta, **d**:
```py
phi = (p-1)*(q-1)
d = pow(e, -1, phi)
```

Utilizando a função **dec()** que nos foi fornecida, facilmente decubrimos a flag:
```py
print("flag: ", dec(unhexlify(un_flag), d, n).decode('utf-8', errors='ignore'))
```
![](../pictures/CTFRSA3.png)
