# Trabalho realizado nas Semanas #2 e #3

## Identificação

CVE-2022-22536

Afeta vários produtos SAP, incluindo o SAP NetWeaver Application Server ABAP, SAP NetWeaver Application Server Java, ABAP Platform, SAP Content Server 7.53 e SAP Web Dispatcher.

A vulnerabilidade permite "request smuggling" e "request concatenation"

## Catalogação

A vulnerabilidade foi relatada pela SAP SE.
A data de publicação inicial foi em 9 de fevereiro de 2022, com uma atualização em 9 de janeiro de 2023.
Nível de gravidade 10.0

## Exploit

Através de "request smuggling" e "request concatenation" atacantes podem anexar requests aos requestuest legítimos do utilizador.

Um ataque bem sucedido pode resultar em quebras de confidencialidade, integridade e disponibilidade do sistema.

## Ataques

A vulnerabilidade foi corrigida antes que fossem feitos quaisquer ataques.
