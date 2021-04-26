---
title: Modelos de página
seo-title: Introdução ao AEM Sites - Modelos de página
description: Saiba como criar e modificar modelos de página. Entenda a relação entre um modelo de página e uma página. Saiba como configurar as políticas de um modelo de página para fornecer governança granular e consistência da marca para o conteúdo.  Um modelo bem estruturado de artigo de revista será criado com base em um modelo do Adobe XD.
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Gerenciamento de conteúdo
feature: Componentes principais, Modelos editáveis, Editor de páginas
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 3%

---


# Modelos de página {#page-templates}

>[!CAUTION]
>
> Os recursos rápidos de criação de sites mostrados aqui serão lançados na segunda metade de 2021. A documentação relacionada está disponível para fins de visualização.

Neste capítulo, exploraremos a relação entre um modelo de página e uma página. Criaremos um modelo de Artigo de Revista sem estilo com base em alguns modelos do [AdobeXD](https://www.adobe.com/products/xd.html). No processo de criação do modelo, os Componentes principais e as configurações avançadas de política são abordados.

## Pré-requisitos {#prerequisites}

Este é um tutorial de várias partes e presume-se que as etapas descritas no capítulo [Autor content and publish changes](./author-content-publish.md) foram concluídas.

## Objetivo

1. Inspect é um design de página criado no Adobe XD e o mapeia para Componentes principais.
1. Entenda os detalhes dos Modelos de página e como as políticas podem ser usadas para impor o controle granular do conteúdo da página.
1. Saiba como Modelos e páginas são vinculados.

## O que você vai criar {#what-you-will-build}

Nesta parte do tutorial, você criará um novo modelo de Página de artigo da Revista que poderá ser usado para criar novos artigos de revistas e se alinha a uma estrutura comum. O modelo será baseado em designs e um Kit de interface do usuário produzido no AdobeXD. Este capítulo só é focado na construção da estrutura ou esqueleto do modelo. Nenhum estilo será implementado, mas o modelo e as páginas serão funcionais.

## Planejamento de interface do usuário com Adobe XD {#adobexd}

Na maioria dos casos, o planejamento de um novo site começa com modelos e designs estáticos. [Adobe ](https://www.adobe.com/products/xd.html) XDé uma ferramenta de design que cria experiências do usuário. Em seguida, inspecionaremos um Kit de interface do usuário e modelos para ajudar a planejar a estrutura do Modelo de página de artigo.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**Baixe o arquivo de design de artigo  [WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> Um [Kit de interface dos componentes principais genérico também está disponível](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) como ponto de partida para projetos personalizados.

## Criar o modelo da página de artigo da revista

Ao criar uma página, você deve selecionar um modelo, que será usado como a base de criação da nova página. O modelo define a estrutura da página resultante, o conteúdo inicial e os componentes permitidos.

Há 3 áreas principais de [Modelos de página](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html):

1. **Estrutura**  - define componentes que fazem parte do modelo. Eles não serão editáveis pelos autores de conteúdo.
1. **Conteúdo inicial**  - define os componentes com os quais o modelo começará, eles podem ser editados e/ou excluídos pelos autores de conteúdo
1. **Políticas**  - define configurações sobre como os componentes se comportarão e quais opções os autores terão disponíveis.

Em seguida, crie um novo modelo no AEM que corresponda à estrutura dos modelos. Isso ocorrerá em uma instância local de AEM. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

### Pacote da solução

Uma solução [concluída do Modelo de Revista](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip) pode ser baixada e instalada por meio do Gerenciador de Pacotes.

## Atualizar o cabeçalho e o rodapé com fragmentos de experiência {#experience-fragments}

Uma prática comum ao criar conteúdo global, como um cabeçalho ou rodapé, é usar um [Fragmento de experiência](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Fragmentos de experiência, permite que os usuários combinem vários componentes para criar um único componente com capacidade de referência. Os Fragmentos de experiência têm a vantagem de suportar o gerenciamento de vários sites e [localização](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

O modelo de Site gerou um Cabeçalho e um Rodapé. Em seguida, atualize os Fragmentos de experiência para corresponder aos modelos. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

Etapas de alto nível para o vídeo abaixo:

1. Baixe o pacote de conteúdo de amostra **[WKND-Starter-Assets-Skate-Article-1.0.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.0.zip)**.
1. Faça upload e instale o pacote de conteúdo usando o Gerenciador de pacotes.
1. Atualize os Fragmentos de experiência do cabeçalho e do rodapé para usar o logotipo WKND

## Criar uma página de artigo da Revista

Em seguida, crie uma nova página usando o modelo Página de artigo da Revista . Crie o conteúdo da página para corresponder aos modelos do site. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

## Parabéns! {#congratulations}

Parabéns, você acabou de criar um novo modelo e página com o Adobe Experience Manager Sites.

### Próximas etapas {#next-steps}

Neste ponto, a página de artigo da revista e o site não correspondem aos estilos de marca da WKND. Siga o tutorial [Theming](theming.md) para saber mais sobre as práticas recomendadas para atualizar o código de primeiro plano CSS e Javascript usado para aplicar estilos globais ao site.

### Pacote da solução

Um pacote de soluções para este capítulo está disponível para download: [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
