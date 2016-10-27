---
title: ASP.NET Web API - HTTP, REST e o ASP.NET
description: Para basear todas as funcionalidades expostas pela tecnologia, precisamos ter um conhecimento básico em relação ao que motivou tudo isso, contando um pouco da história e evolução, passando pela estrutura do protocolo HTTP e a relação que tudo isso tem com o ASP.NET.
author: MSCommunityPubService
ms.author: walteros
ms.date: 10/27/2016
ms.topic: Reference / API
ms.service: ASP.Net Web API
ms.custom: CommunityDocs
---


# ASP.NET Web API - HTTP, REST e o ASP.NET


##**Israel Aece**

Para basear todas as funcionalidades expostas pela tecnologia, precisamos ter um conhecimento básico em relação ao que motivou tudo isso, contando um pouco da história e evolução, passando pela estrutura do protocolo HTTP e a relação que tudo isso tem com o ASP.NET.

Os sistemas que são construídos atualmente produzem toneladas e toneladas de informações, que são armazenadas em uma base de dados, para mais tarde, estas mesmas informações serão consumidas por estas e outras aplicações.

Não existe muitos problemas enquanto há apenas uma única aplicação consumidora destas informações, afinal ela pode consumir diretamente a base de dados para ler e gravar as informações desejadas. Se estamos dentro de um ambiente controlado, como por exemplo, aplicações construídas dentro de uma mesma empresa, podemos utilizar a própria base de dados para “integrar” as mesmas.

Enquanto estamos consumindo informações locais, estamos diretamente conectados a nossa infraestrutura, sem qualquer impecílio mais crítico para acessar e consumir as informações. O dinamismo do mercado faz com que hoje as empresas operem em tempo real, onde as informações que precisam ser consumidas ou disponibilizadas estão além do ambiente interno, ou seja, a necessidade de uma integração real com parceiros de negócios, órgãos governamentais, etc.

Utilizar a base de dados como meio de integração não é mais uma opção. Estamos agora lidando com empresas que estão fora da nossa rede, com aplicações construídas com tecnologias diferentes daquelas que adotamos internamente, padrões diferentes, etc. 

Tudo isso motivou algumas grandes empresas de tecnologia do mundo a trabalharem na construção de forma de integração, criando um padrão predefinido para troca de informações entre tais empresas. Em pouco tempo surgiu o conceito de Web Services, onde a ideia é permitir com que sistemas se conectem a fim de trocar informações entre eles, sem a necessidade da intervenção humana.

Para dar suporte a tudo isso, o XML foi utilizado como forma de expressar essa comunicação entre as partes. O XML é uma linguagem de marcação, e que apesar de ser texto puro, fornece uma gama de outros recursos agregados que o torna bastante poderoso, como por exemplo, a possibilidade de estensível, separar definição de conteúdo, opções para validação de estrutura, simplicidade na leitura (inclusive por humanos).

Como a internet estava cada vez mais difundida, utilizá-la como forma de integração foi uma das principais opções, pois tratavam-se de tecnologias de padrão aberto, como é o caso do HTTP e HTML. O HTTP dá vida aos Web Services, pois é ele que o torna acessível, para que as aplicações interessadas possam acessá-los e trocar as informações. Além disso, o fato de se apoiar em tecnologias de padrão aberto, torna o consumo muito mais simplificado pelas mais diversas linguagens e tecnologias, desde uma aplicação de linha de comando, passando por um navegador e até mesmo um dispositivo móvel.

Sendo o HTTP o responsável pela infraestrutura de comunicação e o XML a linguagem que descreverá a comunicação, ainda era necessário um formato para formalizar a interação entre as partes (produtora e consumidora). Eis que surge o SOAP (Simple Object Access Protocol). A finalidade do SOAP foi padronizar o conteúdo que trafega entre as partes sob o protoloco HTTP. O SOAP é baseado em XML e, consequentemente, é estensível, e em pouco tempo ele ficou popular e foi utilizado em larga escala. Cada empresa utilizou as especificações criadas e regidas por órgãos independentes, criando um ferramental para que seja possível a criação e consumo destes tipos de serviços. A Microsoft fez a sua parte criando os ASP.NET Web Services (ASMX), que seguia as especificações do W3C para a criação e consumo de serviços dentro da plataforma .NET, e isso contribuiu ainda mais para a popularização destes tipos de serviços.

O grande uso destes tipos de serviços motivou a criação de algumas outras tecnologias para incrementar os Web Services, incluindo algumas características referentes a segurança, entrega de mensagens, transações, etc., e então uma série de especificações (WS-*) foram criadas (graças a estensibilidade que o SOAP possibilita), a fim de padronizar cada um destes novos recursos. A Microsoft correu para adequar os ASP.NET Web Services para suportar estas novas funcionalidades, e aí surgiu o WS-E (Web Services Enhancements).

Dentro do universo Microsoft há várias opções para comunicação distruída, onde cada uma tinha um objetivo diferente, uma implementação única e uma API nada comum. Com o intuito de facilitar a comunicação distribuída dentro da plataforma .NET ela construiu o WCF – Windows Communication Foundation. Ele é um dos pilares do .NET Framework, sendo o framework de comunicação para toda a plafaforma. O WCF unifica todas as tecnologias de comunicação distribuídas que a Microsoft tinha até então. A proposta com ele é tornar a construção e consumo de serviços algo simples, onde o foco está apenas nas regras de negócios, e detalhes com a infraestrutura, comunicação, protocolo, etc., seriam poucas configurações a serem realizadas.

O WCF foi construído com o protocolo (HTTP, TCP, MQ) sendo apenas uma forma de comunicação, onde o SOAP é o padrão que define todo o tráfego das mensagens, independentemente do protocolo que seja utilizado para a comunicação.

Como vimos até então, o SOAP se apresenta como a solução ideal para a integração entre os mais variados sistemas, de qualquer tecnologia, justamente por trabalhar com padrões abertos e gerenciados por órgãos independentes. Só que o SOAP foi ganhando uma dimensão em funcionalidades, e grande parte das plataformas e linguagens não conseguiram acompanhar a sua evolução, tornando cada vez mais complicado a sua adoção, pois a cada nova funcionalidade adicionada, uma série de novos recursos precisam serm, também, adicionados nas plataformas/linguagens para suportar isso.

Motivada pela complexidade que o SOAP ganhou ao longo do tempo, e também pela dificuldade na evolução por parte dos produtos e consumidores destes tipos de serviços, uma nova alternativa foi ganhando cada vez mais espaço: o REST (Representational State Transfer). O estilo REST vai contra tudo o que prega o SOAP, ou seja, enquanto o SOAP entende que o HTTP é uma forma de tráfego da informação, o REST abraça o HTTP, utilizando integralmente todos os seus recursos.

Orientado à recursos, o estilo REST define o acesso à elementos como se eles fossem um conjunto predefinido de informações quais queremos interagir (acessível através de sua URI), extraindo e/ou criando estes recursos, diferindo do estilo baseado em RPC (Remote Procedure Call) que usávamos até então, que define que o acesso é à alguma funcionalidade.

Novamente, correndo atrás do que o mercado estava utilizando como principal meio de comunicação e integração, a Microsoft começou a trabalhar em uma nova versão do WCF, já que era o pilar de comunicação da plataforma, para agregar a possibilidade de construir serviços baseados em REST. Passamos então a ter a possibilidade de criar este tipo de serviço no WCF, mas como o mesmo foi construído abstraindo a figura do protocolo (HTTP e outros), ficou complexo demais criar serviços REST sobre ele, já que era necessário entender toda a infraestrutura do WCF para criar um serviço deste tipo, pois para interagir com os elementos do HTTP, tínhamos que lidar com objetos do framework que passava a expor o acesso aos recursos do HTTP (URIs, headers, requisição, resposta, etc.).

Como a criação de serviços REST no WCF acabou ficando mais complexo do que deveria, a Microsoft começou a investir na criação de uma nova forma de construir e consumir serviços REST na plataforma .NET. Inicialmente ele seria uma espécie de agregado ao WCF, mas pela complexidade de manter o modelo de classes que já existia, a Microsoft decidiu criar a vincular isso ao ASP.NET, tornando-o uma plataforma completa para a criação de qualquer funcionalidade para ser exposta via Web, e assim surgiu o ASP.NET Web API. Como toda e qualquer tecnologia, o ASP.NET Web API abstrai vários elementos que são necessários para o funcionamento destes tipos de serviços. Mas isso não é motivo para não entender quais são e para o que eles servem.

Web API nada mais é que uma interface que um sistema expõe através do HTTP para ser acessado pelos mais variados clientes, utilizando as características do próprio protocolo para interagir com o mesmo. E como estamos agora fundamentados no HTTP, é necessário conhecermos alguns elementos que passam a ser essencias para a construção destes serviços. Sendo assim, quando estamos lidando com a Web, toda e qualquer requisição e resposta possuem algumas características específicas que envolvem todo o processo, a saber:

* Recursos: recursos podem ser qualquer coisa que esteja disponível e desejamos acessar. Podemos entender o recurso como uma página HTML, produtos de uma loja, um video, uma imagem e também uma funcionalidade que pode ser acessível através de outras aplicações.
* URIs: Todo e qualquer recurso para ser acessível, precisa ter uma URI que determina o endereço onde ele está localizado. A URI segue a seguinte estrutura: http://Servidor/MusicStore/Artistas?max+pezzali.
* Representação: Basicamete é uma fotografia de um recurso em um determinado momento.
* Media Type: O recurso retornado ao cliente é uma fotografia em um determinado momento, seguindo um determinado formato. O Media Type é responsável por determinado o formato em que a requisição é enviado e como a resposta é devolvida. Alguns media types: “image/png”, “text/plain”, etc.

Se depurarmos uma requisição para qualquer tipo de API via HTTP, podemos enxegar cada um dos elementos elencados acima. Novamente, apesar de não ser necessário entender os “bastidores” da comunicação, o entendimento básico nos dará conhecimento para depurar e encontrar mais facilmente eventuais problemas, entendimento para tirar proveito de alguns recursos que o próprio HTTP nos fornece.

O HTTP é baseado no envio e recebimento de mensagens. Tanto a requisição quanto a resposta HTTP possui a mesma estrutura. Temos o cabeçalho da mensagem, uma linha em branco, e o corpo da mensagem. O cabeçalho é composto por informações inerentes ao endereço do serviço e um conjunto de headers, que nada mais é que um dicionário contendo uma série de informações contextuais à requisição/resposta, que guiará o serviço ou o cliente a como tratar a mensagem.

```
HTML

POST http://localhost:1062/api/clientes HTTP/1.1
User-Agent: Fiddler
Content-Type: application/json
Host: localhost:1062
Content-Length: 48

{"Nome": "Israel", "Email": "ia@israelaece.com"}
```

Na primeira linha vemos a forma (verbo) e para onde (URI) a requisição está sendo encaminhada. Vale lembrar que o verbo tem um significado importante no HTTP, pois ele determina o formato da mensagem e como ela será processada. Vemos também o Content-Type, que determina o formato do conteúdo da mensagem, e que neste caso, está serializado em JSON. Depois da linha em branco temos o conteúdo da mensagem. Se olharmos o todo, vemos que o cliente está interessado em postar (adicionar) um novo cliente chamado “Israel” na coleção de clientes.

Depois de processado pelo cliente, e partindo do princípio que tudo deu certo, uma mensagem de resposta será retornada, e lá teremos informações referente ao processamento da requisição (referente ao sucesso ou a falha):

```
HTML

HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
Location: http://localhost:1062/api/clientes/12
Content-Length: 58

{"Id": 12, "Nome": "Israel", "Email": "ia@israelaece.com"}
```

Estruturalmente a mensagem de resposta é igual a da requisição, com cabeçalho, linha em branco e o corpo. A resposta contém também a coleção de headers, incluindo o Content-Type do corpo, pois a entidade postada está sendo retornada ao cliente informando, através do código de status 201 (Created), que o cliente foi criado com sucesso e o Id que foi gerado e atribuído à ele.

Claro que o HTTP fornece muito mais do que isso que vimos nestes exemplos de requisição e resposta. No decorrer dos próximos capítulos iremos abordarmos com mais detalhes cada um desses elementos e diversos outros recursos que sob demanda iremos explorando e, consequentemente, agregando e exibindo novas funcionalidades que o HTTP fornece.

#Veja também:

[ASP.NET Web API – HTTP, REST e o ASP.NET](https://msdn.microsoft.com/pt-br/library/dn369238.aspx): Para basear todas as funcionalidades expostas pela tecnologia, precisamos ter um conhecimento básico em relação ao que motivou tudo isso, contando um pouco da história e evolução, passando pela estrutura do protocolo HTTP e a relação que tudo isso tem com o ASP.NET.

[ASP.NET Web API – Estrutura da API](https://msdn.microsoft.com/pt-br/library/dn376302.aspx): Entenderemos aqui a template de projeto que o Visual Studio fornece para a construção das APIs, bem como sua estrutura e como ela se relaciona ao protocolo.

[ASP.NET Web API – Roteamento](https://msdn.microsoft.com/pt-br/library/dn376303.aspx): Como o próprio nome diz, o capítulo irá abordar a configuração necessária para que a requisição seja direcionada corretamente para o destino solicitado, preenchendo e validando os parâmetros que são por ele solicitado.

[ASP.NET Web API – Hosting](https://msdn.microsoft.com/pt-br/library/dn376304.aspx): Um capítulo de extrema relevância para a API. É o hosting que dá vida à API, disponibilizando para o consumo por parte dos clientes, e a sua escolha interfere diretamente em escalabilidade, distribuição e gerenciamento. Existem diversas formas de se expor as APIs, e aqui vamos abordar as principais delas.

[ASP.NET Web API – Consumo](https://msdn.microsoft.com/pt-br/library/dn376305.aspx): Como a proposta é ter uma API sendo consumido por qualquer cliente, podem haver os mais diversos meios (bibliotecas) de consumir estas APIs. Este capítulo tem a finalidade de exibir algumas opções que temos para este consumo, incluindo as opções que a Microsoft criou para que seja possível efetuar o consumo por aplicações .NET.

[ASP.NET Web API – Formatadores](https://msdn.microsoft.com/pt-br/library/dn376306.aspx): Os formatadores desempenham um papel importante na API. São eles os responsáveis por avaliar a requisição, extrair o seu conteúdo, e quando a resposta é devolvida ao cliente, ele entra em ação novamente para formatar o conteúdo no formato em que o cliente possa entender. Aqui vamos explorar os formatadores padrões que já estão embuitdos, bem como a criação de um novo.

[ASP.NET Web API – Segurança](https://msdn.microsoft.com/pt-br/library/dn376307.aspx): Como a grande maioria das aplicações, temos também que nos preocupar com a segurança das APIs. E quando falamos de aplicações distribuídas, além da autenticação e autorização, é necessário nos preocuparmos com a segurança das mensagens que são trocadas entre o cliente e o serviço. Este capítulo irá abordar algumas opções que temos disponíveis para tornar as APIs mais seguras.

[ASP.NET Web API – Testes e Tracing](https://msdn.microsoft.com/pt-br/library/dn376309.aspx): Para toda e qualquer aplicação, temos a necessidade de escrever testes para garantir que a mesma se comporte conforme o esperado. Isso não é diferentes com APIs Web. Aqui iremos abordar os recursos, incluindo a própria IDE, para a escrita, gerenciamento e execução dos testes.

[ASP.NET Web API – Estensibilidade e Arquitetura](https://msdn.microsoft.com/pt-br/library/dn376308.aspx): Mesmo que já tenhamos tudo o que precisamos para criar e consumir uma API no ASP.NET Web API, a customização de algum ponto sempre acaba sendo necessária, pois podemos criar mecanismos reutilizáveis, “externalizando-os” do processo de negócio em si. O ASP.NET Web API foi concebido com a estensibilidade em mente, e justamente por isso que existe um capítulo exclusivo para abordar esse assunto.
