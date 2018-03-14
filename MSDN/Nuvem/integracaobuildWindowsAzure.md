---
redirect_url: https://docs.microsoft.com/
title: Como fazer Integração Contínua de Build no Windows Azure usando o Visual Studio Online?
description: Neste artigo vou mostrar um passo a passo de como criar uma conta e usar o VS Online, integrar no Windows Azure, usar o Visual Studio 2013 para criar um projeto, fazer o deploy no Azure através do VS Online e explorar as opções disponíveis no site do projeto no Azure para um melhor acompanhamento e gerenciamento. E, é claro, quais as ferramentas disponíveis no VS 2013 para automatizar esta integração.
author: MSCommunityPubService
ms.author: walteros
ms.date: 09/30/2016
ms.topic: article
ms.prod: 
ms.technology: 
ms.service: Windows Forms
ms.custom: CommunityDocs
---

#PComo fazer Integração Contínua de Build no Windows Azure usando o Visual Studio Online?

##**Renato Haddad**
**MVP ASP.NET /MCP/MCTS/MCPD/RD**
Março, 2014

[Blog](http://weblogs.asp.net/renatohaddad/)

Que tal armazenar a sua aplicação na nuvem usando o serviço do Windows Azure para controlar o ciclo de vida e a implantação? 
Agora a Microsoft tem um serviço chamado Visual Studio Online que é totalmente integrado ao Visual Studio 2013, o qual permite controlar 
o versionamento de códigos, gerenciar os Builds, implementar e controlar o ciclo de vida do desenvolvimento da aplicação no time através 
de workflow, usar todos os controles de tarefas (work items), ou seja, para quem usa o famoso Team Foundation Server (TFS), agora tem 
todas opções online no Azure.
Neste artigo vou mostrar um passo a passo de como criar uma conta e usar o VS Online, integrar no Windows Azure, usar o Visual Studio 2013 
para criar um projeto, fazer o deploy no Azure através do VS Online e explorar as opções disponíveis no site do projeto no Azure para um 
melhor acompanhamento e gerenciamento. E, é claro, quais as ferramentas disponíveis no VS 2013 para automatizar esta integração.
Os pré-requisitos para este artigo são o Visual Studio 2013 e o Windows Azure SDK, assim como uma conta para ambos. Caso não tenha 
algum destes, faça o download diretamente do site da Microsoft (http://www.windowsazure.com/pt-br/downloads/).


