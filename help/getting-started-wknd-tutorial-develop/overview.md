---
title: Introdução ao AEM Sites - Tutorial WKND
description: Saiba como implementar um site de AEM para uma marca fictícia de estilo de vida chamada WKND. Obtenha uma apresentação sobre tópicos fundamentais do Experience Manager, como configuração de projetos, arquétipos maven, Componentes principais, Modelos editáveis, bibliotecas de clientes e desenvolvimento de componentes.
topics: development
version: Cloud Service
activity: develop
audience: developer
jira: KT-13565
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: 5f43db64f68218ed3d349508a2c4fd232601a9ef
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 7%

---

# Introdução ao AEM Sites - Tutorial WKND {#introduction}

Bem-vindo a um tutorial em várias partes projetado para desenvolvedores novos no Adobe Experience Manager (AEM). Este tutorial aborda a implementação de um site AEM para uma marca fictícia de estilo de vida, a WKND. O tutorial abrange tópicos fundamentais como configuração de projetos, componentes principais, modelos editáveis, bibliotecas do lado do cliente e desenvolvimento de componentes com o Adobe Experience Manager Sites.

## Visão geral {#wknd-tutorial-overview}

O objetivo deste tutorial em várias partes é ensinar o desenvolvedor a implementar um site usando os mais recentes padrões e tecnologias no Adobe Experience Manager (AEM). Após concluir este tutorial, um desenvolvedor deve entender a base básica da plataforma e os padrões de design comuns no AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Opções para iniciar um projeto do Sites

Há duas abordagens básicas para iniciar um projeto AEM Sites.

**Arquétipo de projeto AEM** - Abordagem tradicional para o desenvolvimento do AEM, gerando um projeto AEM mínimo usando um modelo Maven. Esta é a abordagem recomendada para projetos AEM 6.5/6.4 e projetos AEM as a Cloud Service que antecipam uma personalização pesada. O tutorial oferece um mergulho mais profundo no desenvolvimento do AEM.

[Inicie o tutorial com o Arquétipo de projeto AEM](./project-archetype/overview.md)

**Modelos de site do AEM** - Também conhecida como Criação rápida de sites, uma abordagem low-code para gerar um site AEM usando um modelo de site predefinido. Use componentes e modelos prontos para uso para ativar e executar rapidamente um site. Use um fluxo de trabalho de temas para aplicar estilos e personalizações específicos da marca apenas com CSS e JavaScript. Recomendado para novos projetos e desenvolvedores. Disponível somente para AEM as a Cloud Service.

[Iniciar o tutorial usando um modelo de site](./site-template/create-site.md)

## Kit de interface do usuário do Adobe XD

Para tornar este tutorial mais próximo de um cenário do mundo real, os designers de UX talentosos da Adobe criaram os modelos para o site usando [Adobe XD](https://www.adobe.com/products/xd.html). Ao longo do tutorial, várias partes dos projetos são implementadas em um site de AEM totalmente autorável. Muito obrigado a **Lorenzo Buosi** e **Kilian Amendola** que criou o belo design para o site WKND.

Baixe os kits de interface do usuário do XD:

* [Kit de interface do usuário dos Componentes principais do AEM](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit de interface do usuário do WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## Site de referência {#reference-site}

Uma versão concluída do site da WKND também está disponível como referência: [https://wknd.site/](https://wknd.site/)

O tutorial aborda as principais habilidades de desenvolvimento necessárias para um desenvolvedor de AEM, mas *não* criar todo o site de ponta a ponta. O site de referência concluído é outro grande recurso para explorar e ver mais do AEM recursos prontos para uso.

Para testar o código mais recente antes de entrar no tutorial, baixe e instale o **[versão mais recente do GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Desenvolvido pela Adobe Stock

Muitas das imagens no site de referência da WKND são de [Adobe Stock](https://stock.adobe.com/) e são Materiais de terceiros conforme definido nos Termos adicionais do ativo de demonstração em [https://www.adobe.com/legal/terms.html](https://www.adobe.com/br/legal/terms.html). Se você quiser usar uma imagem do Adobe Stock para outros propósitos além da exibição deste site de demonstração, como exibi-la em um site ou em materiais de marketing, é possível adquirir uma licença no Adobe Stock.

Com o Adobe Stock, você tem acesso a mais de 140 milhões de imagens de alta qualidade e isentas de royalties, incluindo fotos, gráficos, vídeos e modelos para dar o pontapé inicial nos seus projetos criativos.

## Próximas etapas {#next-steps}

O que você está esperando?! Saiba como [gerar um novo projeto do Adobe Experience Manager usando o Arquétipo de projeto AEM](./project-archetype/overview.md) ou [criar um site usando um Modelo de site](./site-template/create-site.md).
