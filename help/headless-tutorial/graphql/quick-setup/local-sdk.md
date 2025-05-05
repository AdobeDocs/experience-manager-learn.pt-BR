---
title: Configuração rápida do AEM Headless usando o AEM SDK local
description: Introdução ao Adobe Experience Manager (AEM) e GraphQL. Instale o AEM SDK, adicione exemplos de conteúdo e implante um aplicativo que consuma conteúdo do AEM usando suas APIs do GraphQL. Veja como o AEM capacita experiências omnicanal.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d2da6efa-1f77-4391-adda-e3180c42addc
duration: 242
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1151'
ht-degree: 1%

---

# Configuração rápida do AEM Headless usando o AEM SDK local {#setup}

A configuração rápida do AEM Headless oferece uma abordagem prática do AEM Headless usando o conteúdo do projeto de amostra do WKND Site e uma amostra do aplicativo React (um SPA) que consome o conteúdo por meio das APIs do AEM Headless GraphQL. Este guia usa o [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=pt-BR).

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v18](https://nodejs.org/en/)
* [Git](https://git-scm.com/)

## 1. Instalar o AEM SDK {#aem-sdk}

Esta instalação usa o [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=pt-BR&#aem-as-a-cloud-service-sdk) para explorar as APIs GraphQL da AEM. Esta seção fornece um guia rápido para instalar o AEM SDK e executá-lo no modo Autor. Um guia mais detalhado para configurar um ambiente de desenvolvimento local [pode ser encontrado aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR#local-development-environment-set-up).

>[!NOTE]
>
> Também é possível seguir o tutorial com um [ambiente AEM as a Cloud Service](./cloud-service.md). Observações adicionais para usar um ambiente de nuvem são incluídas no tutorial.

1. Navegue até o **[Portal de Distribuição de Software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)** > **AEM as a Cloud Service** e baixe a versão mais recente do **AEM SDK**.

   ![Portal de Distribuição de Software](assets/quick-setup/aem-sdk/downloads__aem-sdk.png)

1. Descompacte o download e copie o Quickstart jar (`aem-sdk-quickstart-XXX.jar`) em uma pasta dedicada, ou seja, `~/aem-sdk/author`.
1. Renomeie o arquivo jar como `aem-author-p4502.jar`.

   O nome `author` especifica que o Quickstart jar inicia no modo Autor. O `p4502` especifica que o Quickstart é executado na porta 4502.

1. Para instalar e iniciar a instância do AEM, abra um prompt de comando na pasta que contém o arquivo jar e execute o seguinte comando:

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. Forneça uma senha de administrador como `admin`. Qualquer senha de administrador é aceitável, no entanto, é recomendável usar `admin` para desenvolvimento local para reduzir a necessidade de reconfigurar.
1. Quando o serviço AEM terminar a instalação, uma nova janela do navegador deverá ser aberta em [http://localhost:4502](http://localhost:4502).
1. Faça logon com o nome de usuário `admin` e a senha selecionada durante a primeira inicialização do AEM (geralmente `admin`).

## 2. Instalar conteúdo de amostra {#install-sample-content}

O conteúdo de amostra do **site de Referência WKND** é usado para acelerar o tutorial. A WKND é uma marca fictícia de estilo de vida, geralmente usada com treinamento em AEM.

O site WKND inclui configurações necessárias para expor um [ponto de extremidade do GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html?lang=pt-BR). Em uma implementação real, siga as etapas documentadas para [incluir os pontos de extremidade do GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html?lang=pt-BR) no projeto do cliente. Um [CORS](#cors-config) também foi empacotado como parte do Site WKND. Uma configuração do CORS é necessária para conceder acesso a um aplicativo externo. Mais informações sobre o [CORS](#cors-config) podem ser encontradas abaixo.

1. Baixe o Pacote do AEM compilado mais recente para o Site WKND: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Baixe a versão padrão compatível com o AEM as a Cloud Service e **não** a versão `classic`.

1. No menu **Início do AEM**, navegue até **Ferramentas** > **Implantação** > **Pacotes**.

   ![Navegar até Pacotes](assets/quick-setup/aem-sdk/aem-sdk__packages.png)

1. Clique em **Carregar Pacote** e escolha o pacote WKND baixado na etapa anterior. Clique em **Instalar** para instalar o pacote.

1. No menu **AEM Start**, navegue até **Assets** > **Arquivos** > **WKND Compartilhado** > **Inglês** > **Aventuras**.

   ![Exibição de pasta de aventuras](assets/quick-setup/aem-sdk/aem-sdk__assets-folder.png)

   Esta é uma pasta de todos os ativos que compõem as várias Aventuras promovidas pela marca WKND. Isso inclui tipos de mídia tradicionais, como imagens e vídeo, e mídias específicas do AEM, como **Fragmentos de conteúdo**.

1. Clique na pasta **Downhill Skiing Wyoming** e clique no cartão **Downhill Skiing Wyoming Content Fragment**:

   ![Cartão do Fragmento do conteúdo](assets/quick-setup/aem-sdk/aem-sdk__content-fragment.png)

1. O editor de Fragmento de conteúdo abre para a aventura Downhill Skiing Wyoming.

   ![Editor de fragmento de conteúdo](assets/quick-setup/aem-sdk/aem-sdk__content-fragment-editor.png)

   Observe que vários campos como **Título**, **Descrição** e **Atividade** definem o fragmento.

   **Fragmentos de conteúdo** são uma das maneiras pelas quais o conteúdo pode ser gerenciado no AEM. Fragmento de conteúdo são conteúdos reutilizáveis e de apresentação independente, compostos de elementos de dados estruturados, como texto, rich text, datas ou referências a outros Fragmentos de conteúdo. Os fragmentos de conteúdo são explorados com mais detalhes posteriormente na configuração rápida.

1. Clique em **Cancelar** para fechar o fragmento. Sinta-se à vontade para navegar em algumas das outras pastas e explorar o outro conteúdo de aventura.

>[!NOTE]
>
> Se estiver usando um ambiente do Cloud Service, consulte a documentação para saber como [implantar uma base de código como o site de Referência WKND em um ambiente do Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=pt-BR#coding-against-the-right-aem-version).

## 3. Baixe e execute o aplicativo WKND React {#sample-app}

Um dos objetivos deste tutorial é mostrar como consumir conteúdo do AEM de um aplicativo externo usando as APIs do GraphQL. Este tutorial usa um aplicativo React de exemplo. O aplicativo React é intencionalmente simples, para se concentrar na integração com as APIs do GraphQL do AEM.

1. Abra um novo prompt de comando e clone o aplicativo React de amostra do GitHub:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Abra o aplicativo React no `aem-guides-wknd-graphql/react-app` no IDE de sua escolha.
1. No IDE, abra o arquivo `.env.development` em `/.env.development`. Verifique se a linha `REACT_APP_AUTHORIZATION` não está comentada e se o arquivo declara as seguintes variáveis:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   Verifique se `REACT_APP_HOST_URI` aponta para o AEM SDK local. Para maior comodidade, este início rápido conecta o aplicativo React ao **AEM Author**. Os serviços do **Author** exigem autenticação, portanto, o aplicativo usa o usuário `admin` para estabelecer sua conexão. Conectar um aplicativo ao AEM Author é uma prática comum durante o desenvolvimento, pois facilita a iteração rápida no conteúdo, sem a necessidade de publicar as alterações.

   >[!NOTE]
   >
   > Em um cenário de produção, o Aplicativo se conectará a um ambiente de **Publicação** do AEM. Isso é abordado com mais detalhes na seção _Implantação de produção_.


1. Instale e inicie o aplicativo React:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Uma nova janela do navegador abre automaticamente o aplicativo em [http://localhost:3000](http://localhost:3000).

   ![Aplicativo inicial do React](assets/quick-setup/aem-sdk/react-app__home-view.png)

   Uma lista do conteúdo de aventura do AEM é exibida.

1. Clique em uma das imagens da aventura para exibir os detalhes da aventura. Uma solicitação é feita ao AEM para retornar os detalhes de uma aventura.

   ![exibição de Detalhes da Aventura](assets/quick-setup/aem-sdk/react-app__adventure-view.png)

1. Use as ferramentas de desenvolvedor do navegador para inspecionar as solicitações de **Rede**. Exiba as solicitações de **XHR** e observe várias solicitações do GET para `/graphql/execute.json/...`. Esse prefixo de caminho chama o endpoint de consulta persistente do AEM, selecionando a consulta persistente a ser executada usando o nome e os parâmetros codificados após o prefixo.

   ![Solicitação XHR do Ponto de Extremidade do GraphQL](assets/quick-setup/aem-sdk/react-app__graphql-request.png)

## 4. Editar conteúdo no AEM

Com o aplicativo React em execução, faça uma atualização do conteúdo no AEM e veja se a alteração se reflete no aplicativo.

1. Navegue até AEM [http://localhost:4502](http://localhost:4502).
1. Navegue até **Assets** > **Arquivos** > **WKND Compartilhado** > **Inglês** > **Aventuras** > **[Bali Surf Camp](http://localhost:4502/assets.html/content/dam/wknd-shared/en/adventures/bali-surf-camp)**.

   ![Pasta do Acampamento de Surf de Bali](assets/setup/bali-surf-camp-folder.png)

1. Clique no fragmento de conteúdo do **Bali Surf Camp** para abrir o editor de fragmentos de conteúdo.
1. Modifique o **Título** e a **Descrição** da aventura.

   ![Modificar fragmento de conteúdo](assets/setup/modify-content-fragment-bali.png)

1. Clique em **Salvar** para salvar as alterações.
1. Atualize o aplicativo React em [http://localhost:3000](http://localhost:3000) para ver suas alterações:

   ![Aventura sobre o Acampamento de Surf de Bali atualizada](assets/setup/overnight-bali-surf-camp-changes.png)

## 5. Explorar GraphiQL {#graphiql}

1. Abra o [GraphiQL](http://localhost:4502/aem/graphiql.html) navegando até **Ferramentas** > **Geral** > **Editor de consultas do GraphQL**
1. Selecione as consultas persistentes existentes à esquerda e execute-as para ver os resultados.

   >[!NOTE]
   >
   > A ferramenta GraphiQL e a API do GraphQL são [exploradas com mais detalhes posteriormente no tutorial](../multi-step/explore-graphql-api.md).

## Parabéns.{#congratulations}

Parabéns, agora você tem um aplicativo externo que consome conteúdo do AEM com o GraphQL. Fique à vontade para inspecionar o código no aplicativo React e continuar a experimentar a modificação de fragmentos de conteúdo existentes.

### Próximas etapas

* [Iniciar o tutorial do AEM Headless](../multi-step/overview.md)
