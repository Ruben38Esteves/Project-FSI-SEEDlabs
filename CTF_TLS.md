# TLS

Começamos por procurar o handshake cujo numero aleatório fosse o desejado. utilizamos o seguinte filtro

![](../pictures/tls_pic1.png)

Com o handshake encontrado, vamos analisa-lo para compor a flag.
Como o handshake e o frame inicial, sabemos que frame_star= 814

![](../pictures/tls_pic2.png)

procurando o frame 814, conseguimos ver tudo o que foi trocado durante o handshake, e perceber que o frame_end será 819, onde a mensagem é New Session Ticket...

![](../pictures/tls_pic3.png)

analisando o packet 814, podemos ver selected_cipher_suite

![](../pictures/tls_pic4.png)

analisando o frame imediatamente após a concretizaçao do handshake, conseguimos chegar ao valor de total_encryped_appdata_exhange (151+1255).

![](../pictures/tls_pic5.png)

por fim, voltamos ao frame 819, que analisando podemos ver a length da mensagem enciptada:

![](../pictures/tls_pic6.png)

Compoe-se assim a flag{814-819-TLS_RSA_WITH_AES_128_CBC_SHA26-1264-80}
