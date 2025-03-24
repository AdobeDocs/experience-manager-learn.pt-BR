---
title: Definição de modelos de fragmento de conteúdo - Introdução ao AEM Headless - GraphQL
description: Introdução ao Adobe Experience Manager (AEM) e GraphQL. Saiba como modelar conteúdo e criar um esquema com modelos de fragmento de conteúdo no AEM. Revise os modelos existentes e crie um modelo. Saiba mais sobre os diferentes tipos de dados que podem ser usados para definir um esquema.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6712
thumbnail: 22452.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 9400d9f2-f828-4180-95a7-2ac7b74cd3c9
duration: 228
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1110'
ht-degree: 2%

---

# Definição de Modelos de fragmentos de conteúdo {#content-fragment-models}

Neste capítulo, saiba como modelar conteúdo e criar um esquema com **Modelos de fragmentos de conteúdo**. Você aprende sobre os diferentes tipos de dados que podem ser usados para definir um esquema como parte do modelo.

Criamos dois modelos simples, **Equipe** e **Pessoa**. O modelo de dados da **Equipe** tem nome, nome curto e descrição e faz referência ao modelo de dados **Pessoa**, que tem nome completo, biodetalhes, imagem de perfil e lista de ocupações.

Você também é bem-vindo para criar seu próprio modelo seguindo as etapas básicas e ajustar as respectivas etapas, como consultas do GraphQL e código do aplicativo React, ou simplesmente seguir as etapas descritas nestes capítulos.

## Pré-requisitos {#prerequisites}

Este é um tutorial em várias partes e presume-se que um [ambiente de criação do AEM esteja disponível](./overview.md#prerequisites).

## Objetivos {#objectives}

* Crie um modelo de fragmento de conteúdo.
* Identifique os tipos de dados disponíveis e as opções de validação para criar modelos.
* Entenda como o Modelo de fragmento de conteúdo define **ambos** o esquema de dados e o modelo de criação de um Fragmento de conteúdo.

## Criar uma configuração de projeto

Uma configuração de projeto contém todos os modelos de Fragmento de conteúdo associados a um projeto específico e fornece um meio de organizar modelos. Pelo menos um projeto deve ser criado **antes** de criar o Modelo de fragmento de conteúdo.

1. Fazer logon no ambiente **Author** do AEM (ex. `https://author-pYYYY-eXXXX.adobeaemcloud.com/`)
1. Na tela inicial do AEM, navegue até **Ferramentas** > **Geral** > **Navegador de Configuração**.

   ![Navegar até o Navegador de Configuração](assets/content-fragment-models/navigate-config-browser.png)
1. Clique em **Criar**, no canto superior direito
1. Na caixa de diálogo resultante, digite:

   * Título*: **Meu Projeto**
   * Nome*: **meu-projeto** (prefira usar todas as letras minúsculas usando hifens para separar palavras. Essa string influencia o endpoint exclusivo do GraphQL no qual os aplicativos clientes executam solicitações.)
   * Verificar **Modelos de fragmentos do conteúdo**
   * Verificar **Consultas Persistentes do GraphQL**

   ![Configuração do Meu Projeto](assets/content-fragment-models/my-project-configuration.png)

## Criar modelos de fragmentos de conteúdo

Em seguida, crie dois modelos para uma **Equipe** e uma **Pessoa**.

### Criar o modelo de pessoa

Crie um modelo para uma **Pessoa**, que é o modelo de dados que representa uma pessoa que faz parte de uma equipe.

1. Na tela inicial do AEM, navegue até **Ferramentas** > **Geral** > **Modelos de fragmentos de conteúdo**.

   ![Navegar até os modelos de fragmento de conteúdo](assets/content-fragment-models/navigate-cf-models.png)

1. Navegue até a pasta **Meu Projeto**.
1. Toque em **Criar** no canto superior direito para abrir o assistente **Criar Modelo**.
1. No campo **Título do Modelo**, digite **Pessoa** e toque em **Criar**. Na caixa de diálogo resultante, toque em **Abrir** para criar o modelo.

1. Arraste e solte um elemento **Texto de linha única** no painel principal. Insira as seguintes propriedades na guia **Propriedades**:

   * **Rótulo do campo**: **Nome completo**
   * **Nome da Propriedade**: `fullName`
   * Verificar **Obrigatório**

   ![Campo de propriedade Nome Completo](assets/content-fragment-models/full-name-property-field.png)

   O **Nome da Propriedade** define o nome da propriedade que é persistida no AEM. O **Nome da Propriedade** também define o nome **chave** dessa propriedade como parte do esquema de dados. Esta **chave** é usada quando os dados do Fragmento de conteúdo são expostos por meio de APIs do GraphQL.

1. Toque na guia **Tipos de dados** e arraste e solte um campo **Texto de várias linhas** abaixo do campo **Nome completo**. Insira as seguintes propriedades:

   * **Rótulo do Campo**: **Biografia**
   * **Nome da Propriedade**: `biographyText`
   * **Tipo Padrão**: **Rich Text**

1. Clique na guia **Tipos de Dados** e arraste e solte um campo **Referência de Conteúdo**. Insira as seguintes propriedades:

   * **Rótulo do Campo**: **Imagem do Perfil**
   * **Nome da Propriedade**: `profilePicture`
   * **Caminho raiz**: `/content/dam`

   Ao configurar o **Caminho raiz**, você pode clicar no ícone de **pasta** para exibir uma modal e selecionar o caminho. Restringe quais pastas os autores podem usar para preencher o caminho. `/content/dam` é a raiz na qual todas as AEM Assets (imagens, vídeos, outros fragmentos de conteúdo) são armazenadas.

1. Adicione uma validação à **Referência da Imagem** para que somente os tipos de conteúdo de **Imagens** possam ser usados para preencher o campo.

   ![Restringir a imagens](assets/content-fragment-models/picture-reference-content-types.png)

1. Clique na guia **Tipos de Dados** e arraste e solte um tipo de dados **Enumeração** abaixo do campo **Referência de Imagem**. Insira as seguintes propriedades:

   * **Renderizar como**: **Caixas de seleção**
   * **Rótulo do campo**: **Ocupação**
   * **Nome da Propriedade**: `occupation`

1. Adicione várias **Opções** usando o botão **Adicionar uma opção**. Use o mesmo valor para **Rótulo de Opção** e **Valor de Opção**:

   **Artista**, **Influenciador**, **Fotógrafo**, **Viajante**, **Escritor**, **YouTuber**

1. O modelo final **Pessoa** deve ser semelhante ao seguinte:

   ![Modelo Final De Pessoa](assets/content-fragment-models/final-author-model.png)

1. Clique em **Salvar** para salvar as alterações.

### Criar o modelo de equipe

Crie um modelo para uma **Equipe**, que é o modelo de dados para uma equipe de pessoas. O modelo Equipe faz referência ao modelo Pessoa para representar os membros da equipe.

1. Na pasta **Meu Projeto**, toque em **Criar** no canto superior direito para abrir o assistente **Criar Modelo**.
1. No campo **Título do Modelo**, insira **Equipe** e toque em **Criar**.

   Toque em **Abrir** na caixa de diálogo resultante para abrir o modelo recém-criado.

1. Arraste e solte um elemento **Texto de linha única** no painel principal. Insira as seguintes propriedades na guia **Propriedades**:

   * **Rótulo do campo**: **Título**
   * **Nome da Propriedade**: `title`
   * Verificar **Obrigatório**

1. Toque na guia **Tipos de dados** e arraste e solte um elemento de **Texto de linha única** no painel principal. Insira as seguintes propriedades na guia **Propriedades**:

   * **Rótulo do Campo**: **Nome Curto**
   * **Nome da Propriedade**: `shortName`
   * Verificar **Obrigatório**
   * Verificar **Exclusivo**
   * Em, **Tipo de validação** > escolha **Personalizado**
   * Em, **Regex de Validação Personalizada** > insira `^[a-z0-9\-_]{5,40}$` - isso garante que somente valores alfanuméricos em minúsculas e traços de 5 a 40 caracteres possam ser inseridos.

   A propriedade `shortName` nos fornece uma maneira de consultar uma equipe individual com base em um caminho encurtado. A configuração **Exclusivo** garante que o valor seja sempre exclusivo por Fragmento de conteúdo deste modelo.

1. Toque na guia **Tipos de Dados** e arraste e solte um campo **Texto de várias linhas** abaixo do campo **Nome Curto**. Insira as seguintes propriedades:

   * **Rótulo do Campo**: **Descrição**
   * **Nome da Propriedade**: `description`
   * **Tipo Padrão**: **Rich Text**

1. Clique na guia **Tipos de dados** e arraste e solte um campo **Referência de fragmento**. Insira as seguintes propriedades:

   * **Renderizar como**: **Vários Campos**
   * **Rótulo do Campo**: **Membros da Equipe**
   * **Nome da Propriedade**: `teamMembers`
   * **Modelos de fragmento de conteúdo permitidos**: use o ícone de pasta para selecionar o modelo **Pessoa**.

1. O modelo final **Equipe** deve ser semelhante ao seguinte:

   ![Modelo final da equipe](assets/content-fragment-models/final-team-model.png)

1. Clique em **Salvar** para salvar as alterações.

1. Agora você deve ter dois modelos para trabalhar:

   ![Dois Modelos](assets/content-fragment-models/two-new-models.png)

## Publicar configuração do projeto e modelos de fragmento de conteúdo

Após a revisão e verificação, publique os `Project Configuration` e `Content Fragment Model`

1. Na tela inicial do AEM, navegue até **Ferramentas** > **Geral** > **Navegador de Configuração**.

1. Toque na caixa de seleção ao lado de **Meu projeto** e toque em **Publicar**

   ![Publicar configuração do projeto](assets/content-fragment-models/publish-project-config.png)

1. Na tela inicial do AEM, navegue até **Ferramentas** > **Geral** > **Modelos de fragmentos de conteúdo**.

1. Navegue até a pasta **Meu Projeto**.

1. Toque nos modelos **Pessoa** e **Equipe** e em **Publicar**

   ![Publicar modelos de fragmento de conteúdo](assets/content-fragment-models/publish-content-fragment-model.png)

## Parabéns. {#congratulations}

Parabéns, você acabou de criar seus primeiros modelos de fragmento de conteúdo!

## Próximas etapas {#next-steps}

No próximo capítulo, [Criação de modelos de fragmento de conteúdo](author-content-fragments.md), você cria e edita um novo fragmento de conteúdo com base em um modelo de fragmento de conteúdo. Você também aprenderá a criar variações de Fragmentos de conteúdo.

## Documentação relacionada

* [Modelos de fragmentos do conteúdo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)

