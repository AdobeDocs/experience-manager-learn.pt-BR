---
title: Definição de modelos de fragmento de conteúdo - Introdução ao AEM sem cabeçalho - GraphQL
description: Introdução ao Adobe Experience Manager (AEM) e ao GraphQL. Saiba como modelar conteúdo e criar um schema com modelos de fragmento de conteúdo em AEM. Analise os modelos existentes e crie um novo modelo. Saiba mais sobre os diferentes tipos de dados que podem ser usados para definir um schema.
sub-product: ativos
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: null
thumbnail: null
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '971'
ht-degree: 1%

---


# Definindo Modelos de Fragmento de Conteúdo {#content-fragment-models}

>[!CAUTION]
>
> A API AEM GraphQL para o Delivery de fragmento de conteúdo será lançada no início de 2021.
> A documentação relacionada está disponível para fins de pré-visualização.

Neste capítulo, saiba como modelar o conteúdo e criar um schema com **Modelos de fragmento de conteúdo**. Você revisará os modelos existentes e criará um novo modelo. Você também aprenderá sobre os diferentes tipos de dados que podem ser usados para definir um schema como parte do modelo.

Neste capítulo, você criará um novo modelo para um **Contributor**, que é o modelo de dados para os usuários que criam conteúdo de revista e aventura como parte da marca WKND.

## Pré-requisitos {#prerequisites}

Este é um tutorial de várias partes e presume-se que as etapas descritas em [Configuração rápida](./setup.md) foram concluídas.

## Objetivos {#objectives}

* Criar um novo modelo de fragmento de conteúdo.
* Identifique os tipos de dados disponíveis e as opções de validação para a criação de modelos.
* Entenda como o Modelo de fragmento de conteúdo define **o schema de dados e o modelo de criação de um Fragmento de conteúdo.**

## Visão geral do modelo de fragmento de conteúdo {#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

O vídeo acima fornece uma visão geral de alto nível sobre como trabalhar com modelos de fragmento de conteúdo.

## Inspect o modelo de fragmento de conteúdo da Adobe

No capítulo anterior, vários Fragmentos de conteúdo de Aventuras foram editados e exibidos em um aplicativo externo. Vamos inspecionar o Modelo de fragmento de conteúdo da Adobe para entender o schema de dados subjacente desses fragmentos.

1. No menu **AEM Start**, navegue até **Ferramentas** > **Ativos** > **Modelos de fragmento de conteúdo**.

   ![Navegar até Modelos de fragmento de conteúdo](assets/content-fragment-models/content-fragment-model-navigation.png)

1. Navegue até a pasta **Site WKND** e passe o mouse sobre o **Modelo de fragmento de conteúdo** e clique no ícone **Editar** (lápis) para abrir o modelo.

   ![Abra o modelo de fragmento de conteúdo da Adobe](assets/content-fragment-models/adventure-content-fragment-edit.png)

1. Isso abre o **Editor do modelo de fragmento de conteúdo**. Observe que os campos definem o modelo de Aventura incluem **Tipos de Dados** diferentes, como **Texto de linha única**, **Texto de várias linhas**, **Lista discriminada** e **Referência de Conteúdo**.

1. A coluna à direita do editor lista os **Tipos de dados** disponíveis que definem os campos de formulário usados para criar Fragmentos de conteúdo.

1. Selecione o campo **Título** no painel principal. Na coluna da direita, clique na guia **Propriedades**:

   ![Propriedades do Título da Aventura](assets/content-fragment-models/adventure-title-properties-tab.png)

   Observe que o campo **Nome da propriedade** está definido como `adventureTitle`. Isso define o nome da propriedade que é persistente para AEM. O **Nome da propriedade** também define o nome **key** para esta propriedade como parte do schema de dados. Essa **chave** será usada quando os dados do Fragmento de conteúdo forem expostos por meio de APIs GraphQL.

   >[!CAUTION]
   >
   > Modificar o **Nome da propriedade** de um campo **depois de** Fragmentos de conteúdo são derivados do Modelo, tem efeitos de downstream. Os valores de campo em fragmentos existentes não serão mais referenciados e o schema de dados exposto pelo GraphQL será alterado, afetando os aplicativos existentes.

1. Role para baixo na guia **Propriedades** e visualização a lista suspensa **Tipo de validação**.

   ![Opções de validação disponíveis](assets/content-fragment-models/validation-options-available.png)

   As validações de formulário prontas estão disponíveis para **E-mail** e **URL**. Também é possível definir uma validação **Custom** usando uma expressão regular.

1. Clique em **Cancelar** para fechar o Editor do Modelo de Fragmento de Conteúdo.

## Criar um modelo de contribuidor

Em seguida, crie um novo modelo para um **Contributor**, que é o modelo de dados para os usuários que criam conteúdo de revista e aventura como parte da marca WKND.

1. Clique em **Criar** no canto superior direito para exibir o assistente **Criar modelo**.
1. Para **Título do Modelo** insira: **Colaborador** e clique em **Criar**

   ![Assistente de modelo de fragmento de conteúdo](assets/content-fragment-models/content-fragment-model-wizard.png)

   Clique em **Abrir** para abrir o modelo recém-criado.

1. Arraste e solte um elemento **Texto de linha única** no painel principal. Digite as seguintes propriedades na guia **Propriedades**:

   * **Rótulo** do campo:  **Nome completo**
   * **Nome da Propriedade**: `fullName`
   * Verificar **Obrigatório**

   ![Campo de propriedade Nome completo](assets/content-fragment-models/full-name-property-field.png)

1. Clique na guia **Tipos de dados** e arraste e solte um campo **Texto multilinha** abaixo do campo **Nome completo**. Digite as seguintes propriedades:

   * **Rótulo** do campo:  **Biografia**
   * **Nome da Propriedade**: `biographyText`
   * **Tipo** padrão:  **Rich Text**

1. Clique na guia **Tipos de dados** e arraste e solte um campo **Referência de conteúdo**. Digite as seguintes propriedades:

   * **Rótulo** do campo:  **Referência de imagem**
   * **Nome da Propriedade**: `pictureReference`
   * **Caminho raiz**: `/content/dam/wknd`

   Ao configurar o **Caminho raiz**, você pode clicar no ícone **pasta** para abrir um modal para selecionar o caminho. Isso restringirá quais pastas os autores podem usar para preencher o caminho.

   ![Caminho raiz configurado](assets/content-fragment-models/root-path-configure.png)

1. Adicione uma validação à **Referência de imagem** para que somente os tipos de conteúdo de **Imagens** possam ser usados para preencher o campo.

   ![Restringir a imagens](assets/content-fragment-models/picture-reference-content-types.png)

1. Clique na guia **Tipos de dados** e arraste e solte um tipo de dados **Lista discriminada** abaixo do campo **Referência de imagem**. Digite as seguintes propriedades:

   * **Rótulo** do campo:  **Ocupação**
   * **Nome da Propriedade**: `occupation`

1. Adicione várias **Opções** usando o botão **Adicionar uma opção**. Use o mesmo valor para **Etiqueta de Opção** e **Valor de Opção**:

   **Artista**,  **Influenciador**,  **Fotógrafo**,  **Viajante**,  **Escritor**,  **YouTuber**

   ![Valores das opções de ocupação](assets/content-fragment-models/occupation-options-values.png)

1. O modelo final **Contributor** deve ser parecido com o seguinte:

   ![Modelo de contribuidor final](assets/content-fragment-models/final-contributor-model.png)

1. Clique em **Salvar** para salvar as alterações.

## Ativar o modelo do contribuidor

Os Modelos de fragmento de conteúdo assumem como padrão um estado **Draft** quando criados pela primeira vez. Isso permite que os usuários refinem o Modelo de fragmento de conteúdo **antes de** permitindo que os autores o usem. Lembre-se de que modificar o **Nome da propriedade** de um campo no modelo altera o schema de dados subjacente e pode ter efeitos descendentes significativos nos fragmentos existentes e aplicativos externos. É recomendável planejar cuidadosamente a convenção de nomenclatura usada para **Nome da propriedade** dos campos.

1. Observe que o modelo **Contributor** está atualmente em um estado **Rascunho**.

1. Ative o **Modelo do contribuidor** passando o mouse sobre o cartão e clicando no ícone **Ativar**:

   ![Ativar o modelo do contribuidor](assets/content-fragment-models/enable-contributor-model.png)

## Parabéns! {#congratulations}

Parabéns, você acabou de criar seu primeiro Modelo de fragmento de conteúdo!

## Próximas etapas {#next-steps}

No próximo capítulo, [Criação de modelos de fragmento de conteúdo](author-content-fragments.md), você criará e editará um novo fragmento de conteúdo com base em um modelo de fragmento de conteúdo. Você também aprenderá a criar variações de Fragmentos de conteúdo.