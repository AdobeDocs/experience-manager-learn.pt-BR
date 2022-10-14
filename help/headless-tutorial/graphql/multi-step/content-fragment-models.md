---
title: Definição dos modelos de fragmento de conteúdo - Introdução ao AEM sem interface - GraphQL
description: Introdução à Adobe Experience Manager (AEM) e GraphQL. Saiba como modelar o conteúdo e criar um esquema com Modelos de fragmento de conteúdo em AEM. Revise modelos existentes e crie um modelo. Saiba mais sobre os diferentes tipos de dados que podem ser usados para definir um schema.
version: Cloud Service
mini-toc-levels: 1
kt: 6712
thumbnail: 22452.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 9400d9f2-f828-4180-95a7-2ac7b74cd3c9
source-git-commit: 25c289b093297e870c52028a759d05628d77f634
workflow-type: tm+mt
source-wordcount: '1115'
ht-degree: 2%

---

# Definição dos modelos de fragmento do conteúdo {#content-fragment-models}

Neste capítulo, saiba como modelar o conteúdo e criar um schema com **Modelos de fragmentos do conteúdo**. Saiba mais sobre os diferentes tipos de dados que podem ser usados para definir um schema como parte do modelo.

Nós criamos dois modelos simples. **Equipe** e **Pessoa**. O **Equipe** o modelo de dados tem nome, nome abreviado e descrição e faz referência ao **Pessoa** modelo de dados, que tem nome completo, detalhes da bio, imagem do perfil e lista de ocupações.

Você também é bem-vindo a criar seu próprio modelo seguindo as etapas básicas e ajustar as respectivas etapas, como consultas GraphQL, e o código do aplicativo React ou simplesmente seguir as etapas descritas nesses capítulos.

## Pré-requisitos {#prerequisites}

Este é um tutorial de várias partes e presume-se que um [AEM ambiente do autor está disponível](./overview.md#prerequisites).

## Objetivos {#objectives}

* Criar um modelo de fragmento de conteúdo.
* Identifique os tipos de dados disponíveis e as opções de validação para criar modelos.
* Entenda como o modelo de fragmento de conteúdo define **both** o schema de dados e o modelo de criação para um Fragmento de conteúdo.

## Criar uma configuração de projeto

Uma configuração de projeto contém todos os modelos de Fragmento de conteúdo associados a um projeto específico e fornece um meio de organizar modelos. Pelo menos um projeto deve ser criado **before** criação do Modelo de fragmento de conteúdo.

1. Faça logon no AEM **Autor** ambiente (ex. `https://author-pYYYY-eXXXX.adobeaemcloud.com/`)
1. Na tela inicial AEM, navegue até **Ferramentas** > **Geral** > **Navegador de configuração**.

   ![Navegar até o Navegador de configuração](assets/content-fragment-models/navigate-config-browser.png)
1. Clique em **Criar**, no canto superior direito
1. Na caixa de diálogo resultante, digite:

   * Título*: **Meu projeto**
   * Nome*: **meu projeto** (prefere usar todas as letras minúsculas usando hífens para separar palavras. Essa sequência de caracteres influencia o ponto de extremidade GraphQL exclusivo para o qual os aplicativos clientes executam solicitações.)
   * Verificar **Modelos de fragmentos do conteúdo**
   * Verificar **Consultas Persistentes GraphQL**

   ![Configuração do Meu Projeto](assets/content-fragment-models/my-project-configuration.png)

## Criar modelos de fragmento de conteúdo

Em seguida, crie dois modelos para um **Equipe** e **Pessoa**.

### Criar o modelo de pessoa

Criar um modelo para um **Pessoa**, que é o modelo de dados que representa uma pessoa que faz parte de uma equipe.

1. Na tela inicial AEM, navegue até **Ferramentas** > **Geral** > **Modelos de fragmentos do conteúdo**.

   ![Navegar até Modelos de fragmentos do conteúdo](assets/content-fragment-models/navigate-cf-models.png)

1. Navegue até o **Meu projeto** pasta.
1. Toque **Criar** no canto superior direito para trazer o **Criar modelo** assistente.
1. Em **Título do modelo** , insira **Pessoa** e tocar **Criar**. Na caixa de diálogo resultante, toque em **Abrir**, para criar o modelo.

1. Arrastar e soltar uma **Texto de linha única** no painel principal. Insira as seguintes propriedades no **Propriedades** guia :

   * **Rótulo do campo**: **Nome completo**
   * **Nome da Propriedade**: `fullName`
   * Verificar **Obrigatório**

   ![Campo de propriedade Nome completo](assets/content-fragment-models/full-name-property-field.png)

   O **Nome da propriedade** define o nome da propriedade que é mantida para AEM. O **Nome da propriedade** também define a variável **key** nome dessa propriedade como parte do schema de dados. Essa **key** é usada quando os dados do Fragmento de conteúdo são expostos por meio de APIs GraphQL.

1. Toque no **Tipos de dados** e arraste e solte uma **Texto de várias linhas** abaixo do campo **Nome completo** campo. Insira as seguintes propriedades:

   * **Rótulo do campo**: **Biografia**
   * **Nome da Propriedade**: `biographyText`
   * **Tipo padrão**: **Texto formatado**

1. Clique no botão **Tipos de dados** e arraste e solte uma **Referência de conteúdo** campo. Insira as seguintes propriedades:

   * **Rótulo do campo**: **Imagem do perfil**
   * **Nome da Propriedade**: `profilePicture`
   * **Caminho raiz**: `/content/dam`

   Ao configurar o **Caminho raiz**, você pode clicar no botão **pasta** ícone para exibir uma modal e selecionar o caminho. Isso restringe quais pastas os autores podem usar para preencher o caminho. `/content/dam` é a raiz na qual todos os AEM Assets (imagens, vídeos, outros Fragmentos de conteúdo) são armazenados.

1. Adicionar uma validação ao **Referência de imagem** para que somente os tipos de conteúdo **Imagens** pode ser usada para preencher o campo.

   ![Restringir a imagens](assets/content-fragment-models/picture-reference-content-types.png)

1. Clique no botão **Tipos de dados** e arraste e solte uma **Enumeração**  tipo de dados abaixo do **Referência de imagem** campo. Insira as seguintes propriedades:

   * **Renderizar como**: **Caixas de seleção**
   * **Rótulo do campo**: **Ocupação**
   * **Nome da Propriedade**: `occupation`

1. Adicione vários **Opções** usando o **Adicionar uma opção** botão. Use o mesmo valor para **Rótulo da opção** e **Valor da opção**:

   **Artista**, **Influenciador**, **Fotógrafo**, **Viajante**, **Escritor**, **YouTuber**

1. A final **Pessoa** O modelo deve ser semelhante ao seguinte:

   ![Modelo de pessoa final](assets/content-fragment-models/final-author-model.png)

1. Clique em **Salvar** para salvar as alterações.

### Criar o modelo de equipe

Criar um modelo para um **Equipe**, que é o modelo de dados para uma equipe de pessoas. O modelo Equipe faz referência ao modelo Pessoa para representar os membros da equipe.

1. No **Meu projeto** pasta, toque em **Criar** no canto superior direito para trazer o **Criar modelo** assistente.
1. Em **Título do modelo** , insira **Equipe** e tocar **Criar**.

   Toque **Abrir** na caixa de diálogo resultante, para abrir o modelo recém-criado.

1. Arrastar e soltar uma **Texto de linha única** no painel principal. Insira as seguintes propriedades no **Propriedades** guia :

   * **Rótulo do campo**: **Título**
   * **Nome da Propriedade**: `title`
   * Verificar **Obrigatório**

1. Toque no **Tipos de dados** e arraste e solte uma **Texto de linha única** no painel principal. Insira as seguintes propriedades no **Propriedades** guia :

   * **Rótulo do campo**: **Nome abreviado**
   * **Nome da Propriedade**: `shortName`
   * Verificar **Obrigatório**
   * Verificar **Exclusivo**
   * Em, **Tipo de validação** > escolha **Personalizado**
   * Em, **Regex de validação personalizada** > enter `^[a-z0-9\-_]{5,40}$` - isso garante que apenas valores alfanuméricos minúsculos e traços de 5 a 40 caracteres possam ser inseridos.

   O `shortName` fornece uma maneira de consultar uma equipe individual com base em um caminho encurtado. O **Exclusivo** essa configuração garante que o valor seja sempre único por Fragmento de conteúdo desse modelo.

1. Toque no **Tipos de dados** e arraste e solte uma **Texto de várias linhas** abaixo do campo **Nome abreviado** campo. Insira as seguintes propriedades:

   * **Rótulo do campo**: **Descrição**
   * **Nome da Propriedade**: `description`
   * **Tipo padrão**: **Texto formatado**

1. Clique no botão **Tipos de dados** e arraste e solte uma **Referência do fragmento** campo. Insira as seguintes propriedades:

   * **Renderizar como**: **Campo Múltiplo**
   * **Rótulo do campo**: **Membros da equipe**
   * **Nome da Propriedade**: `teamMembers`
   * **Modelos permitidos de fragmento do conteúdo**: Use o ícone de pasta para selecionar o **Pessoa** modelo.

1. A final **Equipe** O modelo deve ser semelhante ao seguinte:

   ![Modelo de Equipe Final](assets/content-fragment-models/final-team-model.png)

1. Clique em **Salvar** para salvar as alterações.

1. Agora você deve ter dois modelos para trabalhar:

   ![Dois modelos](assets/content-fragment-models/two-new-models.png)

## Publicar a configuração do projeto e os modelos de fragmento do conteúdo

Após análise e verificação, publique o `Project Configuration` &amp; `Content Fragment Model`

1. Na tela inicial AEM, navegue até **Ferramentas** > **Geral** > **Navegador de configuração**.

1. Toque na caixa de seleção ao lado de **Meu projeto** e tocar **Publicar**

   ![Publicar configuração do projeto](assets/content-fragment-models/publish-project-config.png)

1. Na tela inicial AEM, navegue até **Ferramentas** > **Geral** > **Modelos de fragmentos do conteúdo**.

1. Navegue até o **Meu projeto** pasta.

1. Toque **Pessoa** e **Equipe** modelos e toque **Publicar**

   ![Publicar modelos de fragmento do conteúdo](assets/content-fragment-models/publish-content-fragment-model.png)

## Parabéns.  {#congratulations}

Parabéns, você acabou de criar seus primeiros Modelos de fragmento de conteúdo!

## Próximas etapas {#next-steps}

No próximo capítulo, [Criação de modelos de fragmentos de conteúdo](author-content-fragments.md), crie e edite um novo Fragmento de conteúdo com base em um Modelo de fragmento de conteúdo. Você também aprenderá a criar variações de Fragmentos de conteúdo.

## Documentação relacionada

* [Modelos de fragmentos do conteúdo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)

