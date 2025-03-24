---
title: Modelos de página
description: Saiba como criar e modificar modelos de página. Entenda a relação entre um Modelo de página e uma Página. Saiba como configurar as políticas de um Modelo de página para fornecer governança granular e consistência de marca para o conteúdo.  Um modelo de Artigo de revista bem estruturado é criado com base em um modelo do Adobe XD.
version: Experience Manager as a Cloud Service
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
recommendations: noDisplay, noCatalog
duration: 1561
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '628'
ht-degree: 0%

---

# Modelos de página {#page-templates}

Neste capítulo, exploraremos a relação entre um Modelo de página e uma Página. Criaremos um modelo não estilizado de Artigo de revista com base em alguns modelos de [AdobeXD](https://www.adobe.com/products/xd.html). No processo de criação do modelo, os Componentes principais e as configurações de política avançadas são abordados.

## Pré-requisitos {#prerequisites}

Este é um tutorial em várias partes e presume-se que as etapas descritas no capítulo [Criar conteúdo e publicar alterações](./author-content-publish.md) foram concluídas.

## Objetivo

1. Entenda os detalhes dos Modelos de página e como as políticas podem ser usadas para impor o controle granular do conteúdo da página.
1. Saiba como Modelos e Páginas são vinculados.
1. Crie um novo modelo e crie uma página.

## O que você vai criar {#what-you-will-build}

Nesta parte do tutorial, você criará um novo modelo de Página de artigo de revista que pode ser usado para criar novos artigos de revista e se alinha a uma estrutura comum. O modelo é baseado em designs e em um kit de interface do usuário produzido no AdobeXD. Este capítulo foca apenas na criação da estrutura ou esqueleto do template. Nenhum estilo é implementado, mas o modelo e as páginas são funcionais.

## Criar o modelo da página de artigo da revista

Ao criar uma página, você deve selecionar um modelo, que é usado como base para a criação da nova página. O modelo define a estrutura da página resultante, o conteúdo inicial e os componentes permitidos.

Existem 3 áreas principais de [Modelos de página](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=pt-BR):

1. **Estrutura** - define os componentes que fazem parte do modelo. Eles não podem ser editados por autores de conteúdo.
1. **Conteúdo inicial** - define os componentes com os quais o modelo começa; eles podem ser editados e/ou excluídos por autores de conteúdo
1. **Políticas** - define as configurações sobre como os componentes se comportam e quais opções os autores terão disponíveis.

Em seguida, crie um novo modelo no AEM que corresponda à estrutura dos modelos. Isso ocorrerá em uma instância local do AEM. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/332915?quality=12&learn=on)

Você pode usar a seguinte miniatura para identificar seu modelo (ou fazer upload do seu próprio modelo!)

![Miniatura do modelo de página de artigo](./assets/page-templates/article-page-template-thumbnail.png)


### Pacote de soluções

Uma solução [concluída do Modelo de Revista](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip) pode ser baixada e instalada via Gerenciador de Pacotes.

## Atualizar o cabeçalho e o rodapé com fragmentos de experiência {#experience-fragments}

Uma prática comum ao criar conteúdo global, como cabeçalho ou rodapé, é usar um [Fragmento de experiência](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Fragmentos de experiência, permitem que os usuários combinem vários componentes para criar um único componente que pode ser referenciado. Os Fragmentos de experiência têm a vantagem de oferecer suporte ao gerenciamento de vários sites e à [localização](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

O modelo de site gerou um Cabeçalho e Rodapé. Em seguida, atualize os Fragmentos de experiência para corresponder aos modelos. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/332916?quality=12&learn=on)

Etapas de alto nível para o vídeo abaixo:

1. Baixe o pacote de conteúdo de exemplo **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**.
1. Faça upload e instale o pacote de conteúdo usando o Gerenciador de pacotes.
1. Atualizar os Fragmentos de experiência de cabeçalho e rodapé para usar o logotipo da WKND

## Criar uma página de artigo da revista

Em seguida, crie uma nova página usando o modelo Página de artigo da revista. Crie o conteúdo da página para corresponder aos modelos de site. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/332917?quality=12&learn=on)

Use o [texto fornecido](./assets/page-templates/la-skateparks-copy.txt) para preencher o corpo do artigo.

## Parabéns. {#congratulations}

Parabéns, você acabou de criar um novo modelo e página com o Adobe Experience Manager Sites.

### Próximas etapas {#next-steps}

Nesse momento, a página de artigo da revista e o site não correspondem aos estilos de marca da WKND. Siga o tutorial [Theming](theming.md) para saber as práticas recomendadas para atualizar o código de front-end CSS e Javascript usado para aplicar estilos globais ao site.

### Pacote de soluções

Um pacote de soluções para este capítulo está disponível para download: [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
