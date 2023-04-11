---
title: AEM Configuração rápida sem cabeçalho usando o SDK AEM local
description: Introdução ao Adobe Experience Manager (AEM) e GraphQL. Instale o SDK do AEM, adicione conteúdo de amostra e implante um aplicativo que consuma conteúdo de AEM usando suas APIs do GraphQL. Veja como o AEM alimenta as experiências omnichannel.
version: Cloud Service
mini-toc-levels: 1
kt: 6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d2da6efa-1f77-4391-adda-e3180c42addc
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '1257'
ht-degree: 2%

---

# AEM Configuração rápida sem cabeçalho usando o SDK AEM local {#setup}

A configuração rápida do AEM Headless leva você com AEM Headless usando o conteúdo do projeto de amostra do WKND Site e uma amostra do React App (a SPA) que consome o conteúdo por meio AEM APIs Headless GraphQL. Este guia usa o [AEM SDK as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html).

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v18](https://nodejs.org/en/)
* [Git](https://git-scm.com/)

## 1. Instalar o SDK do AEM {#aem-sdk}

Essa configuração usa o [AEM SDK as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?#aem-as-a-cloud-service-sdk) para explorar AEM APIs do GraphQL. Esta seção fornece um guia rápido para instalar o SDK do AEM e executá-lo no modo Autor. Um guia mais detalhado para configurar um ambiente de desenvolvimento local [pode ser encontrada aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html#local-development-environment-set-up).

>[!NOTE]
>
> Também é possível seguir o tutorial com um [AEM ambiente as a Cloud Service](./cloud-service.md). Observações adicionais sobre o uso de um ambiente do Cloud estão incluídas em todo o tutorial.

1. Navegue até o **[Portal de distribuição de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)** > **AEM as a Cloud Service** e baixe a versão mais recente do **AEM SDK**.

   ![Portal de distribuição de software](assets/quick-setup/aem-sdk/downloads__aem-sdk.png)

1. Descompacte o download e copie o jar do Quickstart (`aem-sdk-quickstart-XXX.jar`) em uma pasta dedicada, ou seja, `~/aem-sdk/author`.
1. Renomeie o arquivo jar para `aem-author-p4502.jar`.

   O `author` name especifica que o jar do Quickstart começa no modo Autor. O `p4502` especifica a execução do Quickstart na porta 4502.

1. Para instalar e iniciar a instância do AEM, abra um prompt de comando na pasta que contém o arquivo jar e execute o seguinte comando:

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. Forneça uma senha de administrador como `admin`. Qualquer senha de administrador é aceitável, no entanto, é recomendável usar `admin` para desenvolvimento local para reduzir a necessidade de reconfigurar.
1. Quando o serviço de AEM terminar de instalar, uma nova janela do navegador deverá abrir em [http://localhost:4502](http://localhost:4502).
1. Faça logon com o nome de usuário `admin` e a senha selecionada durante AEM primeira inicialização (normalmente `admin`).

## 2. Instalar conteúdo de amostra {#install-sample-content}

Conteúdo de amostra do **Site de referência WKND** é usada para acelerar o tutorial. A WKND é uma marca fictícia de estilo de vida, frequentemente usada com treinamento AEM.

O site WKND inclui configurações necessárias para expor um [Ponto de extremidade GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html). Em uma implementação real, siga as etapas documentadas para [incluir os endpoints do GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html) no projeto do cliente. A [CORS](#cors-config) também foi embalada como parte do site WKND. Uma configuração do CORS é necessária para conceder acesso a um aplicativo externo, para obter mais informações sobre [CORS](#cors-config) pode ser encontrada abaixo.

1. Baixe o pacote de AEM compilado mais recente para o site WKND: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Faça o download da versão padrão compatível com AEM as a Cloud Service e **not** o `classic` versão.

1. No **Início do AEM** , navegue até **Ferramentas** > **Implantação** > **Pacotes**.

   ![Navegue até Pacotes](assets/quick-setup/aem-sdk/aem-sdk__packages.png)

1. Clique em **Fazer upload do pacote** e escolha o pacote WKND baixado na etapa anterior. Clique em **Instalar** para instalar o pacote.

1. No **Início do AEM** , navegue até **Ativos** > **Arquivos** > **WKND Compartilhado** > **Inglês** > **Aventuras**.

   ![Exibição de pasta de Aventuras](assets/quick-setup/aem-sdk/aem-sdk__assets-folder.png)

   Esta é uma pasta de todos os ativos que compõem as várias Aventuras promovidas pela marca WKND. Isso inclui tipos de mídia tradicionais, como imagens e vídeo, além de mídia específica de AEM como **Fragmentos de conteúdo**.

1. Clique no botão **Descarga do Skiing Wyoming** e clique no botão **Fragmento de conteúdo de esqui descendente do Wyoming** cartão:

   ![Cartão de fragmento de conteúdo](assets/quick-setup/aem-sdk/aem-sdk__content-fragment.png)

1. O editor de Fragmento de conteúdo é aberto para a aventura de Baixo Skiing Wyoming.

   ![Editor de fragmento de conteúdo](assets/quick-setup/aem-sdk/aem-sdk__content-fragment-editor.png)

   Observe que vários campos como **Título**, **Descrição** e **Atividade** defina o fragmento.

   **Fragmentos de conteúdo** são uma das maneiras de gerenciar o conteúdo no AEM. Os Fragmentos de conteúdo são conteúdo reutilizável e independente da apresentação, composto de elementos de dados estruturados, como texto, rich text, datas ou referências a outros Fragmentos de conteúdo. Os Fragmentos de conteúdo são explorados com mais detalhes posteriormente na configuração rápida.

1. Clique em **Cancelar** para fechar o fragmento. Você pode navegar em algumas das outras pastas e explorar o outro conteúdo da Adventure.

>[!NOTE]
>
> Se estiver usando um ambiente de Cloud Service, consulte a documentação para saber como [implante uma base de código como o site de referência WKND em um ambiente Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version).

## 3. Baixe e execute o aplicativo WKND React {#sample-app}

Um dos objetivos deste tutorial é mostrar como consumir conteúdo AEM de um aplicativo externo usando as APIs do GraphQL. Este tutorial usa um exemplo de Aplicativo React. O aplicativo React é intencionalmente simples, para se concentrar na integração com AEM APIs GraphQL.

1. Abra um novo prompt de comando e clone o aplicativo React de amostra do GitHub:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Abra o aplicativo React em `aem-guides-wknd-graphql/react-app` no IDE de sua escolha.
1. No IDE, abra o arquivo `.env.development` at `/.env.development`. Verifique o `REACT_APP_AUTHORIZATION` for descomentada e o arquivo declarar as seguintes variáveis:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   Garantir `REACT_APP_HOST_URI` aponta para o SDK do AEM local. Para maior comodidade, esta inicialização rápida conecta o aplicativo React ao  **Autor do AEM**. **Autor** os serviços exigem autenticação, portanto, o aplicativo usa a variável `admin` para estabelecer sua conexão. A conexão de um aplicativo ao autor do AEM é uma prática comum durante o desenvolvimento, pois facilita a iteração rápida do conteúdo sem a necessidade de publicar alterações.

   >[!NOTE]
   >
   > Em um cenário de produção, o aplicativo se conectará a um AEM **Publicar** ambiente. Isso é abordado com mais detalhes na seção _Implantação de produção_ seção.


1. Instale e inicie o aplicativo React:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Uma nova janela do navegador abre automaticamente o aplicativo em [http://localhost:3000](http://localhost:3000).

   ![React starter app](assets/quick-setup/aem-sdk/react-app__home-view.png)

   Uma lista do conteúdo da aventura do AEM é exibida.

1. Clique em uma das imagens da aventura para ver os detalhes da aventura. É feito um pedido para AEM retornar os detalhes de uma aventura.

   ![Exibição de Detalhes da Aventura](assets/quick-setup/aem-sdk/react-app__adventure-view.png)

1. Use as ferramentas do desenvolvedor do navegador para inspecionar a **Rede** solicitações. Visualize o **XHR** e observar várias solicitações do GET para `/graphql/execute.json/...`. Esse prefixo de caminho chama AEM ponto de extremidade de consulta persistente, selecionando a consulta persistente a ser executada usando o nome e os parâmetros codificados após o prefixo .

   ![Solicitação XHR do GraphQL Endpoint](assets/quick-setup/aem-sdk/react-app__graphql-request.png)

## 4. Editar conteúdo no AEM

Com o aplicativo React em execução, faça uma atualização do conteúdo no AEM e veja se a alteração foi refletida no aplicativo.

1. Navegar para AEM [http://localhost:4502](http://localhost:4502).
1. Navegar para **Ativos** > **Arquivos** > **WKND Compartilhado** > **Inglês** > **Aventuras** > **[Campo de Surf de Bali](http://localhost:4502/assets.html/content/dam/wknd-shared/en/adventures/bali-surf-camp)**.

   ![Pasta Bali Surf Camp](assets/setup/bali-surf-camp-folder.png)

1. Clique no botão **Campo de Surf de Bali** fragmento de conteúdo para abrir o editor Fragmento de conteúdo .
1. Modifique o **Título** e **Descrição** da aventura.

   ![Modificar fragmento de conteúdo](assets/setup/modify-content-fragment-bali.png)

1. Clique em **Salvar** para salvar as alterações.
1. Atualize o aplicativo React em [http://localhost:3000](http://localhost:3000) para ver suas alterações:

   ![Aventura Bali Surf Camp Atualizado](assets/setup/overnight-bali-surf-camp-changes.png)

## 5. Explore o GraphiQL {#graphiql}

1. Abrir [GraphiQL](http://localhost:4502/aem/graphiql.html) navegando até **Ferramentas** > **Geral** > **Editor de consultas do GraphQL**
1. Selecione consultas persistentes existentes à esquerda e execute-as para ver os resultados.

   >[!NOTE]
   >
   > A ferramenta GraphiQL e a API do GraphQL são [explorado com mais detalhes posteriormente no tutorial](../multi-step/explore-graphql-api.md).

## Parabéns!{#congratulations}

Parabéns, agora você tem um aplicativo externo consumindo conteúdo AEM com o GraphQL. Inspecione o código no aplicativo React e continue a testar a modificação dos Fragmentos de conteúdo existentes.

### Próximas etapas

* [Inicie o tutorial AEM Headless](../multi-step/overview.md)
