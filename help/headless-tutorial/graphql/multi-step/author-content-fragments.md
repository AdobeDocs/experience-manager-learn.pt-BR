---
title: Criação de fragmentos de conteúdo - Introdução ao AEM Headless - GraphQL
description: Introdução ao Adobe Experience Manager (AEM) e GraphQL. Crie e edite um novo Fragmento de conteúdo com base em um Modelo de fragmento de conteúdo. Saiba como criar variações de Fragmentos de conteúdo.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6713
thumbnail: 22451.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 701fae92-f740-4eb6-8133-1bc45a472d0f
duration: 173
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '853'
ht-degree: 2%

---

# Criação de fragmento de conteúdo {#authoring-content-fragments}

Neste capítulo, você cria e edita um novo Fragmento de conteúdo com base no [Modelo de fragmento de conteúdo recém-definido](./content-fragment-models.md). Você também aprenderá a criar variações de Fragmentos de conteúdo.

## Pré-requisitos {#prerequisites}

Este é um tutorial de várias partes e presume-se que as etapas descritas em [Definição dos modelos de fragmento de conteúdo](./content-fragment-models.md) foram concluídas.

## Objetivos {#objectives}

* Criar um fragmento de conteúdo com base em um modelo de fragmento de conteúdo
* Criar uma variação de Fragmento do conteúdo

## Criar uma pasta de ativos

Os fragmentos de conteúdo são armazenados em pastas no AEM Assets. Para criar fragmentos de conteúdo a partir dos modelos criados no capítulo anterior, uma pasta deve ser criada para armazená-los. Uma configuração é necessária na pasta para habilitar a criação de fragmentos de modelos específicos.

1. Na tela inicial do AEM, navegue até **Assets** > **Arquivos**.

   ![Navegar até os arquivos de ativos](assets/author-content-fragments/navigate-assets-files.png)

1. Toque em **Criar** no canto superior direito e em **Pasta**. Na caixa de diálogo resultante, digite:

   * Título*: **Meu Projeto**
   * Nome: **meu-projeto**

   ![Caixa de diálogo Criar pasta](assets/author-content-fragments/create-folder-dialog.png)

1. Selecione a pasta **Minha Pasta** e toque em **Propriedades**.

   ![Abrir propriedades da pasta](assets/author-content-fragments/open-folder-properties.png)

1. Toque na guia **Cloud Services**. Na guia Configuração na nuvem, use o localizador de caminhos para selecionar a configuração **Meu projeto**. O valor deve ser `/conf/my-project`.

   ![Definir configuração de nuvem](assets/author-content-fragments/set-cloud-config-my-project.png)

   Definir essa propriedade permite que os fragmentos de conteúdo sejam criados usando os modelos criados no capítulo anterior.

1. Toque na guia **Políticas**, no campo **Modelos de fragmento de conteúdo permitidos**, use o localizador de caminhos para selecionar o modelo **Pessoa** e **Equipe** criado anteriormente.

   ![Modelos de fragmento de conteúdo permitidos](assets/author-content-fragments/allowed-content-fragment-models.png)

   Essas políticas são herdadas automaticamente por qualquer subpasta e podem ser substituídas. Você também pode permitir modelos por tags ou ativar modelos de outras configurações do projeto. Esse mecanismo fornece uma maneira eficiente de gerenciar a hierarquia de conteúdo.

1. Toque em **Salvar e fechar** para salvar as alterações nas propriedades da pasta.

1. Navegue dentro da pasta **Meu Projeto**.

1. Crie outra pasta com os seguintes valores:

   * Título*: **Inglês**
   * Nome: **en**

   Uma prática recomendada é configurar projetos para suporte multilíngue. Consulte [a seguinte página de documentos para obter mais informações](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html).


## Criar um fragmento de conteúdo {#create-content-fragment}

>[!TIP]
>
>Para usuários locais do AEM SDK: utilize a interface do usuário do AEM Assets para criar e criar fragmentos de conteúdo, em vez da interface do usuário de fragmentos de conteúdo descrita abaixo. Para obter instruções detalhadas, consulte a [documentação do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html).

Em seguida, vários Fragmentos de conteúdo são criados com base nos modelos **Equipe** e **Pessoa**.

1. Na Tela inicial do AEM, toque em **Fragmentos de conteúdo** para abrir a interface dos Fragmentos de conteúdo.

   ![Interface do usuário de fragmento de conteúdo](assets/author-content-fragments/cf-fragment-ui.png)

1. No painel à esquerda, expanda **Meu projeto** e toque em **Inglês**.
1. Toque em **Criar** para abrir a caixa de diálogo **Novo fragmento de conteúdo** e inserir os seguintes valores:

   * Localização: `/content/dam/my-project/en`
   * Modelo de fragmento de conteúdo: **Pessoa**
   * Título: **João da Silva**
   * Nome: `john-doe`

   ![Novo fragmento de conteúdo](assets/author-content-fragments/new-content-fragment-john-doe.png)
1. Toque em **Criar**.
1. Repita as etapas acima para criar um fragmento representando **Alison Smith**:

   * Localização: `/content/dam/my-project/en`
   * Modelo de fragmento de conteúdo: **Pessoa**
   * Título: **Alison Smith**
   * Nome: `alison-smith`

   Toque em **Criar** para criar o fragmento de Pessoa.

1. Em seguida, repita as etapas para criar um fragmento de **Equipe** representando o **Alpha da Equipe**:

   * Localização: `/content/dam/my-project/en`
   * Modelo de Fragmento de Conteúdo: **Equipe**
   * Título: **Alpha da Equipe**
   * Nome: `team-alpha`

   Toque em **Criar** para criar o fragmento de Equipe.

1. Deve haver três fragmentos de conteúdo abaixo de **Meu projeto** > **Inglês**:

   ![Novos fragmentos de conteúdo](assets/author-content-fragments/new-content-fragments.png)

## Editar fragmentos de conteúdo de pessoa {#edit-person-content-fragments}

Em seguida, preencha os fragmentos recém-criados com dados.

1. Toque na caixa de seleção ao lado de **João da Silva** e toque em **Abrir**.

   ![Abrir fragmento de conteúdo](assets/author-content-fragments/open-fragment-for-editing.png)

1. O Editor de fragmento de conteúdo contém um formulário baseado no modelo de fragmento de conteúdo. Preencha os vários campos para adicionar conteúdo ao fragmento **João da Silva**. Para Foto do perfil, carregue sua própria imagem no AEM Assets.

   ![Editor de fragmento de conteúdo](assets/author-content-fragments/content-fragment-editor-jd.png)

1. Toque em **Salvar e fechar** para salvar as alterações no fragmento João da Silva.
1. Retorne à interface do usuário do fragmento de conteúdo e abra o arquivo **Alison Smith** para edição.
1. Repita as etapas acima para preencher o fragmento **Alison Smith** com conteúdo.

## Editar fragmento do conteúdo da equipe {#edit-team-content-fragment}

1. Abra o Fragmento de Conteúdo do **Team Alpha** usando a interface de Fragmento de Conteúdo.
1. Preencha os campos para **Título**, **Nome Abreviado** e **Descrição**.
1. Selecione os Fragmentos de Conteúdo de **João da Silva** e **Alison Smith** para preencher o campo **Membros da Equipe**:

   ![Definir membros da equipe](assets/author-content-fragments/select-team-members.png)

   >[!NOTE]
   >
   >Você também pode criar fragmentos de conteúdo embutidos usando o botão **Novo fragmento de conteúdo**.

1. Toque em **Salvar e fechar** para salvar as alterações no fragmento do Team Alpha.

## Publicar fragmentos de conteúdo

>[!TIP]
>
>Para usuários locais do AEM SDK: utilize a interface do usuário do AEM Assets para publicar fragmentos de conteúdo, em vez da interface do usuário de fragmentos de conteúdo descrita abaixo. Para obter instruções detalhadas, consulte a [documentação do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html#publishing-and-referencing-a-fragment).

Após a revisão e verificação, publique o `Content Fragments` criado

1. Na Tela inicial do AEM, toque em **Fragmentos de conteúdo** para abrir a interface dos Fragmentos de conteúdo.

1. No painel à esquerda, expanda **Meu projeto** e toque em **Inglês**.

1. Toque na caixa de seleção ao lado dos fragmentos de conteúdo e toque em **Publicar**.
   ![Publicar fragmento do conteúdo](assets/author-content-fragments/publish-content-fragment.png)

## Parabéns. {#congratulations}

Parabéns, você criou vários fragmentos de conteúdo e uma variação.

## Próximas etapas {#next-steps}

No próximo capítulo, [Explorar APIs do GraphQL](explore-graphql-api.md), você explorará as APIs do GraphQL da AEM usando a ferramenta GrapiQL integrada. Saiba como o AEM gera automaticamente um esquema do GraphQL com base em um modelo de Fragmento de conteúdo. Você experimentará a construção de consultas básicas usando a sintaxe do GraphQL.

## Documentação relacionada

* [Gerenciamento dos fragmentos de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html)
* [Variações - Criação dos fragmentos de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html?lang=pt-BR)
