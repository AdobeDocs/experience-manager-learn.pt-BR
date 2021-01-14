---
title: Criação de fragmentos de conteúdo - Introdução ao AEM sem cabeçalho - GraphQL
description: Introdução ao Adobe Experience Manager (AEM) e ao GraphQL. Crie e edite um novo Fragmento de conteúdo com base em um Modelo de fragmento de conteúdo. Saiba como criar variações de Fragmentos de conteúdo.
sub-product: ativos
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6713
thumbnail: 22451.jpg
translation-type: tm+mt
source-git-commit: eb2b556c5947b15a31a74a86dadd525fb06bcf14
workflow-type: tm+mt
source-wordcount: '815'
ht-degree: 0%

---


# Criação de fragmento de conteúdo {#authoring-content-fragments}

>[!CAUTION]
>
> A API AEM GraphQL para o Delivery de fragmentos de conteúdo está disponível sob solicitação.
> Entre em contato com o Suporte ao Adobe para habilitar a API do seu AEM como programa Cloud Service.

Neste capítulo, você criará e editará um novo Fragmento de conteúdo com base no [modelo de fragmento de conteúdo do contribuidor recém-definido](./content-fragment-models.md). Você também aprenderá a criar variações de Fragmentos de conteúdo.

## Pré-requisitos {#prerequisites}

Este é um tutorial de várias partes e presume-se que as etapas descritas em [Definindo Modelos de Fragmento de Conteúdo](./content-fragment-models.md) foram concluídas.

## Objetivos {#objectives}

* Criar um fragmento de conteúdo com base em um modelo de fragmento de conteúdo
* Criar uma variação de fragmento de conteúdo

## Visão geral de criação de fragmentos de conteúdo {#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

O vídeo acima fornece uma visão geral de alto nível da criação de Fragmentos de conteúdo.

## Criar um fragmento de conteúdo {#create-content-fragment}

No capítulo anterior, [Definindo modelos de fragmento de conteúdo](./content-fragment-models.md), um modelo **Colaborador** foi criado. Crie um novo fragmento de conteúdo usando este modelo.

1. No menu **AEM Start**, navegue até **Assets** > **Files**.
1. Clique nas pastas para navegar até **Site WKND** > **Inglês** > **Colaboradores**. Esta pasta contém uma lista de fotos principais para os contribuidores da marca WKND.

1. Clique em **Criar** no canto superior direito e selecione **Fragmento de conteúdo**:

   ![Clique em Criar um novo fragmento](assets/author-content-fragments/create-content-fragment-menu.png)

1. Selecione o modelo **Contributor** e clique em **Próximo**.

   ![Selecionar modelo de contribuidor](assets/author-content-fragments/select-contributor-model.png)

   Este é o mesmo modelo **Contributor** que foi criado no capítulo anterior.

1. Digite **Stacey Roswells** como título e clique em **Criar**.
1. Clique em **Abrir** na caixa de diálogo **Êxito** para abrir o fragmento recém-criado.

   ![Novo fragmento de conteúdo criado](assets/author-content-fragments/new-content-fragment.png)

   Observe que os campos definidos pelo modelo agora estão disponíveis para criar essa instância do Fragmento de conteúdo.

1. Para **Nome Completo** introduza: **Stacey Roswells**.
1. Para **Biografia** inserir uma breve biografia. Precisa de alguma inspiração? Sinta-se à vontade para reutilizar este [ficheiro de texto](assets/author-content-fragments/stacey-roswells-bio.txt).
1. Para **Referência de imagem** clique no ícone **pasta** e navegue até **Site WKND** > **Inglês** > **Contribuidores** > **stacey-roswells.jpg**. Isso avaliará o caminho: `/content/dam/wknd/en/contributors/stacey-roswells.jpg`.
1. Para **Ocupação** escolha **Fotógrafo**.

   ![Fragmento criado](assets/author-content-fragments/stacye-roswell-fragment-authored.png)

1. Clique em **Salvar** para salvar as alterações.

## Criar uma variação de fragmento do conteúdo

Todos os fragmentos de conteúdo são start com uma variação **Principal**. A variação **Principal** pode ser considerada o conteúdo *padrão* do fragmento e é usada automaticamente quando o conteúdo é exposto pelas APIs GraphQL. Também é possível criar variações de um Fragmento de conteúdo. Este recurso oferta flexibilidade adicional para projetar uma implementação.

As variações podem ser usadas para canais específicos do público alvo. Por exemplo, uma variação **mobile** pode ser criada que contenha uma quantidade menor de texto ou faça referência a uma imagem específica do canal. A forma como as variações são utilizadas está realmente à altura da implementação. Como qualquer recurso, um planejamento cuidadoso deve ser feito antes de usar.

Em seguida, crie uma nova variação para ter uma ideia dos recursos disponíveis.

1. Abra novamente o **Fragmento de conteúdo do Stacey Roswells**.
1. No painel esquerdo, clique em **Criar variação**.
1. No modal **Nova Variação** digite um Título de **Resumo**.

   ![Nova variação - Resumo](assets/author-content-fragments/new-variation-summary.png)

1. Clique no campo de **Biografia** várias linhas e clique no botão **Expandir** para inserir a visualização em tela cheia para o campo de várias linhas.

   ![Digite a visualização de tela inteira](assets/author-content-fragments/enter-full-screen-view.png)

1. Clique em **Resumir texto** no menu superior direito.

1. Digite um Start **de** de **50** palavras e clique em **Público alvo**.

   ![Pré-visualização de resumo](assets/author-content-fragments/summarize-text-preview.png)

   Isso abrirá uma pré-visualização de resumo. AEM processador de idioma da máquina tentará resumir o texto com base na contagem de palavras do público alvo. Você também pode selecionar frases diferentes para remover.

1. Clique em **Resumir** quando estiver satisfeito com o resumo. Clique no campo de texto de várias linhas e alterne o botão **Expandir** para retornar à visualização principal.

1. Clique em **Salvar** para salvar as alterações.

## Criar um fragmento de conteúdo adicional

Repita as etapas descritas em [Criar um fragmento de conteúdo](#create-content-fragment) para criar um **Contribuidor** adicional. Isso será usado no próximo capítulo como um exemplo de como query vários fragmentos.

1. Na pasta **Contributors**, clique em **Criar** no canto superior direito e selecione **Fragmento de conteúdo**:
1. Selecione o modelo **Contributor** e clique em **Próximo**.
1. Digite **Jacob Wester** como título e clique em **Criar**.
1. Clique em **Abrir** na caixa de diálogo **Êxito** para abrir o fragmento recém-criado.
1. Para **Nome Completo** introduza: **Jacob Wester**.
1. Para **Biografia** inserir uma breve biografia. Precisa de alguma inspiração? Sinta-se à vontade para reutilizar este [ficheiro de texto](assets/author-content-fragments/jacob-wester.txt).
1. Para **Referência de imagem** clique no ícone **pasta** e navegue até **Site WKND** > **Inglês** > **Contribuidores** > **jacob_wester.jpg**. Isso avaliará o caminho: `/content/dam/wknd/en/contributors/jacob_wester.jpg`.
1. Para **Ocupação** escolha **Escritor**.
1. Clique em **Salvar** para salvar as alterações. Não há necessidade de criar uma variação, a menos que você deseje!

   ![Fragmento de conteúdo adicional](assets/author-content-fragments/additional-content-fragment.png)

   Agora você deve ter dois fragmentos **Contribuidores**.

## Parabéns! {#congratulations}

Parabéns, você acabou de criar vários Fragmentos de conteúdo e criar uma variação.

## Próximas etapas {#next-steps}

No próximo capítulo, [Explorar APIs do GraphQL](explore-graphql-api.md), você explorará AEM APIs do GraphQL usando a ferramenta integrada do GrapiQL. Saiba como AEM automaticamente gera um schema GraphQL com base em um modelo de Fragmento de conteúdo. Você tentará construir query básicos usando a sintaxe GraphQL.