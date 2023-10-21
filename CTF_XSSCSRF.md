# XSS CSRF CTF

## Parte 1 - Investigaçao

Entrando no link http://ctf-fsi.fe.up.pt:5004/ encontramos a seguinte pagina: 

![](../pictures/CTF6mainpage.png)

Clicando em submit, somos redirecionados para a seguinte pagina:

![](../pictures/CTF6just.png)

Note-se que ao url foi acrescentado "request/<requestid>", sendo o <requestid> o id que aparecia na primeira pagina.
Esta pagina tem um link apara a pagina que o admin ira usar para aprovar o nosso pedido:

![](../pictures/CTF6adminpage.png)

Olhando com atençao para o URL, verificamos que a porta mudou de 5004 para 5005.
Inspecionando o codigo fonte da pagina, encontramos o form de aceitaçao:

![](../pictures/CTF6acceptform.png)

## Parte 2 - Captura da Bandeira

Sabendo isto, escrevemos o nosso proprio form de aceitaçao:

```html
<form method="POST" action="http://ctf-fsi.fe.up.pt:5005/request/<requestid>/approve" role="form" hidden>
    <div class="submit">
        <input type="submit" id="giveflag" value="Give the flag">
    </div>
    <script>
        document.getElementById('giveflag').click();
    </script>
</form>
```
(Sendo <requestid> o id que se encontrou na primeira pagina)

Assim, ao correr isto, o admin sera direcionado para a pagina de aceitaçao e ira "clicar" no botao "giveflag".

Introduzimos isto na caixa de texto da paginal inicial, clicamos em submit e fomos direcionados para uma pagina que nos dizia que nao tinhamos permissao para aceder:

![](../pictures/CTF6error.png)

Ou seja, o que nós precisavamos era que o admin seja redirecionado mas nós nao. Para isso basta desligar o Javascript no nosso browser.
Voltando a escrever o nosso form html na pagina inicial voltamos outra vez aqui:

![](../pictures/CTF6just.png)

Olhando para o codigo fonte desta pagina encontramos o seguinte:

```html
<script>
    setTimeout(function(){window.location.reload();}, 5000);
</script>
```
Este script em Javascript é responsavel por dar reload à pagina mas visto que nos desligamos o Javascript, ele nao vai correr.
Démos entao reload manualmente à pagina e encontramos o seguinte:

![](../pictures/CTF6flag.png)

Vitoria, encontramos a flag.
Após introduzida em ctf-fsi.fe.up.pt, verificamos que estava correta!
