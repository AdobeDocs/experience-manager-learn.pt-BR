---
title: Introdução ao AEM Sites - Tutorial do WKND
description: Introdução ao AEM Sites - Tutorial do WKND. O tutorial WKND é um tutorial em várias partes projetado para desenvolvedores novos no Adobe Experience Manager. O tutorial aborda a implementação de um site de AEM para uma marca fictícia de estilo de vida, a WKND. O tutorial aborda tópicos fundamentais como configuração de projeto, arquétipos de maven, Componentes principais, Modelos editáveis, bibliotecas de clientes e desenvolvimento de componentes.
sub-product: sites
topics: development
version: Cloud Service
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
exl-id: 09a600f4-1ada-4fb7-ae44-586364cff389
recommendations: disable
source-git-commit: 04096fe3c99cdcce2d43b2b29899c2bbe37ac056
workflow-type: tm+mt
source-wordcount: '624'
ht-degree: 6%

---

# Introdução ao AEM Sites - Tutorial do WKND {#introduction}

Bem-vindo a um tutorial de várias partes projetado para desenvolvedores novos no Adobe Experience Manager (AEM). Este tutorial aborda a implementação de um site de AEM para uma marca fictícia de estilo de vida na WKND. O tutorial aborda tópicos fundamentais como configuração de projeto, Componentes principais, Modelos editáveis, bibliotecas do lado do cliente e desenvolvimento de componentes com o Adobe Experience Manager Sites.

## Visão geral {#wknd-tutorial-overview}

O objetivo deste tutorial em várias partes é ensinar um desenvolvedor a implementar um site usando os padrões e tecnologias mais recentes no Adobe Experience Manager (AEM). Após concluir este tutorial, um desenvolvedor deve entender a base básica da plataforma e com conhecimento de padrões de design comuns no AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

## Opções para iniciar um projeto do Sites

Há duas abordagens básicas para iniciar um projeto do AEM Sites.

**Arquétipo de projeto AEM** - Abordagem tradicional AEM desenvolvimento através da criação de um projeto AEM mínimo utilizando um modelo Maven. Essa é a abordagem recomendada para AEM projetos 6.5/6.4 e AEM projetos as a Cloud Service que antecipam uma personalização pesada. O tutorial oferece um mergulho mais profundo no desenvolvimento AEM.

[Inicie o tutorial com o Arquétipo de projeto AEM](./project-archetype/overview.md)

**Modelos de site AEM** - Também conhecida como Criação rápida de site, uma abordagem de código baixo para gerar um site AEM usando um modelo de site predefinido. Use componentes e modelos prontos para uso para ativar e executar rapidamente um site. Use um fluxo de trabalho temático para aplicar estilos e personalizações específicos da marca apenas com CSS e JavaScript. Recomendado para novos projetos e desenvolvedores. Disponível apenas para AEM as a Cloud Service.

[Iniciar o tutorial usando um modelo de site](./site-template/create-site.md)

## Kit da interface do usuário do Adobe XD

Para tornar este tutorial mais próximo de um cenário real, os designers UX talentosos criaram modelos para o site usando [Adobe XD](https://www.adobe.com/products/xd.html). Ao longo do tutorial, várias partes dos designs são implementadas em um site de AEM totalmente criável. Agradecimentos especiais a **Lorenzo Buosi** e **Kilian Emenda** que criou o belo design para o site WKND.

Baixe os kits da interface do usuário do XD:

* [Kit da interface do usuário do componente principal do AEM](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit da interface do usuário do WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

## Site de referência {#reference-site}

Uma versão concluída do Site WKND também está disponível como referência: [https://wknd.site/](https://wknd.site/)

O tutorial aborda as principais habilidades de desenvolvimento necessárias para um desenvolvedor de AEM, mas *not* criar o site inteiro de ponta a ponta. O site de referência finalizado é outro excelente recurso para explorar e ver mais recursos AEM prontos para uso.

Para testar o código mais recente antes de entrar no tutorial, baixe e instale o **[última versão do GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Alimentado pela Adobe Stock

Muitas das imagens no site de referência WKND são de [Adobe Stock](https://stock.adobe.com/) e são Materiais de terceiros conforme definido nos Termos Adicionais do Ativo de demonstração em [https://www.adobe.com/legal/terms.html](https://www.adobe.com/br/legal/terms.html). Se você quiser usar uma imagem do Adobe Stock para outros propósitos além da exibição deste site de demonstração, como exibi-la em um site ou em materiais de marketing, poderá comprar uma licença no Adobe Stock.

Com o Adobe Stock, você tem acesso a mais de 140 milhões de imagens de alta qualidade e isentas de royalties, incluindo fotos, gráficos, vídeos e modelos para começar seus projetos criativos.

## Próximas etapas {#next-steps}

O que você está esperando?! Saiba como [gerar um novo projeto do Adobe Experience Manager usando o Arquétipo de projeto AEM](./project-archetype/overview.md) ou [criar um site usando um modelo de site](./site-template/create-site.md).
