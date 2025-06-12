---
title: 'Introdução ao AEM Sites: tutorial WKND'
description: Saiba como implementar um site do AEM para uma marca fictícia de estilo de vida, a WKND. Obtenha um passo a passo com tópicos fundamentais do Experience Manager, como configuração de projetos, arquétipos do Maven, componentes principais, modelos editáveis, bibliotecas de clientes e desenvolvimento de componentes.
version: Experience Manager as a Cloud Service
jira: KT-13565
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
doc-type: Catalog
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: dbf63f30ccfd06e4f4d7883c2f7bc4ac78245364
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 100%

---

# Introdução ao AEM Sites: tutorial WKND {#introduction}

{{traditional-aem}}

Bem-vindo a um tutorial em várias partes projetado para desenvolvedores novatos no Adobe Experience Manager (AEM). Este tutorial aborda a implementação de um site do AEM para uma marca fictícia de estilo de vida, a WKND. O tutorial abrange tópicos fundamentais como configuração de projetos, componentes principais, modelos editáveis, bibliotecas do lado do cliente e desenvolvimento de componentes com o Adobe Experience Manager Sites.

## Visão geral {#wknd-tutorial-overview}

O objetivo deste tutorial é ensinar ao desenvolvedor como implementar um site com base nos padrões e nas tecnologias mais recentes do Adobe Experience Manager (AEM). Após concluir este tutorial, o desenvolvedor entenderá os aspectos básicos da plataforma e os padrões de design comuns do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Opções para iniciar um projeto do Sites

Há duas abordagens básicas para iniciar um projeto do AEM Sites.

**Arquétipo de projeto do AEM**: abordagem tradicional ao desenvolvimento no AEM por meio da geração de um projeto mínimo do AEM, usando-se um modelo do Maven. Esta é a abordagem recomendada para projetos do AEM 6.5/6.4 e projetos do AEM as a Cloud Service que preveem uma personalização intensa. O tutorial oferece um grande aprofundamento no desenvolvimento no AEM.

[Iniciar tutorial com o arquétipo de projeto do AEM](./project-archetype/overview.md)

**Modelos de site do AEM**: também conhecido como criação rápida de sites, uma abordagem low-code para gerar um site do AEM com base em um modelo de site predefinido. Use componentes e modelos prontos para uso para montar e publicar um site rapidamente. Use um fluxo de trabalho de tema para aplicar estilos e personalizações específicos da marca apenas com CSS e JavaScript. Recomendado para novos projetos e desenvolvedores. Disponível somente para o AEM as a Cloud Service.

[Iniciar tutorial com um modelo de site](./site-template/create-site.md)

## Kit da interface do Adobe XD

Para aproximar este tutorial de um caso do mundo real, os talentosos designers de UX da Adobe criaram os modelos para o site por meio do [Adobe XD](https://www.adobe.com/products/xd.html). No decorrer do tutorial, várias partes dos designs são implementadas em um site do AEM totalmente criável. Agradecimentos especiais a **Lorenzo Buosi** e a **Kilian Amendola**, que criaram um belo design para o site da WKND.

Baixe os kits da interface do XD:

* [Kit da interface dos componentes principais do AEM](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit da interface da WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## Site de referência {#reference-site}

Uma versão acabada do site da WKND também está disponível como referência: [https://wknd.site/](https://wknd.site/)

O tutorial aborda as principais habilidades de desenvolvimento necessárias para um desenvolvedor do AEM, mas *não* criará todo o site de ponta a ponta. O site de referência acabado é outro grande recurso para explorar e ver mais dos recursos prontos para uso do AEM.

Para testar o código mais recente antes de começar o tutorial, baixe e instale a **[última versão do GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Viabilizado pelo Adobe Stock

Muitas das imagens no site de referência da WKND provêm do [Adobe Stock](https://stock.adobe.com/) e são materiais de terceiros, conforme definido nos Termos adicionais dos ativos de demonstração, em [https://www.adobe.com/legal/terms.html](https://www.adobe.com/br/legal/terms.html). Se você quiser usar uma imagem do Adobe Stock para outros propósitos além da exibição deste site de demonstração, como exibi-la em um site ou em materiais de marketing, é possível adquirir uma licença no Adobe Stock.

Com o Adobe Stock, você tem acesso a mais de 140 milhões de imagens de alta qualidade e isentas de royalties, incluindo fotos, gráficos, vídeos e modelos, para dar o pontapé inicial nos seus projetos de criação.

## Próximas etapas {#next-steps}

O que você está esperando? Saiba como [gerar um novo projeto do Adobe Experience Manager com o arquétipo de projeto do AEM](./project-archetype/overview.md) ou [criar um site com um modelo de site](./site-template/create-site.md).
