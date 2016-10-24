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

1.Usar a regra @import para importar regras de estilo de outras folhas de estilo:
2.@import url(style600min.css) screen and (min-width: 600px);
3.Colocar consultas de mídia diretamente na folha de estilos, como mostrado na exemplo. Esta é a abordagem mais comum.

Exemplo - Implementação de consultas de mídia diretamente em uma folha de estilos.

```
#nav
    {
        float: right;
    }
        #nav ul
        {
            list-style: none;
        }
    @media screen and (min-width: 400px) and (orientation: portrait)
        {
                #nav li
                {
                    float: right;
                    margin: 0 0 0 .5em;
                    border:1px solid #000000;
                }
        }
    @media screen and (min-width: 800px)
        {
            #nav
            {
                width: 200px;
            }
               #nav li
                {
                    float: left;
                    margin: 0 0 0 .5em;
                    border: none;               
                }
        }
```
Incluir uma consulta em um atributo de mídia de folha de estilos vinculado.

```
<link rel="stylesheet" type="text/css" media="screen and (max-device-width: 800px)" href="style800.css" />
```
Por causa da natureza (em cascata da CSS, estilos padrão são definidos no alto com as regras e estilos correspondentes da consulta de mídia abaixo). Estilos definidos no alto serão lançados em cascata para os estilos correspondentes na regra, ou até mesmo totalmente sobrescritos.

As imagens a seguir apresentam um exemplo de uma abordagem de design de Web responsivo que usa consultas de mídia. A Figura 1 e a Figura 2 mostram uma área de trabalho usando o Internet Explorer 9 em duas resoluções diferentes. A Figura 3 mostra o mesmo site responsivo em um Windows Phone, também com o Internet Explorer 9.

![Img](img/IC650128-01.png)

Figura 1 - A navegação aparece à esquerda.

![Img](img/IC650128-02.png)

Figura 2 - Em uma janela redimensionada para 800x600, a navegação passa para o topo.

![Img](img/IC650128-03.png)

Figura 3 - O mesmo site em um Windows Phone

Se estiver procurando alguns exemplos ótimos de design de Web responsivo que tiram proveito total de consultas de mídia, o site de entusiastas (http://mediaqueri.es/) pode ser viciante, como mostra a Figura 4.

![Img](img/IC650128-04.png)


Figura 4 - Um conjunto de sites que usam consultas de mídia

#Ouvintes de consultas de mídia

Levando as consultas de mídia um passo adiante, o grupo de trabalho CSS Object Model (CSSOM) do W3C também criou ouvintes de consultas de mídia, que fornecem uma API para responder a mudanças nas consultas de mídia. Em vez de ter de pesquisar mudanças ou carregar várias versões de um recurso, você pode usar a API, por exemplo, para baixar imagens apenas de um determinado tamanho quando uma correspondência de consulta de mídia é disparada.

Hoje, o Firefox e o the [Internet Explorer 10 Platform Preview](https://msdn.microsoft.com/library/hh673538.aspx#_DOMMediaQuery) implementam ouvintes de consultas de mídia; você pode ver a demonstração “Consultas de mídia CSS3 e Ouvintes de consultas de mídia” no test-drive do IE.

#Uma palavra sobre o Viewport

Quando testa consultas de mídia em navegadores móveis, você pode notar que as consultas corretas não estão sendo realmente aplicadas. Quando isso acontece, o navegador móvel está fazendo um trabalho em seu nome para renderizar a página de forma ideal na tela menor.

Então você acha que não há uma forma de se obter a resolução real? Na verdade, há, na metatag viewport. A metatag viewport controla as dimensões lógicas e o dimensionamento da janela do navegador móvel (Chromeless). Definir width como device-width contorna o problema:

```
<meta name="viewport" content="width=device-width">
```
Outras configurações de viewport incluem maximum-zoom e initial-scale.

#Grades flexíveis

Um layout baseado em grade flexível é uma das pedras angulares do design responsivo. O termo "grade" é usado com bastante liberdade e não implica em um requisito de se implementar qualquer das estruturas de grade disponíveis. O que isso significa aqui é usar a CSS para posicionar e definir margens e espaçamento e para implementar vários [tipos de layout da Web](http://sixrevisions.com/web_design/a-guide-on-layout-types-in-web-design/) de uma forma nova. Layouts e tamanhos de texto são tipicamente expressos em pixels. Designers adoram pixels. O Photoshop adora pixels. Mas um pixel pode ser um ponto em um dispositivo e oito pontos em outro. Então como se aborda o design de Web responsivo se tudo se baseia em pixels? Você pode não gostar da resposta. Você para de usar layouts baseados em pixels e começa a usar porcentagens ou o em para dimensionamento.

Baseando os tamanhos de texto, larguras e margens em porcentagens ou no em, uma unidade de medida baseada no tamanho do ponto de uma fonte, você pode transformar um tamanho fixo em tamanho relativo. Isso significa que você precisará fazer alguns pequenas contas para obter um sistema de grade e de tamanho de texto flexíveis. Mas a fórmula para calcular o em é bem simples:

destino ÷ contexto = resultado

Digamos que o contexto normal para o tamanho da fonte do corpo seja de 16 pixels. Se o designer especificar que a H1 deve ser de 24 pixels, você pode calcular o seguinte:

24 ÷ 16 = 1.5

Isso resulta no estilo de CSS a seguir:

```
h1
{
        font-size: 1.5em;
}
```
Internet - Design da Web responsivo 




Ethan Marcotte

Web Developer

Março 2013

Tudo começou com Design da Web responsivo, um artigo de Ethan Marcotte em A List Apart. Essencialmente, o artigo propunha lidar com o panorama de dispositivos, navegadores, tamanhos e orientações de tela em constante mudança através da criação de sites flexíveis, fluidos e adaptáveis. Em vez de responder às necessidades de hoje de uma versão Web adaptada de desktop, junto com uma determinada versão móvel (frequentemente específica de um único dispositivo móvel), a ideia é abordar a questão ao contrário: usar layouts flexíveis e fluidos para se adaptarem a quase qualquer tela.

Conceitos básicos

Três recursos técnicos principais são a alma do design de Web responsivo:
•Consultas de mídia e ouvintes de consultas de mídia
•Um layout flexível baseado em grade que usa dimensionamento relativo
•Imagens e mídia flexíveis, através do redimensionamento dinâmico ou CSS

O design de Web verdadeiramente responsivo exige que os três recursos sejam implantados

O ponto principal é a adaptação às necessidades do usuário e às capacidades do dispositivo. Imagine que um usuário móvel verá o seu site em uma tela pequena. Levar as necessidades do usuário em consideração não significa apenas adaptar seu conteúdo ao tamanho da tela. Significa também pensar no que aquele usuário móvel precisará primeiro quando visitar seu site e então elaborar o conteúdo de acordo. Talvez você apresente as informações em uma ordem diferente. Não presuma que o usuário não precisará de acesso a todas as informações do site só porque está em um dispositivo móvel. Você pode precisar mudar as fontes ou as áreas de interação para responder melhor a um ambiente de toque. Todos esses fatores influenciam o design de Web responsivo.

Embora os dispositivos móveis estejam mudando o panorama das telas, com o aparecimento crescente de mais e mais telas pequenas, não se esqueça do que está acontecendo do outro lado do espectro. Telas também estão se tornando cada vez maiores. Precisar atender aos dois segmentos não deveria impedir designers de serem inovadores em nenhum deles. 

Consultas de mídia

Começando com CSS 2.1, tipos de mídia eram usados para aplicara CSS tanto para tela como para impressão. Você deve se lembrar destes tipos de mídia:



CSS




Copiar


<link rel="stylesheet" type="text/css" href="style.css" media="screen" />
<link rel="stylesheet" type="text/css" href="printfriendly.css" media="print" />


Era assim! Felizmente, o W3C melhorou consultas de mídia no CSS3, levando-as um passo adiante.

Hoje, você pode usar consultas de mídia para definir estilos para capacidades específicas, aplicando diferentes estilos com base nas capacidades que correspondam à sua consulta. Pode até combinar consultas e testar diversos recursos usando operadores semânticos (como AND e NOT). Recursos incluem largura, altura, largura máxima, altura máxima, altura do dispositivo, orientação, taxa de proporção, resolução e outros.

Há três maneiras de se implementar consultas de mídia:
1.Usar a regra @import para importar regras de estilo de outras folhas de estilo:
2.@import url(style600min.css) screen and (min-width: 600px);
3.Colocar consultas de mídia diretamente na folha de estilos, como mostrado na exemplo. Esta é a abordagem mais comum.

Exemplo - Implementação de consultas de mídia diretamente em uma folha de estilos.



CSS




Copiar


#nav
    {
        float: right;
    }
        #nav ul
        {
            list-style: none;
        }
    @media screen and (min-width: 400px) and (orientation: portrait)
        {
                #nav li
                {
                    float: right;
                    margin: 0 0 0 .5em;
                    border:1px solid #000000;
                }
        }
    @media screen and (min-width: 800px)
        {
            #nav
            {
                width: 200px;
            }
               #nav li
                {
                    float: left;
                    margin: 0 0 0 .5em;
                    border: none;               
                }
        }


Incluir uma consulta em um atributo de mídia de folha de estilos vinculado



CSS




Copiar


<link rel="stylesheet" type="text/css" media="screen and (max-device-width: 800px)" href="style800.css" />


Por causa da natureza (em cascata da CSS, estilos padrão são definidos no alto com as regras e estilos correspondentes da consulta de mídia abaixo). Estilos definidos no alto serão lançados em cascata para os estilos correspondentes na regra, ou até mesmo totalmente sobrescritos.

As imagens a seguir apresentam um exemplo de uma abordagem de design de Web responsivo que usa consultas de mídia. A Figura 1 e a Figura 2 mostram uma área de trabalho usando o Internet Explorer 9 em duas resoluções diferentes. A Figura 3 mostra o mesmo site responsivo em um Windows Phone, também com o Internet Explorer 9.

Dn163510.0FB2C0890E9B77214E202BCB55646BEF(pt-br,MSDN.10).png

Figura 1 - A navegação aparece à esquerda

Dn163510.2BA92441D11BD1BC8046825E521DB607(pt-br,MSDN.10).png

Figura 2 - Em uma janela redimensionada para 800x600, a navegação passa para o topo

Dn163510.ABC1F5808E57FD63666C7298845CAD79(pt-br,MSDN.10).png

Figura 3 - O mesmo site em um Windows Phone

Se estiver procurando alguns exemplos ótimos de design de Web responsivo que tiram proveito total de consultas de mídia, o site de entusiastas http://mediaqueri.es/ pode ser viciante, como mostra a Figura 4.

Dn163510.9C726020E028D9DC5D730DC8B9547F7A(pt-br,MSDN.10).png

Figura 4 - Um conjunto de sites que usam consultas de mídia

Ouvintes de consultas de mídia

Levando as consultas de mídia um passo adiante, o grupo de trabalho CSS Object Model (CSSOM) do W3C também criou ouvintes de consultas de mídia, que fornecem uma API para responder a mudanças nas consultas de mídia. Em vez de ter de pesquisar mudanças ou carregar várias versões de um recurso, você pode usar a API, por exemplo, para baixar imagens apenas de um determinado tamanho quando uma correspondência de consulta de mídia é disparada.

Hoje, o Firefox e o the Internet Explorer 10 Platform Preview implementam ouvintes de consultas de mídia; você pode ver a demonstração “Consultas de mídia CSS3 e Ouvintes de consultas de mídia” no test-drive do IE.

Uma palavra sobre o Viewport

Quando testa consultas de mídia em navegadores móveis, você pode notar que as consultas corretas não estão sendo realmente aplicadas. Quando isso acontece, o navegador móvel está fazendo um trabalho em seu nome para renderizar a página de forma ideal na tela menor.

Então você acha que não há uma forma de se obter a resolução real? Na verdade, há, na metatag viewport. A metatag viewport controla as dimensões lógicas e o dimensionamento da janela do navegador móvel (Chromeless). Definir width como device-width contorna o problema:



metatag




Copiar


<meta name="viewport" content="width=device-width">


Outras configurações de viewport incluem maximum-zoom e initial-scale.

Grades flexíveis

Um layout baseado em grade flexível é uma das pedras angulares do design responsivo. O termo "grade" é usado com bastante liberdade e não implica em um requisito de se implementar qualquer das estruturas de grade disponíveis. O que isso significa aqui é usar a CSS para posicionar e definir margens e espaçamento e para implementar vários tipos de layout da Web de uma forma nova. Layouts e tamanhos de texto são tipicamente expressos em pixels. Designers adoram pixels. O Photoshop adora pixels. Mas um pixel pode ser um ponto em um dispositivo e oito pontos em outro. Então como se aborda o design de Web responsivo se tudo se baseia em pixels? Você pode não gostar da resposta. Você para de usar layouts baseados em pixels e começa a usar porcentagens ou o em para dimensionamento.

Baseando os tamanhos de texto, larguras e margens em porcentagens ou no em, uma unidade de medida baseada no tamanho do ponto de uma fonte, você pode transformar um tamanho fixo em tamanho relativo. Isso significa que você precisará fazer alguns pequenas contas para obter um sistema de grade e de tamanho de texto flexíveis. Mas a fórmula para calcular o em é bem simples:

destino ÷ contexto = resultado

Digamos que o contexto normal para o tamanho da fonte do corpo seja de 16 pixels. Se o designer especificar que a H1 deve ser de 24 pixels, você pode calcular o seguinte:

24 ÷ 16 = 1.5

Isso resulta no estilo de CSS a seguir:

