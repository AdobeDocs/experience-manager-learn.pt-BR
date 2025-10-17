---
title: Modelos de página
description: Saiba como criar e modificar modelos de página. Entenda a relação entre um modelo de página e uma página. Saiba como configurar as políticas de um modelo de página para permitir uma governança granular e consistência da marca no conteúdo.  Um modelo de artigo de revista bem estruturado será criado com base em uma simulação do Adobe XD.
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
workflow-type: ht
source-wordcount: '628'
ht-degree: 100%

---

# Modelos de página {#page-templates}

Neste capítulo, vamos abordar a relação entre um modelo de página e uma página. Vamos criar um modelo não estilizado de artigo de revista com base em algumas simulações do [AdobeXD](https://www.adobe.com/products/xd.html). No processo de criação do modelo, os componentes principais e as configurações de política avançadas serão abordados.

## Pré-requisitos {#prerequisites}

Este é um tutorial em várias partes, e presume-se que as etapas descritas no capítulo [Criar conteúdo e publicar alterações](./author-content-publish.md) tenham sido concluídas.

## Objetivo

1. Entender os detalhes dos modelos de página e como as políticas podem ser usadas para impor um controle granular do conteúdo da página.
1. Aprender sobre como modelos e páginas são vinculados.
1. Criar um novo modelo e uma página.

## O que você criará {#what-you-will-build}

Nesta parte do tutorial, você criará um novo modelo de página de artigo de revista que pode ser usado para criar novos artigos de revista e está alinhado a uma estrutura comum. O modelo é baseado em designs e em um kit da IU produzido no AdobeXD. Este capítulo foca-se apenas na criação da estrutura ou esqueleto do modelo. Nenhum estilo é implementado, mas o modelo e as páginas funcionam.

## Criar o modelo de página de artigo de revista

Ao criar uma página, é necessário selecionar um modelo, que é usado como base para criação da nova página. O modelo define a estrutura da página resultante, o conteúdo inicial e os componentes permitidos.

Existem três áreas principais nos [Modelos de página](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=pt-BR):

1. **Estrutura**: define os componentes que fazem parte do modelo. Não podem ser editados por criadores de conteúdo.
1. **Conteúdo inicial**: define os componentes com os quais o modelo começa; podem ser editados e/ou excluídos por criadores de conteúdo
1. **Políticas**: define as configurações de como os componentes se comportam e quais opções estarão disponíveis para os criadores.

Em seguida, crie um novo modelo no AEM que corresponda à estrutura das simulações. Isso ocorrerá em uma instância local do AEM. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/332915?quality=12&learn=on)

Você pode usar a seguinte miniatura para identificar o seu modelo (ou carregar o seu próprio modelo).

![Miniatura do modelo de página de artigo](./assets/page-templates/article-page-template-thumbnail.png)


### Pacote de solução

Uma solução completa de [modelo de revista](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip) pode ser baixada e instalada por meio do gerenciador de pacotes.

## Atualizar o cabeçalho e o rodapé com fragmentos de experiência {#experience-fragments}

Uma prática comum ao criar um conteúdo global, como cabeçalho ou rodapé, é usar um [Fragmento de experiência](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Os fragmentos de experiência permitem que os usuários combinem vários componentes para criar um componente unificado que pode ser referenciado. Os fragmentos de experiência têm a vantagem de permitir o gerenciamento de vários sites e a [localização](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

O modelo de site gerou um cabeçalho e um rodapé. Em seguida, atualize os fragmentos de experiência para corresponderem às simulações. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/332916?quality=12&learn=on)

Etapas de alto nível do vídeo abaixo:

1. Baixe o pacote de conteúdo de exemplo **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**.
1. Carregue e instale o pacote de conteúdo com o gerenciador de pacotes.
1. Atualizar os fragmentos de experiência de cabeçalho e rodapé para usar o logotipo da WKND

## Criar uma página de artigo de revista

Em seguida, crie uma nova página com o modelo de página de artigo de revista. Crie o conteúdo da página para corresponder às simulações do site. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/332917?quality=12&learn=on)

Use o [texto fornecido](./assets/page-templates/la-skateparks-copy.txt) para preencher o corpo do artigo.

## Parabéns! {#congratulations}

Parabéns, você acabou de criar um novo modelo e uma nova página com o Adobe Experience Manager Sites.

### Próximas etapas {#next-steps}

Nesse momento, a página de artigo de revista e o site não correspondem aos estilos da marca da WKND. Siga o tutorial de [Temas](theming.md) para saber as práticas recomendadas para atualizar o código de front-end do CSS e do Javascript usados para aplicar estilos globais ao site.

### Pacote de solução

Um pacote de soluções para este capítulo está disponível para download: [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
