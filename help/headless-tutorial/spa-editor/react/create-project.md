---
title: Criar projeto | Introdução ao AEM SPA Editor e React
description: Saiba como gerar um projeto Maven do Adobe Experience Manager (AEM) como ponto de partida para um aplicativo React integrado ao AEM SPA Editor.
feature: SPA Editor, AEM Project Archetype
version: Experience Manager as a Cloud Service
jira: KT-413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 57c8fc16-fed5-4af4-b98b-5c3f0350b240
duration: 250
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '975'
ht-degree: 1%

---

# Criar projeto {#spa-editor-project}

{{spa-editor-deprecation}}

Saiba como gerar um projeto Maven do Adobe Experience Manager (AEM) como ponto de partida para um aplicativo React integrado ao AEM SPA Editor.

## Objetivo

1. Gere um projeto ativado do Editor SPA usando o Arquétipo de projeto do AEM.
2. Implante o projeto inicial em uma instância local do AEM.

## O que você vai criar {#what-build}

Neste capítulo, um novo projeto do AEM é gerado, com base no [Arquétipo de Projetos AEM](https://github.com/adobe/aem-project-archetype). O projeto do AEM é inicializado com um ponto de partida muito simples para o SPA do React.

**O que é um projeto Maven?** - [Apache Maven](https://maven.apache.org/) é uma ferramenta de gerenciamento de software para compilar projetos. *Todas as implementações do Adobe Experience Manager* usam projetos Maven para compilar, gerenciar e implantar código personalizado sobre o AEM.

**O que é um arquétipo Maven?** - Um [Arquétipo Maven](https://maven.apache.org/archetype/index.html) é um modelo ou padrão para gerar novos projetos. O arquétipo do Projeto AEM nos permite gerar um novo projeto com um namespace personalizado e incluir uma estrutura de projeto que segue as práticas recomendadas, acelerando consideravelmente nosso projeto.

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment). Verifique se uma nova instância do Adobe Experience Manager, iniciada no modo **author**, está em execução localmente.

## Criar o projeto {#create}

>[!NOTE]
>
>Este tutorial usa a versão **35** do arquétipo.

1. Abra um terminal de linha de comando e insira o seguinte comando Maven:

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=35 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Se estiver direcionado para o AEM 6.5.5+, substitua `aemVersion="cloud"` por `aemVersion="6.5.5"`. Se for para 6.4.8+, use `aemVersion="6.4.8"`.

   Observe a propriedade `frontendModule=react`. Isso instrui o Arquétipo de Projetos AEM a inicializar o projeto com uma [base de código React](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html?lang=pt-BR) inicial a ser usada com o Editor SPA do AEM. Propriedades como `appTitle`, `appId`, `artifactId` e `groupId` são usadas para identificar o projeto e a finalidade.

   Uma lista completa de propriedades disponíveis para configurar um projeto [pode ser encontrada aqui](https://github.com/adobe/aem-project-archetype#available-properties).

1. A seguinte pasta e estrutura de arquivo são geradas pelo arquétipo Maven no sistema de arquivos local:

   ```plain
   |--- aem-guides-wknd-spa.react/
       |--- LICENSE
       |--- README.md
       |--- all/
       |--- archetype.properties
       |--- core/
       |--- dispatcher/
       |--- it.tests/
       |--- pom.xml
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.config/
       |--- ui.content/
       |--- ui.frontend/
       |--- ui.tests /
       |--- .gitignore
   ```

   Cada pasta representa um módulo Maven individual. Neste tutorial, trabalharemos principalmente com o módulo `ui.frontend`, que é o aplicativo React. Mais detalhes sobre módulos individuais podem ser encontrados na [documentação do Arquétipo de Projetos AEM](https://experienceleague.adobe.com/pt-br/docs/experience-manager-core-components/using/developing/archetype/overview).

## Implantar e criar o projeto

Em seguida, compile, crie e implante o código do projeto em uma instância local do AEM usando o Maven.

1. Verifique se uma instância do AEM está sendo executada localmente na porta **4502**.
1. Na linha de comando, navegue até o diretório de projeto `aem-guides-wknd-spa.react`.

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. Execute o seguinte comando para criar e implantar o projeto inteiro no AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   A build levará cerca de um minuto e deve terminar com a seguinte mensagem:

   ```shell
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd-spa.react 1.0.0-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd-spa.react .......................... SUCCESS [  0.257 s]
   [INFO] WKND SPA React - Core .............................. SUCCESS [ 12.553 s]
   [INFO] WKND SPA React - UI Frontend ....................... SUCCESS [01:46 min]
   [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  1.082 s]
   [INFO] WKND SPA React - UI apps ........................... SUCCESS [  8.237 s]
   [INFO] WKND SPA React - UI content ........................ SUCCESS [  5.633 s]
   [INFO] WKND SPA React - UI config ......................... SUCCESS [  0.234 s]
   [INFO] WKND SPA React - All ............................... SUCCESS [  0.643 s]
   [INFO] WKND SPA React - Integration Tests ................. SUCCESS [ 12.377 s]
   [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.066 s]
   [INFO] WKND SPA React - UI Tests .......................... SUCCESS [  0.074 s]
   [INFO] WKND SPA React - Project Analyser .................. SUCCESS [ 31.287 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   O perfil Maven `autoInstallSinglePackage` compila os módulos individuais do projeto e implanta um único pacote na instância do AEM. Por padrão, esse pacote é implantado em uma instância do AEM em execução localmente na porta **4502** e com as credenciais de `admin:admin`.

1. Navegue até **Gerenciador de Pacotes** na sua instância do AEM local: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

1. Você deve ver vários pacotes com o prefixo `aem-guides-wknd-spa.react`.

   ![Pacotes de SPA do WKND](assets/create-project/package-manager.png)

   *Gerenciador de pacotes do AEM*

   Todo o código personalizado necessário para o projeto é incorporado a esses pacotes e instalado no ambiente do AEM.

## Conteúdo do autor

Em seguida, abra o SPA inicial gerado pelo arquétipo e atualize parte do conteúdo.

1. Navegue até o console **Sites**: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   O WKND SPA inclui uma estrutura básica do site com um país, idioma e página inicial. Esta hierarquia é baseada nos valores padrão do arquétipo para `language_country` e `isSingleCountryWebsite`. Estes valores podem ser substituídos atualizando as [propriedades disponíveis](https://github.com/adobe/aem-project-archetype#available-properties) ao gerar um projeto.

2. Abra a página **us** > **en** > **Página Inicial do WKND SPA React** selecionando a página e clicando no botão **Editar** na barra de menus:

   ![console do site](./assets/create-project/open-home-page.png)

3. Um componente **Texto** já foi adicionado à página. Você pode editar esse componente como qualquer outro componente no AEM.

   ![Atualizar componente de Texto](./assets/create-project/update-text-component.gif)

4. Adicionar um componente **Texto** adicional à página.

   Observe que a experiência de criação é semelhante àquela de uma página tradicional do AEM Sites. Atualmente, um número limitado de componentes está disponível para uso. Mais informações são adicionadas durante o curso do tutorial.

## Inspecionar o aplicativo de página única

Em seguida, verifique se este é um aplicativo de página única com o uso das ferramentas de desenvolvedor do seu navegador.

1. No **Editor de páginas**, clique no botão **Informações da Página** > **Exibir como Publicado**:

   ![Botão Exibir como Publicado](./assets/create-project/view-as-published.png)

   Isso abrirá uma nova guia com o parâmetro de consulta `?wcmmode=disabled` que desativa efetivamente o editor do AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. Exiba a fonte da página e observe que o conteúdo do texto **[!DNL Hello World]** ou qualquer outro conteúdo não foi encontrado. Em vez disso, você deve ver o HTML da seguinte maneira:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` é o SPA do React carregado na página e responsável pela renderização do conteúdo.

   No entanto, *de onde vem o conteúdo?*

3. Retorne à guia: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. Abra as ferramentas de desenvolvedor do navegador e inspecione o tráfego de rede da página durante uma atualização. Exibir as solicitações de **XHR**:

   ![Solicitações XHR](./assets/create-project/xhr-requests.png)

   Deve haver uma solicitação para [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Ele contém todo o conteúdo, formatado em JSON, que direcionará o SPA.

5. Em uma nova guia, abra [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   A solicitação `en.model.json` representa o modelo de conteúdo que direcionará o aplicativo. Inspecione a saída JSON e você poderá encontrar o trecho que representa o(s) componente(s) **[!UICONTROL Texto]**.

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
   }
   ...
   ```

   No próximo capítulo, verificaremos como esse conteúdo JSON é mapeado dos componentes do AEM para os componentes SPA, para formar a base da experiência do Editor SPA do AEM.

   >[!NOTE]
   >
   > Pode ser útil instalar uma extensão de navegador para formatar automaticamente a saída JSON.

## Parabéns! {#congratulations}

Parabéns, você acabou de criar seu primeiro projeto do Editor SPA do AEM.

O SPA é bastante simples. Nos próximos capítulos, mais funcionalidade será adicionada.

### Próximas etapas {#next-steps}

[Integrar um SPA](integrate-spa.md) - Saiba como o código-fonte do SPA é integrado ao Projeto do AEM e entenda as ferramentas disponíveis para desenvolver rapidamente o SPA.
