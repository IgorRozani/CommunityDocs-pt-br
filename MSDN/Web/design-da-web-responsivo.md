---
title: Design da Web responsivo
description: Tudo começou com Design da Web responsivo, um artigo de Ethan Marcotte em A List Apart. Essencialmente, o artigo propunha lidar com o panorama de dispositivos, navegadores, tamanhos e orientações de tela em constante mudança através da criação de sites flexíveis, fluidos e adaptáveis.
author: MSCommunityPubService
ms.date: 10/23/2016
ms.topic: how-to-article
ms.service: Web solutions
ms.custom: CommunityDocs
---


#Design da Web responsivo

##**Ethan Marcotte**
Web developer

Tudo começou com Design da Web responsivo, um artigo de Ethan Marcotte em A List Apart. Essencialmente, o artigo propunha lidar com o panorama de dispositivos, navegadores, tamanhos e orientações de tela em constante mudança através da criação de sites flexíveis, fluidos e adaptáveis. Em vez de responder às necessidades de hoje de uma versão Web adaptada de desktop, junto com uma determinada versão móvel (frequentemente específica de um único dispositivo móvel), a ideia é abordar a questão ao contrário: usar layouts flexíveis e fluidos para se adaptarem a quase qualquer tela.

#Conceitos básicos

Três recursos técnicos principais são a alma do design de Web responsivo:

* Consultas de mídia e ouvintes de consultas de mídia
* Um layout flexível baseado em grade que usa dimensionamento relativo
* Imagens e mídia flexíveis, através do redimensionamento dinâmico ou CSS

O design de Web verdadeiramente responsivo exige que os três recursos sejam implantados

O ponto principal é a adaptação às necessidades do usuário e às capacidades do dispositivo. Imagine que um usuário móvel verá o seu site em uma tela pequena. Levar as necessidades do usuário em consideração não significa apenas adaptar seu conteúdo ao tamanho da tela. Significa também pensar no que aquele usuário móvel precisará primeiro quando visitar seu site e então elaborar o conteúdo de acordo. Talvez você apresente as informações em uma ordem diferente. Não presuma que o usuário não precisará de acesso a todas as informações do site só porque está em um dispositivo móvel. Você pode precisar mudar as fontes ou as áreas de interação para responder melhor a um ambiente de toque. Todos esses fatores influenciam o design de Web responsivo.

Embora os dispositivos móveis estejam mudando o panorama das telas, com o aparecimento crescente de mais e mais telas pequenas, não se esqueça do que está acontecendo do outro lado do espectro. Telas também estão se tornando cada vez maiores. Precisar atender aos dois segmentos não deveria impedir designers de serem inovadores em nenhum deles. 

Consultas de mídia

Começando com CSS 2.1, tipos de mídia eram usados para aplicara CSS tanto para tela como para impressão. Você deve se lembrar destes tipos de mídia:

```CSS
<link rel="stylesheet" type="text/css" href="style.css" media="screen" />'
<link rel="stylesheet" type="text/css" href="printfriendly.css" media="print" />
```
Era assim! Felizmente, o W3C melhorou [consultas de mídia](http://www.w3.org/TR/css3-mediaqueries/) no CSS3, levando-as um passo adiante.

Hoje, você pode usar consultas de mídia para definir estilos para capacidades específicas, aplicando diferentes estilos com base nas capacidades que correspondam à sua consulta. Pode até combinar consultas e testar diversos recursos usando operadores semânticos (como AND e NOT). Recursos incluem largura, altura, largura máxima, altura máxima, altura do dispositivo, orientação, taxa de proporção, resolução e outros.

Há três maneiras de se implementar consultas de mídia:



