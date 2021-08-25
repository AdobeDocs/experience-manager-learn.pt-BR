---
title: Configuração rápida - Introdução ao AEM sem interface - GraphQL
description: Introdução à Adobe Experience Manager (AEM) e GraphQL. Instale o SDK do AEM, adicione conteúdo de amostra e implante um aplicativo que consuma conteúdo de AEM usando suas APIs GraphQL. Veja como o AEM alimenta as experiências omnichannel.
version: cloud-service
mini-toc-levels: 1
kt: 6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1814'
ht-degree: 2%

---


# Configuração rápida {#setup}

Este capítulo oferece uma configuração rápida de um ambiente local para ver o conteúdo de consumo de um aplicativo externo do AEM usando APIs GraphQL AEM. Os capítulos posteriores no tutorial sairão dessa configuração.

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## Objetivos {#objectives}

1. Baixe e instale o SDK do AEM.
1. Baixe e instale o conteúdo de amostra do site de referência WKND.
1. Baixe e instale um aplicativo de amostra para consumir conteúdo usando as APIs GraphQL.

## Instalar o SDK do AEM {#aem-sdk}

Este tutorial usa o [AEM como um SDK do Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en#aem-as-a-cloud-service-sdk) para explorar AEM APIs GraphQL. Esta seção fornece um guia rápido para instalar o SDK do AEM e executá-lo no modo Autor. Um guia mais detalhado para configurar um ambiente de desenvolvimento local [pode ser encontrado aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up).

>[!NOTE]
>
> Também é possível seguir o tutorial com um AEM como um ambiente Cloud Service. Observações adicionais sobre o uso de um ambiente do Cloud estão incluídas em todo o tutorial.

1. Navegue até o **[Portal de distribuição de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM como um Cloud Service** e baixe a versão mais recente do **AEM SDK**.

   ![Portal de distribuição de software](assets/setup/software-distribution-portal-download.png)

   >[!CAUTION]
   >
   > O recurso GraphQL é ativado por padrão somente no SDK do AEM de 2021-02-04 ou mais recente.

1. Descompacte o download e copie o jar do Quickstart (`aem-sdk-quickstart-XXX.jar`) em uma pasta dedicada, ou seja `~/aem-sdk/author`.
1. Renomeie o arquivo jar para `aem-author-p4502.jar`.

   O nome `author` especifica que o jar do Quickstart será iniciado no modo Autor. O `p4502` especifica que o servidor Quickstart será executado na porta 4502.

1. Abra uma nova janela do terminal e navegue até a pasta que contém o arquivo jar. Execute o seguinte comando para instalar e iniciar a instância do AEM:

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. Forneça uma senha de administrador como `admin`. Qualquer senha de administrador é aceitável, no entanto, sua recomendação é usar o padrão para desenvolvimento local para reduzir a necessidade de reconfigurar.
1. Após alguns minutos, a instância de AEM terminará a instalação e uma nova janela do navegador deverá abrir em [http://localhost:4502](http://localhost:4502).
1. Faça logon com o nome de usuário `admin` e senha `admin`.

## Instalar conteúdo de amostra e pontos de extremidade GraphQL {#wknd-site-content-endpoints}

O conteúdo de amostra do **site de referência WKND** será instalado para acelerar o tutorial. A WKND é uma marca fictícia ao estilo de vida, frequentemente usada em conjunto com AEM treinamento.

O site de referência WKND inclui configurações necessárias para expor um ponto de extremidade [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=en#graphql-aem-endpoint). Em uma implementação real, siga as etapas documentadas para [incluir os pontos de extremidade GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=en#graphql-aem-endpoint) no projeto do cliente. Um [CORS](#cors-config) também foi empacotado como parte do site WKND. Uma configuração do CORS é necessária para conceder acesso a um aplicativo externo. Mais informações sobre [CORS](#cors-config) podem ser encontradas abaixo.

1. Baixe o pacote de AEM compilado mais recente para o site WKND: [aem-guides-wknd.all-x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Faça o download da versão padrão compatível com o AEM como Cloud Service e **not** a versão `classic`.

1. No menu **AEM Iniciar**, navegue até **Ferramentas** > **Implantação** > **Pacotes**.

   ![Navegue até Pacotes](assets/setup/navigate-to-packages.png)

1. Clique em **Upload Package** e escolha o pacote WKND baixado na etapa anterior. Clique em **Install** para instalar o pacote.

1. No menu **AEM Iniciar**, navegue até **Ativos** > **Arquivos**.
1. Clique nas pastas para navegar até **Site WKND** > **Inglês** > **Aventuras**.

   ![Exibição de pasta de Aventuras](assets/setup/folder-view-adventures.png)

   Esta é uma pasta de todos os ativos que compõem as várias Aventuras promovidas pela marca WKND. Isso inclui tipos de mídia tradicionais, como imagens e vídeo, bem como mídia específica de AEM como **Fragmentos de conteúdo**.

1. Clique na pasta **Baixar esquiando Wyoming** e clique no cartão **Baixar esquiando o fragmento de conteúdo Wyoming**:

   ![Baixar o cartão do fragmento de conteúdo de esqui](assets/setup/downhill-skiing-cntent-fragment.png)

1. A interface do usuário do Editor de fragmento de conteúdo será aberta para a aventura de esqui de Baixo no Wyoming.

   ![Fragmento de conteúdo de esqui de preenchimento](assets/setup/down-hillskiing-fragment.png)

   Observe que vários campos como **Title**, **Descrição** e **Activity** definem o fragmento.

   **Os** Fragmentos de conteúdo são uma das maneiras de gerenciar o conteúdo no AEM. Os Fragmentos de conteúdo são conteúdo reutilizável e agnóstico de apresentação composto de elementos de dados estruturados, como texto, rich text, datas ou referências a outros Fragmentos de conteúdo. Os Fragmentos de conteúdo serão explorados com mais detalhes posteriormente no tutorial.

1. Clique em **Cancelar** para fechar o fragmento. Você pode navegar em algumas das outras pastas e explorar o outro conteúdo da Adventure.

>[!NOTE]
>
> Se estiver usando um ambiente Cloud Service, consulte a documentação de como [implantar uma base de código como o site de Referência WKND em um ambiente Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=en#deploying).

## Instalar o aplicativo de amostra{#sample-app}

Um dos objetivos deste tutorial é mostrar como consumir conteúdo AEM de um aplicativo externo usando APIs GraphQL. Este tutorial usa um exemplo de aplicativo React que foi parcialmente concluído para acelerar o tutorial. As mesmas lições e conceitos se aplicam a aplicativos criados com iOS, Android ou qualquer outra plataforma. O aplicativo React é intencionalmente simples, para evitar complexidade desnecessária; não se trata de uma implementação de referência.

1. Abra uma nova janela de terminal e clone a ramificação inicial do tutorial usando o Git:

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. No IDE de sua escolha, abra o arquivo `.env.development` em `aem-guides-wknd-graphql/react-app/.env.development`. Verifique se a linha `REACT_APP_AUTHORIZATION` não está comentada e se o arquivo tem a seguinte aparência:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   Certifique-se de que `React_APP_HOST_URI` corresponda à instância de AEM local. Neste capítulo, conectaremos o aplicativo React diretamente ao ambiente AEM **Author**. **** Por padrão, os ambientes de autoria exigem autenticação, portanto, nosso aplicativo se conectará como o  `admin` usuário. Essa é uma prática comum durante o desenvolvimento, pois permite que façamos rapidamente alterações no ambiente de AEM e as vejamos imediatamente refletidas no aplicativo.

   >[!NOTE]
   >
   > Em um cenário de produção, o aplicativo se conectará a um ambiente de **Publicação** AEM. Isso é abordado com mais detalhes no capítulo [Implantação de produção](production-deployment.md).

1. Navegue até a pasta `aem-guides-wknd-graphql/react-app`. Instale e inicie o aplicativo:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Uma nova janela do navegador deve iniciar automaticamente o aplicativo em [http://localhost:3000](http://localhost:3000).

   ![React starter app](assets/setup/react-starter-app.png)

   Uma lista do conteúdo Aventura atual do AEM deve ser exibida.

1. Clique em uma das imagens da aventura para ver os detalhes da aventura. É feito um pedido para AEM retornar os detalhes de uma aventura.

   ![Exibição de Detalhes da Aventura](assets/setup/adventure-details-view.png)

1. Use as ferramentas de desenvolvedor do navegador para inspecionar as solicitações de **Rede**. Visualize as solicitações **XHR** e observe várias solicitações de POST para `/content/graphql/global/endpoint.json`, o ponto de extremidade GraphQL configurado para AEM.

   ![Solicitação GraphQL Endpoint XHR](assets/setup/endpoint-gql.png)

1. Você também pode visualizar os parâmetros e a resposta JSON inspecionando a solicitação de rede. Pode ser útil instalar uma extensão de navegador como [GraphQL Network Inspetor](https://chrome.google.com/webstore/detail/graphql-network-inspector/ndlbedplllcgconngcnfmkadhokfaaln) para Chrome para obter uma melhor compreensão da consulta e da resposta.

## Modificar um fragmento do conteúdo

Agora que o aplicativo React está em execução, faça uma atualização do conteúdo no AEM e veja a alteração refletida no aplicativo.

1. Navegue até AEM [http://localhost:4502](http://localhost:4502).
1. Navegue até **Assets** > **Arquivos** > **Site WKND** > **Inglês** > **Aventuras** > **[Campo de Surf Bali](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**.

   ![Pasta Bali Surf Camp](assets/setup/bali-surf-camp-folder.png)

1. Clique no fragmento de conteúdo **Campo de navegação de bali** para abrir o Editor de fragmento de conteúdo.
1. Modifique o **Título** e o **Descrição** da aventura

   ![Modificar fragmento de conteúdo](assets/setup/modify-content-fragment-bali.png)

1. Clique em **Save** para salvar as alterações.
1. Navegue de volta ao aplicativo React em [http://localhost:3000](http://localhost:3000) e atualize para ver suas alterações:

   ![Aventura Bali Surf Camp Atualizado](assets/setup/overnight-bali-surf-camp-changes.png)

## Instalação da ferramenta GraphiQL {#install-graphiql}

[](https://github.com/graphql/graphiql) O GraphiQL é uma ferramenta de desenvolvimento e é necessária somente em ambientes de nível inferior, como uma instância de desenvolvimento ou local. O GraphiQL IDE permite testar e refinar rapidamente as consultas e os dados retornados. O GraphiQL também oferece acesso fácil à documentação, facilitando o aprendizado e a compreensão de quais métodos estão disponíveis.

1. Navegue até o **[Portal de distribuição de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM como um Cloud Service**.
1. Procure por &quot;GraphiQL&quot; (certifique-se de incluir o **i** em **GraphiQL**.
1. Baixe o **Pacote de Conteúdo GraphiQL v.x.x** mais recente

   ![Download do pacote GraphiQL](assets/explore-graphql-api/software-distribution.png)

   O arquivo zip é um pacote AEM que pode ser instalado diretamente.

1. No menu **AEM Iniciar**, navegue até **Ferramentas** > **Implantação** > **Pacotes**.
1. Clique em **Upload Package** e escolha o pacote baixado na etapa anterior. Clique em **Install** para instalar o pacote.

   ![Instalar o pacote GraphiQL](assets/explore-graphql-api/install-graphiql-package.png)
1. Navegue até GraphiQL IDE em [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html) e comece a explorar as APIs GraphQL.

   >[!NOTE]
   >
   > A ferramenta GraphiQL e a API GraphQL são [exploradas com mais detalhes posteriormente no tutorial](./explore-graphql-api.md).

## Parabéns! {#congratulations}

Parabéns, agora você tem um aplicativo externo consumindo conteúdo AEM com GraphQL. Inspecione o código no aplicativo React e continue a testar a modificação dos Fragmentos de conteúdo existentes.

## Próximas etapas {#next-steps}

No próximo capítulo, [Definição de modelos de fragmento de conteúdo](content-fragment-models.md), saiba como modelar o conteúdo e criar um esquema com **Modelos de fragmento de conteúdo**. Você verificará os modelos existentes e criará um novo modelo. Você também aprenderá sobre os diferentes tipos de dados que podem ser usados para definir um schema como parte do modelo.

## (Bônus) Configuração do CORS {#cors-config}

AEM, por padrão, bloqueia solicitações entre origens, impedindo que aplicativos não autorizados se conectem e surjam seu conteúdo.

Para permitir que o aplicativo React deste tutorial interaja com AEM pontos de extremidade da API GraphQL, uma configuração de compartilhamento de recursos entre origens foi definida no projeto de referência do site WKND.

![Configuração de compartilhamento de recursos entre origens](assets/setup/cross-origin-resource-sharing-configuration.png)

Para exibir a configuração implantada:

1. Navegue até AEM console da Web do SDK em [http://localhost:4502/system/console](http://localhost:4502/system/console).

   >[!NOTE]
   >
   > O Console da Web só está disponível no SDK. Em um ambiente AEM como Cloud Service, essas informações podem ser visualizadas por meio do [Console do desenvolvedor](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html).

1. No menu superior, clique em **OSGI** > **Configuração** para exibir todas as [Configurações OSGi](http://localhost:4502/system/console/configMgr).
1. Role a página **Compartilhamento de recursos entre origens do Adobe Granite** para baixo.
1. Clique na configuração de `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.
1. Os seguintes campos foram atualizados:
   * Origens permitidas (Regex): `http://localhost:.*`
      * Permite todas as conexões de host local.
   * Caminhos permitidos: `/content/graphql/global/endpoint.json`
      * Este é o único ponto de extremidade GraphQL configurado atualmente. Como prática recomendada, as configurações de CORs devem ser o mais restritivas possível.
   * Métodos permitidos: `GET`, `HEAD`, `POST`
      * Somente `POST` é necessário para GraphQL, no entanto, os outros métodos podem ser úteis ao interagir com AEM sem interface.
   * Cabeçalhos suportados: **authorization** foi adicionada para transmitir a autenticação básica no ambiente do autor.
   * Suporta Credenciais: `Yes`
      * Isso é necessário, pois o aplicativo React se comunicará com os pontos finais GraphQL protegidos no serviço de autor do AEM.

Essa configuração e os pontos de extremidade GraphQL fazem parte do projeto WKND AEM. Você pode visualizar todas as configurações de [OSGi aqui](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig).
