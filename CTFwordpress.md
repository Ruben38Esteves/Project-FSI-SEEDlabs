#Wordpress CVE

## FLAG 1

Ao aceder ao site que estava a ser hospedado no WordPress começamos imediatamente a procurar as versoes do wordpress e qualquer tipo de plugins que estivessem a ser utilizadas para depois procurar uma CVE associada e encontramos o seguinte:

Versão do wordpress: 5.8.1;
Plugins instalados: 
    -WooCommerce plugin, 5.7.1;
    -Booster for WooCommerce plugin, 5.4.3;
Possíveis utilizadores: Orval Sanford;

Apos pesquisar por CVEs associadas encontramos a seguinte: CVE-2021-34646. Esta CVE mencionava na sua descriçao a possibilidade de fazer login com outros ultilizadores, incluindo administradores.

Demos entao input de "flag{CVE-2021-34646}" e conseguimos assim a primeira flag.

## FLAG 2

Sabendo agora a CVE, pesquisamos no google possíveis exploits para esta CVE e encontramos um script em Python que quando executado nos permitiu aceder como administrador e entao encontrar a flag final "flag{please don't bother me}"
