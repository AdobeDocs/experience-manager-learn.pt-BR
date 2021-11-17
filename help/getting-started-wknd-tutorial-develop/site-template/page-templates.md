---
title: Modelos de página
description: Saiba como criar e modificar modelos de página. Entenda a relação entre um modelo de página e uma página. Saiba como configurar as políticas de um modelo de página para fornecer governança granular e consistência da marca para o conteúdo.  Um modelo bem estruturado de artigo de revista será criado com base em um modelo do Adobe XD.
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
source-git-commit: 04096fe3c99cdcce2d43b2b29899c2bbe37ac056
workflow-type: tm+mt
source-wordcount: '691'
ht-degree: 5%

---

# Modelos de página {#page-templates}

>[!CAUTION]
>
> No momento, a ferramenta Criação rápida de site é uma visualização técnica. É disponibilizado para fins de ensaio e avaliação e não se destina à utilização da produção, a menos que acordado com o apoio ao Adobe.

Neste capítulo, exploraremos a relação entre um modelo de página e uma página. Criaremos um modelo de Artigo da Revista sem estilo baseado em alguns modelos de [AdobeXD](https://www.adobe.com/products/xd.html). No processo de criação do modelo, os Componentes principais e as configurações avançadas de política são abordados.

## Pré-requisitos {#prerequisites}

Este é um tutorial de várias partes e presume-se que as etapas descritas na seção [Criar conteúdo e publicar alterações](./author-content-publish.md) capítulo foi concluído.

## Objetivo

1. Entenda os detalhes dos Modelos de página e como as políticas podem ser usadas para impor o controle granular do conteúdo da página.
1. Saiba como Modelos e Páginas são vinculados.
1. Crie um novo modelo e crie uma página.

## O que você vai criar {#what-you-will-build}

Nesta parte do tutorial, você criará um novo modelo de Página de artigo da Revista que poderá ser usado para criar novos artigos de revistas e se alinha a uma estrutura comum. O modelo será baseado em designs e um Kit de interface do usuário produzido no AdobeXD. Este capítulo só é focado na construção da estrutura ou esqueleto do modelo. Nenhum estilo será implementado, mas o modelo e as páginas serão funcionais.

## Criar o modelo da página de artigo da revista

Ao criar uma página, você deve selecionar um modelo, que será usado como a base de criação da nova página. O modelo define a estrutura da página resultante, o conteúdo inicial e os componentes permitidos.

Há três áreas principais de [Modelos de página](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html?lang=pt-BR):

1. **Estrutura** - define os componentes que fazem parte do modelo. Eles não serão editáveis pelos autores de conteúdo.
1. **Conteúdo inicial** - define os componentes com os quais o modelo começará, eles podem ser editados e/ou excluídos pelos autores de conteúdo
1. **Políticas** - define configurações sobre como os componentes se comportarão e quais opções os autores terão disponíveis.

Em seguida, crie um novo modelo no AEM que corresponda à estrutura dos modelos. Isso ocorrerá em uma instância local de AEM. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

Você pode usar a seguinte miniatura para identificar o modelo (ou fazer upload do seu próprio!)

![Miniatura do modelo de página do artigo](./assets/page-templates/article-page-template-thumbnail.png)


### Pacote da solução

Uma conclusão [solução do modelo de revista](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip) pode ser baixado e instalado por meio do Gerenciador de pacotes.

## Atualizar o cabeçalho e o rodapé com fragmentos de experiência {#experience-fragments}

Uma prática comum ao criar conteúdo global, como um cabeçalho ou rodapé, é usar um [Fragmento de experiência](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Fragmentos de experiência, permite que os usuários combinem vários componentes para criar um único componente com capacidade de referência. Os Fragmentos de experiência têm a vantagem de oferecer suporte ao gerenciamento de vários sites e [localização](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

O modelo de Site gerou um Cabeçalho e um Rodapé. Em seguida, atualize os Fragmentos de experiência para corresponder aos modelos. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

Etapas de alto nível para o vídeo abaixo:

1. Baixe o pacote de conteúdo de exemplo **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**.
1. Faça upload e instale o pacote de conteúdo usando o Gerenciador de pacotes.
1. Atualize os Fragmentos de experiência do cabeçalho e do rodapé para usar o logotipo WKND

## Criar uma página de artigo da Revista

Em seguida, crie uma nova página usando o modelo Página de artigo da Revista . Crie o conteúdo da página para corresponder aos modelos do site. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

Use o [texto fornecido](./assets/page-templates/la-skateparks-copy.txt) para preencher o corpo do artigo.

## Parabéns! {#congratulations}

Parabéns, você acabou de criar um novo modelo e página com o Adobe Experience Manager Sites.

### Próximas etapas {#next-steps}

Neste ponto, a página de artigo da revista e o site não correspondem aos estilos de marca da WKND. Siga as [Teming](theming.md) tutorial para saber mais sobre as práticas recomendadas para atualizar o código de primeiro plano CSS e Javascript usado para aplicar estilos globais ao site.

### Pacote da solução

Um pacote de soluções para este capítulo está disponível para download: [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip).
