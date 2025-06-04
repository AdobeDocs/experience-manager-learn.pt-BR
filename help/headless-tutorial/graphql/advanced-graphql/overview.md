---
title: Conceitos avançados do AEM Headless — GraphQL
description: Um tutorial completo que ilustra conceitos avançados das APIs GraphQL do Adobe Experience Manager (AEM).
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: daae6145-5267-4958-9abe-f6b7f469f803
duration: 441
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: ht
source-wordcount: '1052'
ht-degree: 100%

---

# Conceitos avançados do AEM Headless

Este tutorial completo continua o [tutorial básico](../multi-step/overview.md) que aborda os fundamentos do Adobe Experience Manager (AEM) Headless e do GraphQL. O tutorial avançado ilustra os aspectos detalhados do trabalho com modelos de fragmento de conteúdo, fragmentos de conteúdo e as consultas persistentes do GraphQL do AEM, incluindo o uso das consultas persistentes do GraphQL em um aplicativo cliente.

## Pré-requisitos

Conclua a [instalação rápida do AEM as a Cloud Service](../quick-setup/cloud-service.md) para configurar seu ambiente do AEM as a Cloud Service.

É altamente recomendável que você conclua o [tutorial básico](../multi-step/overview.md) anterior e assista a [série de vídeos](../video-series/modeling-basics.md) antes de prosseguir com este tutorial avançado. Embora você possa concluir o tutorial usando um ambiente AEM local, este tutorial aborda apenas o fluxo de trabalho do AEM as a Cloud Service.

>[!CAUTION]
>
>Se você não tiver acesso ao ambiente do AEM as a Cloud Service, poderá concluir a [instalação rápida do AEM Headless usando o SDK local](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/quick-setup/local-sdk.html?lang=pt-BR). No entanto, é importante observar que algumas páginas da interface do produto, como a navegação por Fragmentos de conteúdo, são diferentes.



## Objetivos

Este tutorial aborda os seguintes tópicos:

* Crie modelos de fragmento de conteúdo usando regras de validação e tipos de dados mais avançados, como Espaços reservados para guias, referências de fragmentos aninhados, objetos JSON e tipos de dados de data e hora.
* Crie fragmentos de conteúdo enquanto trabalha com referências de conteúdo e fragmento aninhadas e configure políticas de pastas para o controle de criação de fragmentos de conteúdo.
* Explore os recursos da API GraphQL do AEM usando consultas do GraphQL com variáveis e diretivas.
* Mantenha consultas do GraphQL com parâmetros no AEM e saiba como usar parâmetros de controle de cache com consultas persistentes.
* Integre solicitações de consultas persistentes no aplicativo GraphQL React da WKND de amostra usando o SDK JavaScript do AEM Headless.

## Conceitos avançados de visão geral do AEM Headless

O vídeo a seguir fornece uma visão geral de alto nível dos conceitos abordados neste tutorial. O tutorial inclui a definição de Modelos de fragmento de conteúdo com tipos de dados mais avançados, o aninhamento de fragmentos de conteúdo e a persistência de consultas do GraphQL no AEM.

>[!VIDEO](https://video.tv.adobe.com/v/340035?quality=12&learn=on)

>[!CAUTION]
>
>Este vídeo (aos 2:25) menciona a instalação do editor de consultas GraphiQL por meio do Gerenciador de pacotes para explorar consultas do GraphQL. No entanto, em versões mais recentes do AEM as Cloud Service, um **GraphiQL Explorer** integrado é fornecido, portanto, a instalação do pacote não é necessária. Consulte [Uso do GraphiQL IDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html?lang=pt-BR) para obter mais informações.


## Configuração do projeto

O projeto do Site da WKND tem todas as configurações necessárias, portanto, você pode iniciar o tutorial logo após concluir a [configuração rápida](../quick-setup/cloud-service.md). Esta seção destaca apenas algumas etapas importantes que você pode usar ao criar seu próprio projeto do AEM Headless.


### Analisar a configuração já existente

A primeira etapa para iniciar qualquer novo projeto no AEM é criar sua configuração como um espaço de trabalho e criar pontos de acesso da API GraphQL. Para analisar ou criar uma configuração, navegue até **Ferramentas** > **Geral** > **Navegador de configuração**.

![Ir até o Navegador de configuração](assets/overview/create-configuration.png)

Observe que a configuração de site `WKND Shared` já foi criada para o tutorial. Para criar uma configuração para o seu próprio projeto, selecione **Criar** no canto superior direito e preencha o formulário no modal “Criar configuração” exibido.

![Analisar a configuração compartilhada da WKND](assets/overview/review-wknd-shared-configuration.png)

### Analisar os pontos de acesso da API GraphQL

Em seguida, você deve configurar os pontos de acesso da API para os quais enviar consultas GraphQL. Para analisar os pontos de acesso já existentes ou criar um, navegue até **Ferramentas** > **Geral** > **GraphQL**.

![Configurar pontos de acesso](assets/overview/endpoints.png)

Observe que `WKND Shared Endpoint` já foi criado. Para criar um ponto de acesso para o seu projeto, selecione **Criar** no canto superior direito e siga o fluxo de trabalho.

![Analisar o ponto de acesso compartilhado da WKND](assets/overview/review-wknd-shared-endpoint.png)

>[!NOTE]
>
> Depois de salvar o ponto de acesso, você verá um modal sobre como visitar o Console de segurança, que permite ajustar as configurações de segurança se desejar configurar o acesso ao ponto de acesso. No entanto, as permissões de segurança em si estão fora do escopo deste tutorial. Para obter mais informações, consulte a [documentação do AEM](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=pt-BR).

### Analisar a estrutura de conteúdo da WKND e a pasta raiz de idioma

Uma estrutura de conteúdo bem definida é a chave para o sucesso da implementação headless do AEM. É útil para o gerenciamento de escalabilidade, usabilidade e permissões do seu conteúdo.

Uma pasta raiz de idioma é uma pasta cujo nome é um código de idioma ISO, como EN ou FR. O sistema de gerenciamento de tradução do AEM usa essas pastas para definir o idioma principal do conteúdo e os idiomas para a tradução de conteúdo.

Vá para **Navegação** > **Ativos** > **Arquivos**.

![Navegar até Arquivos](assets/overview/files.png)

Navegue até a pasta **WKND Compartilhado**. Observe a pasta com o título “Inglês” e o nome “EN”. Esta pasta é a pasta raiz do idioma para o projeto do Site da WKND.

![Pasta Inglês](assets/overview/english.png)

Para seu próprio projeto, crie uma pasta raiz de idioma dentro da sua configuração. Consulte a seção sobre [criação de pastas](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders) para obter mais detalhes.

### Atribuir uma configuração à pasta aninhada

Por fim, você deve atribuir a configuração do projeto à pasta raiz do idioma. Essa atribuição permite a criação de fragmentos de conteúdo com base em modelos de fragmento de conteúdo definidos na configuração do seu projeto.

Para atribuir a pasta raiz do idioma à configuração, selecione a pasta e, em seguida, **Propriedades** na barra de navegação superior.

![Selecione Propriedades](assets/overview/properties.png)

Em seguida, navegue até a guia **Serviços de nuvem** e selecione o ícone de pasta no campo **Configuração da nuvem**.

![Configuração na nuvem](assets/overview/cloud-conf.png)

No modal exibido, selecione a configuração criada anteriormente para atribuir a pasta raiz de idioma a ela.

### Práticas recomendadas

Estas são as práticas recomendadas ao criar seu próprio projeto no AEM:

* A hierarquia de pastas deve ser modelada tendo em mente a localização e a tradução. Em outras palavras, as pastas de idioma devem ser aninhadas nas pastas de configuração, o que permite uma fácil tradução do conteúdo nessas pastas de configuração.
* A hierarquia de pastas deve ser mantida simples e direta. Evite mover ou renomear pastas e fragmentos posteriormente, em especial depois da publicação para uso em tempo real, pois altera caminhos que podem afetar referências de fragmentos e consultas do GraphQL.

## Pacotes iniciais e de solução

Dois **pacotes** do AEM estão disponíveis e podem ser instalados via [Gerenciador de Pacotes](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)

* [Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip) é usado posteriormente no tutorial e contém imagens e pastas de exemplo.
* [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip) contém a solução concluída para os Capítulos 1 a 4, incluindo novos modelos de fragmento de conteúdo, fragmentos de conteúdo e consultas persistentes do GraphQL. Útil para quem deseja pular diretamente para o capítulo [Integração de aplicativos cliente](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).


O projeto [Aplicativo React — Tutorial Avançado — Aventuras WKND](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/advanced-tutorial/README.md) está disponível para examinar e explorar o aplicativo de amostra. Este aplicativo de amostra recupera o conteúdo do AEM invocando as consultas persistentes do GraphQL e as renderiza em uma experiência imersiva.

## Introdução

Para começar a usar este tutorial avançado, siga estas etapas:

1. Configure um ambiente de desenvolvimento usando o [AEM as a Cloud Service](../quick-setup/cloud-service.md).
1. Inicie o capítulo do tutorial em [Criar modelos de fragmento de conteúdo](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md).
