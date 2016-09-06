---
title: Web - evitar impressão e botão Ctrl
description: Aqui eu mostro como bloquear a impressão de tudo, mesmo utilizando o menu Arquivo >> Imprimir do browser ou clicando no botão de atalho Ctrl + P. Passando para a parte do botão Ctrl, dependendo do seu sistema é melhor adicionar uma página “full screen” ou “popup”, assim o usuário não tem como clicar Ctrl + N ou qualquer outra tecla para salvar a página. Quando o usuário clica no botão Ctrl, aparece pra ele uma mensagem falando que essa tecla está proibida e nada é feito.
author: MSCommunityPubService
ms.date: 09/06/2016
ms.topic: how-to-article
ms.service: Web solutions
ms.custom: CommunityDocs
---




#Web - evitar impressão e botão Ctrl


##**Mauricio Junior**
Outubro, 2013

Olá leitor(a), hoje eu gostaria de mostrar aqui como evitar que o usuário imprima a sua página utilizando o browser e como evitar que o mesmo usuário clique no botão Ctrl para salvar o que tem na sua página.

O grande desafio aqui não é bloquear tudo, é bloquear apenas o necessário. Geralmente o bloqueio é feito em partes do sistema, aquela parte que geralmente alguns perfis tem acesso com usuário e senha. 

Aqui eu mostro como bloquear a impressão de tudo, mesmo utilizando o menu Arquivo >> Imprimir do browser ou clicando no botão de atalho Ctrl + P. Passando para a parte do botão Ctrl, dependendo do seu sistema é melhor adicionar uma página “full screen” ou “popup”, assim o usuário não tem como clicar Ctrl + N ou qualquer outra tecla para salvar a página. Quando o usuário clica no botão Ctrl, aparece pra ele uma mensagem falando que essa tecla está proibida e nada é feito. 

A parte da impressão é feita utilizando CSS, enquanto que a parte do Ctrl é feita com JavaScript na página.

Qual a razão de utilizar essas duas funcionalidades? Bom, a minha razão é porque o cliente solicitou isso no sistema interno dele. Ele quer evitar que os outros usuário possam imprimir a página ou o documento específico. Se você leitor(a) quiser usar só a parte do CSS ou a só a parte do JavaScript, fique a vontade para fazer o que for melhor para você.

Lembro que me baseei pesquisando na Internet e juntando algumas peças necessárias.

#Primeiro passo

O primeiro passo é criar um arquivo chamado print.css, simples e fácil. Esse arquivo pode ser criado usando o notepad do Windows sem qualquer dificuldade. Eu resolvi chamar o arquivo de print.css pelo fato de que o arquivo é focado apenas para imprimir. O code 1.1 mostra o código desenvolvido.

```
CSS

body {
}
#naoimprime 
{
    display: none;
}
#imprime
{
    display:block;
}

```
    
![](./img/PowerBI-Aug-013.png)

