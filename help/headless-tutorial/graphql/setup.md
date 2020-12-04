---
title: Configuração rápida - Introdução ao AEM sem cabeçalho - GraphQL
description: Introdução ao Adobe Experience Manager (AEM) e ao GraphQL. Instale o SDK AEM, adicione conteúdo de amostra e implante um aplicativo que consuma conteúdo de AEM usando suas APIs GraphQL. Veja como AEM as experiências oncológicas de canal.
sub-product: sites
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: null
thumbnail: null
translation-type: tm+mt
source-git-commit: 2e0352b051bf25a491b67468a76522084e53a71f
workflow-type: tm+mt
source-wordcount: '1553'
ht-degree: 2%

---


# Configuração rápida {#setup}

>[!CAUTION]
>
> A API AEM GraphQL para o Delivery de fragmento de conteúdo será lançada no início de 2021.
> A documentação relacionada está disponível para fins de pré-visualização.

Este capítulo oferta uma configuração rápida de um ambiente local para ver um aplicativo externo consumir conteúdo de AEM usando AEM APIs GraphQL. Os capítulos posteriores do tutorial serão desenvolvidos a partir dessa configuração.

## Pré-requisitos {#prerequisites}

As seguintes ferramentas devem ser instaladas localmente:

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=lista&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## Objetivos {#objectives}

1. Baixe e instale o SDK AEM.
1. Baixe e instale o conteúdo de amostra do site de referência WKND.
1. Baixe e instale um aplicativo de amostra para consumir conteúdo usando as APIs GraphQL.

## Instale o SDK AEM{#aem-sdk}

Este tutorial usa o AEM [como Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en#aem-as-a-cloud-service-sdk) para explorar AEM APIs GraphQL. Esta seção fornece um guia rápido para instalar o SDK AEM e executá-lo no modo Autor. Um guia mais detalhado para configurar um ambiente de desenvolvimento local [pode ser encontrado aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up).

>[!NOTE]
>
> Também é possível seguir o tutorial com um AEM como ambiente Cloud Service. Notas adicionais para usar um ambiente da Cloud estão incluídas no tutorial.

1. Navegue até **[Portal de distribuição de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM como Cloud Service** e faça download da versão mais recente do **AEM SDK**.

   ![Portal de distribuição de software](assets/setup/software-distribution-portal-download.png)

1. Descompacte o download e copie o jar do Quickstart (`aem-sdk-quickstart-XXX.jar`) para uma pasta dedicada, ou seja, `~/aem-sdk/author`.
1. Nomeie novamente o arquivo jar como `aem-author-p4502.jar`.
1. Abra uma nova janela de terminal e navegue até a pasta que contém o arquivo jar. Execute o seguinte comando para instalar e start a instância AEM:

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. Forneça uma senha de administrador como `admin`. Qualquer senha de administrador é aceitável, no entanto, é recomendável usar o padrão para desenvolvimento local para reduzir a necessidade de reconfiguração.
1. Após alguns minutos, a instância AEM terminará a instalação e uma nova janela do navegador deverá ser aberta em [http://localhost:4502](http://localhost:4502).
1. Faça logon com o nome de usuário `admin` e a senha `admin`.

## Instalar conteúdo de amostra{#wknd-site}

O conteúdo de amostra do site de referência WKND será instalado para acelerar o tutorial. A WKND é uma marca fictícia ao estilo de vida, muitas vezes usada em conjunto com o treinamento AEM.

1. Baixe o pacote de AEM compilado mais recente para o site WKND: [aem-guides-wknd.all-x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Certifique-se de baixar a versão padrão compatível com AEM como Cloud Service e **not** a versão `classic`.

1. No menu **AEM Start**, navegue até **Ferramentas** > **Implantação** > **Pacotes**.

   ![Navegue até Pacotes](assets/setup/navigate-to-packages.png)

1. Clique em **Carregar pacote** e escolha o pacote WKND baixado na etapa anterior. Clique em **Instalar** para instalar o pacote.

1. No menu **AEM Start**, navegue até **Assets** > **Files**.
1. Clique nas pastas para navegar até **Site WKND** > **Inglês** > **Aventuras**.

   ![Visualização de Pastas de Aventuras](assets/setup/folder-view-adventures.png)

   Esta é uma pasta de todos os ativos que compõem as várias Aventuras promovidas pela marca WKND. Isso inclui tipos de mídia tradicionais, como imagens e vídeos, bem como mídia específica para AEM, como **Fragmentos de conteúdo**.

1. Clique na pasta **Baixar esquiando Wyoming** e clique no cartão **Baixar o Wyoming Content Fragment**:

   ![Cartão de fragmento do conteúdo de esqui em andamento](assets/setup/downhill-skiing-cntent-fragment.png)

1. A interface do usuário do Editor de fragmentos de conteúdo será aberta para a aventura do Wyoming de esqui de baixo.

   ![Reduzir o fragmento do conteúdo de esqui](assets/setup/down-hillskiing-fragment.png)

   Observe que vários campos, como **Title**, **Descrição** e **Atividade** definem o fragmento.

   **Fragmentos de conteúdo** são uma das maneiras pelas quais o conteúdo pode ser gerenciado em AEM. Fragmento de conteúdo são conteúdo reutilizável e agnóstico de apresentação composto de elementos de dados estruturados, como texto, rich text, datas ou referências a outros Fragmentos de conteúdo. Fragmentos de conteúdo serão explorados com mais detalhes posteriormente no tutorial.

1. Clique em **Cancelar** para fechar o fragmento. Sinta-se à vontade para navegar em algumas das outras pastas e explorar o outro conteúdo da Adobe.

>[!NOTE]
>
> Se estiver usando um ambiente Cloud Service, consulte a documentação para ver como [implantar uma base de código como o site de referência WKND para um ambiente Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=en#deploying).

## Instale os Endpoints do GraphQL{#graphql-endpoint}

É necessário configurar pontos finais GraphQL. Isso proporciona flexibilidade ao projeto para determinar o endpoint exato em que a API do GraphQL é exposta. Um [CORS](#cors-config) também é necessário para conceder acesso a um aplicativo externo. Para agilizar o tutorial, um pacote foi pré-criado.

1. Baixe o pacote [aem-guides-wknd-grafql.all-1.0.0-SNAPSHOT.zip](./assets/setup/aem-guides-wknd-graphql.all-1.0.0-SNAPSHOT.zip).
1. No menu **AEM Start**, navegue até **Ferramentas** > **Implantação** > **Pacotes**.
1. Clique em **Carregar pacote** e escolha o pacote baixado na etapa anterior. Clique em **Instalar** para instalar o pacote.

O pacote acima também contém a [ferramenta GraphiQL](https://github.com/graphql/graphiql) que será usada em capítulos posteriores. Mais informações sobre a configuração do CORS podem ser [encontradas abaixo](#cors-config).

## Instale o aplicativo de amostra{#sample-app}

Um dos objetivos deste tutorial é mostrar como consumir AEM conteúdo de um aplicativo externo usando as APIs GraphQL. Este tutorial usa um exemplo de aplicativo React que foi parcialmente concluído para acelerar o tutorial. As mesmas lições e conceitos se aplicam a aplicativos criados com iOS, Android ou qualquer outra plataforma. O aplicativo React é intencionalmente simples, para evitar complexidade desnecessária. não se trata de uma implementação de referência.

1. Abra uma nova janela do terminal e clone a ramificação de inicialização do tutorial usando Git:

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. No IDE de sua escolha, abra o arquivo `.env.development` em `aem-guides-wknd-graphql/react-app/.env.development`. Exclua as barras de comentário da linha `REACT_APP_AUTHORIZATION` para que o arquivo tenha a seguinte aparência:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/endpoint.gql
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   Certifique-se de que `React_APP_HOST_URI` corresponda à sua instância AEM local. Neste capítulo, conectaremos o aplicativo React diretamente ao ambiente AEM **Author** e, portanto, precisaremos autenticar. Essa é uma prática comum durante o desenvolvimento, pois permite que façamos rapidamente alterações no ambiente AEM e as vejamos imediatamente refletidas no aplicativo.

   >[!NOTE]
   >
   > Em um cenário de produção, o aplicativo se conectará a um ambiente AEM **Publish**. Isso é abordado com mais detalhes, posteriormente no tutorial.

1. Navegue até a pasta `aem-guides-wknd-graphql/react-app`. Instale e start o aplicativo:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Uma nova janela do navegador deve iniciar automaticamente o aplicativo em [http://localhost:3000](http://localhost:3000).

   ![React starter app](assets/setup/react-starter-app.png)

   Uma lista do conteúdo atual da Aventura do AEM deve ser exibida.

1. Clique em uma das imagens de aventura para visualização dos detalhes da aventura. É feito um pedido para AEM retornar os detalhes de uma aventura.

   ![Visualização Detalhes da Aventura](assets/setup/adventure-details-view.png)

1. Use as ferramentas do desenvolvedor do navegador para inspecionar as solicitações **Network**. Visualização as solicitações **XHR** e observe várias solicitações de POST para `/content/graphql/endpoint.gql`, o ponto de extremidade GraphQL configurado para AEM.

   ![Solicitação XHR do Ponto Final GraphQL](assets/setup/endpoint-gql.png)

1. Você também pode visualização os parâmetros e a resposta JSON inspecionando a solicitação de rede. Pode ser útil instalar uma extensão do navegador como [GraphQL Network](https://chrome.google.com/webstore/detail/graphql-network/igbmhmnkobkjalekgiehijefpkdemocm) para o Chrome para obter uma melhor compreensão do query e da resposta.

   ![Extensão de Rede GraphQL](assets/setup/GraphQL-extension.png)

   *Usando a extensão Chrome GraphQL Network*

## Modificar um fragmento do conteúdo

Agora que o aplicativo React está em execução, atualize o conteúdo no AEM e veja a alteração refletida no aplicativo.

1. Navegue até AEM [http://localhost:4502](http://localhost:4502).
1. Navegue até **Assets** > **Arquivos** > **Site WKND** > **Inglês** > **Aventuras** > **[Campo de Surfe de Bali](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**.

   ![Pasta Bali Surf Camp](assets/setup/bali-surf-camp-folder.png)

1. Clique no fragmento de conteúdo **Bali Surf Camp** para abrir o Editor de fragmentos de conteúdo.
1. Modifique **Title** e **Description** da aventura

   ![Modificar fragmento de conteúdo](assets/setup/modify-content-fragment-bali.png)

1. Clique em **Salvar** para salvar as alterações.
1. Navegue até o aplicativo React em [http://localhost:3000](http://localhost:3000) e atualize para ver suas alterações:

   ![Aventura Bali Surf Camp Atualizada](assets/setup/overnight-bali-surf-camp-changes.png)

## Parabéns! {#congratulations}

Parabéns, agora você tem um aplicativo externo que consome AEM conteúdo com o GraphQL. Sinta-se à vontade para inspecionar o código no aplicativo React e continuar a experimentar a modificação de fragmentos de conteúdo existentes.

## Próximas etapas {#next-steps}

No próximo capítulo, [Definindo modelos de fragmento de conteúdo](content-fragment-models.md), saiba como modelar o conteúdo e criar um schema com **Modelos de fragmento de conteúdo**. Você revisará os modelos existentes e criará um novo modelo. Você também aprenderá sobre os diferentes tipos de dados que podem ser usados para definir um schema como parte do modelo.

## (Bônus) Configuração do CORS {#cors-config}

AEM, por padrão, bloqueia solicitações de origem cruzada, impedindo que aplicativos não autorizados se conectem e surjam seu conteúdo.

Para permitir que o aplicativo React deste tutorial interaja com AEM endpoints da API GraphQL, uma configuração de compartilhamento de recursos entre origens foi definida no pacote de endpoints GraphQL.

![Configuração de compartilhamento de recursos entre Origens](./assets/setup/cross-origin-resource-sharing-configuration.png)

Para configurar isso manualmente:

1. Navegue até AEM console Web do SDK em **Ferramentas** > **Operações** > **Console Web**
1. Clique na linha **Adobe Granite Cross-Origem Resource Sharing Policy** para criar uma nova configuração
1. Atualize os seguintes campos, deixando os outros com seus valores padrão:
   * Origens permitidas: `localhost:3000`
   * Origens permitidas (Regex): `.* `
   * Caminhos permitidos: `/content/graphql/endpoint.gql`
   * Métodos permitidos: `GET`, `HEAD`, `POST`
      * Somente `POST` é necessário para o GraphQL, no entanto, os outros métodos podem ser úteis ao interagir com o AEM de forma imutável.
   * Suporta Credenciais: `Yes`
      * Isso é necessário, pois nosso aplicativo React se comunicará com os pontos finais GraphQL protegidos no serviço de autor de AEM.
1. Clique em **Salvar**

Essa configuração permite `POST` solicitações HTTP originárias de `localhost:3000` para o serviço de autor de AEM no caminho `/content/graphql/endpoint.gql`.

Essa configuração e os pontos finais GraphQL são gerados a partir de um projeto AEM. [Visualização os detalhes aqui](https://github.com/adobe/aem-guides-wknd-graphql/tree/master/aem-project).
