---
title: Definição dos modelos de fragmento de conteúdo - Introdução ao AEM Headless - GraphQL
description: Introdução ao Adobe Experience Manager (AEM) e GraphQL. Saiba como modelar o conteúdo e criar um esquema com Modelos de fragmento de conteúdo no AEM. Revise os modelos existentes e crie um novo modelo. Saiba mais sobre os diferentes tipos de dados que podem ser usados para definir um schema.
sub-product: ativos
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6712
thumbnail: 22452.jpg
feature: '"Fragmentos de conteúdo, APIs GraphQL"'
topic: '"Sem Cabeça, Gerenciamento De Conteúdo"'
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '975'
ht-degree: 1%

---


# Definição dos modelos de fragmento de conteúdo {#content-fragment-models}

Neste capítulo, saiba como modelar o conteúdo e criar um schema com **Modelos de fragmento de conteúdo**. Você verificará os modelos existentes e criará um novo modelo. Você também aprenderá sobre os diferentes tipos de dados que podem ser usados para definir um schema como parte do modelo.

Neste capítulo, você criará um novo modelo para um **Contributor**, que é o modelo de dados para os usuários que criam conteúdo de revista e aventura como parte da marca WKND.

## Pré-requisitos {#prerequisites}

Este é um tutorial de várias partes e presume-se que as etapas descritas em [Configuração rápida](./setup.md) foram concluídas.

## Objetivos {#objectives}

* Crie um novo Modelo de fragmento de conteúdo.
* Identifique os tipos de dados disponíveis e as opções de validação para criar modelos.
* Entenda como o Modelo do fragmento de conteúdo define **o esquema de dados e o modelo de criação para um Fragmento de conteúdo.**

## Visão geral do modelo de fragmento de conteúdo {#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

O vídeo acima fornece uma visão geral de alto nível do trabalho com Modelos de fragmento de conteúdo.

## Inspecionar o modelo de fragmento de conteúdo de empreendimento

No capítulo anterior, vários Fragmentos de conteúdo de empresas foram editados e exibidos em um aplicativo externo. Vamos inspecionar o Modelo de fragmento de conteúdo do Adventure para entender o esquema de dados subjacente desses fragmentos.

1. No menu **AEM Start**, navegue até **Ferramentas** > **Ativos** > **Modelos de fragmento de conteúdo**.

   ![Navegar até Modelos de fragmentos do conteúdo](assets/content-fragment-models/content-fragment-model-navigation.png)

1. Navegue até a pasta **Site WKND** e passe o mouse sobre o **Modelo de fragmento de conteúdo do Adventure** e clique no ícone **Editar** (lápis) para abrir o modelo.

   ![Abra o modelo de fragmento de conteúdo de empreendimento](assets/content-fragment-models/adventure-content-fragment-edit.png)

1. Isso abre o **Editor do modelo de fragmento de conteúdo**. Observe que os campos que definem o modelo de Aventura incluem **Tipos de Dados** diferentes como **Texto de linha única**, **Texto de várias linhas**, **Enumeração** e **Referência de conteúdo**.

1. A coluna à direita do editor lista os **Tipos de dados** disponíveis que definem os campos de formulário usados para criar Fragmentos de conteúdo.

1. Selecione o campo **Title** no painel principal. Na coluna à direita, clique na guia **Properties**:

   ![Propriedades do Título da Aventura](assets/content-fragment-models/adventure-title-properties-tab.png)

   Observe que o campo **Nome da propriedade** está definido como `adventureTitle`. Isso define o nome da propriedade que é persistente no AEM. O **Nome da propriedade** também define o nome **key** para essa propriedade como parte do schema de dados. Essa **chave** será usada quando os dados do Fragmento de conteúdo forem expostos por meio de APIs GraphQL.

   >[!CAUTION]
   >
   > Modificar o **Nome da propriedade** de um campo **depois de** Fragmentos de conteúdo são derivados do Modelo, tem efeitos downstream. Os valores de campo em fragmentos existentes não serão mais referenciados e o esquema de dados exposto pelo GraphQL será alterado, afetando os aplicativos existentes.

1. Role para baixo na guia **Properties** e exiba a lista suspensa **Validation Type**.

   ![Opções de validação disponíveis](assets/content-fragment-models/validation-options-available.png)

   As validações de formulário prontas para uso estão disponíveis para **E-mail** e **URL**. Também é possível definir uma validação **Custom** usando uma expressão regular.

1. Clique em **Cancelar** para fechar o Editor do Modelo de Fragmento de Conteúdo.

## Criar um modelo de colaborador

Em seguida, crie um novo modelo para um **Contributor**, que é o modelo de dados para os usuários que criam revista e conteúdo de aventura como parte da marca WKND.

1. Clique em **Criar** no canto superior direito para exibir o assistente **Criar modelo**.
1. Para **Título do Modelo** digite: **Contributor** e clique em **Criar**

   ![Assistente do modelo de fragmento de conteúdo](assets/content-fragment-models/content-fragment-model-wizard.png)

   Clique em **Abrir** para abrir o modelo recém-criado.

1. Arraste e solte um elemento **Single line text** no painel principal. Insira as seguintes propriedades na guia **Properties**:

   * **Rótulo** do campo:  **Nome completo**
   * **Nome da Propriedade**: `fullName`
   * Marque **Obrigatório**

   ![Campo de propriedade Nome completo](assets/content-fragment-models/full-name-property-field.png)

1. Clique na guia **Data Types** e arraste e solte um campo **Multi line text** abaixo do campo **Full Name**. Insira as seguintes propriedades:

   * **Rótulo** do campo:  **Biografia**
   * **Nome da Propriedade**: `biographyText`
   * **Tipo** padrão:  **Texto formatado**

1. Clique na guia **Tipos de dados** e arraste e solte um campo **Referência de conteúdo**. Insira as seguintes propriedades:

   * **Rótulo** do campo:  **Referência de imagem**
   * **Nome da Propriedade**: `pictureReference`
   * **Caminho raiz**: `/content/dam/wknd`

   Ao configurar o **Caminho Raiz**, você pode clicar no ícone **folder** para trazer um modal para selecionar o caminho. Isso restringirá quais pastas os autores podem usar para preencher o caminho.

   ![Caminho raiz configurado](assets/content-fragment-models/root-path-configure.png)

1. Adicione uma validação ao **Referência de imagem** para que somente os tipos de conteúdo de **Imagens** possam ser usados para preencher o campo.

   ![Restringir a imagens](assets/content-fragment-models/picture-reference-content-types.png)

1. Clique na guia **Tipos de dados** e arraste e solte um tipo de dados **Enumeration** abaixo do campo **Referência de imagem**. Insira as seguintes propriedades:

   * **Rótulo** do campo:  **Ocupação**
   * **Nome da Propriedade**: `occupation`

1. Adicione várias **Opções** usando o botão **Adicionar uma opção**. Use o mesmo valor para **Etiqueta de Opção** e **Valor de Opção**:

   **Artista**,  **Influenciador**,  **Fotógrafo**,  **Viajante**,  **Escritor**,  **Tuber**

   ![Valores das opções de ocupação](assets/content-fragment-models/occupation-options-values.png)

1. O modelo final **Contributor** deve ser semelhante ao seguinte:

   ![Modelo de contribuidor final](assets/content-fragment-models/final-contributor-model.png)

1. Clique em **Save** para salvar as alterações.

## Ativar o modelo de colaborador

Os Modelos de fragmento de conteúdo precisam ser **Ativado** antes que os autores de conteúdo possam usá-los. É possível **Desativar** um Modelo de Fragmento de Conteúdo, proibindo assim os autores de usá-lo. Lembre-se de que modificar o **Nome da propriedade** de um campo no modelo altera o schema de dados subjacente e pode ter efeitos downstream significativos em fragmentos existentes e aplicativos externos. É recomendável planejar cuidadosamente a convenção de nomenclatura usada para o **Nome da propriedade** dos campos antes de habilitar o Modelo do fragmento de conteúdo para usuários.

1. Certifique-se de que o modelo **Contributor** esteja atualmente em um estado **Enabled**.

   ![Modelo de contribuidor habilitado](assets/content-fragment-models/enable-contributor-model.png)

   É possível alternar o estado de um Modelo de fragmento de conteúdo passando o mouse sobre o cartão e clicando no ícone **Disable** / **Enable**.

## Parabéns! {#congratulations}

Parabéns, você acabou de criar seu primeiro Modelo de fragmento de conteúdo!

## Próximas etapas {#next-steps}

No próximo capítulo, [Criação de modelos de fragmento de conteúdo](author-content-fragments.md), você criará e editará um novo Fragmento de conteúdo com base em um Modelo de fragmento de conteúdo. Você também aprenderá a criar variações de Fragmentos de conteúdo.