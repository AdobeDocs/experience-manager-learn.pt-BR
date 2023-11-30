---
title: Criar projeto AEM | Introdução ao SPA Editor e React
description: Saiba como gerar um projeto Maven do Adobe Experience Manager (AEM) como ponto de partida para um aplicativo React integrado ao editor do AEM SPA.
feature: SPA Editor, AEM Project Archetype
version: Cloud Service
jira: KT-413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 57c8fc16-fed5-4af4-b98b-5c3f0350b240
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1063'
ht-degree: 2%

---

# Criar projeto {#spa-editor-project}

Saiba como gerar um projeto Maven do Adobe Experience Manager (AEM) como ponto de partida para um aplicativo React integrado ao editor do AEM SPA.

## Objetivo

1. Gere um projeto habilitado para o Editor de SPA usando o Arquétipo de projeto AEM.
2. Implante o projeto inicial em uma instância local do AEM.

## O que você vai criar {#what-build}

Neste capítulo, é gerado um novo projeto de AEM, com base no [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype). O projeto AEM é inicializado com um ponto de partida muito simples para o SPA React.

**O que é um projeto Maven?** - [Apache Maven](https://maven.apache.org/) O é uma ferramenta de gerenciamento de software para criar projetos. *Todos os Adobe Experience Manager* As implementações do usam projetos Maven para criar, gerenciar e implantar código personalizado sobre o AEM.

**O que é um arquétipo Maven?** - A [Arquétipo Maven](https://maven.apache.org/archetype/index.html) é um modelo ou padrão para gerar novos projetos. O arquétipo do Projeto AEM nos permite gerar um novo projeto com um namespace personalizado e incluir uma estrutura de projeto que siga as práticas recomendadas, acelerando consideravelmente nosso projeto.

## Pré-requisitos

Analisar as ferramentas e instruções necessárias para a configuração de um [ambiente de desenvolvimento local](overview.md#local-dev-environment). Verifique se uma nova instância do Adobe Experience Manager, iniciada em **autor** está sendo executado localmente.

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
   > Se estiver direcionando para AEM 6.5.5+, substituir `aemVersion="cloud"` com `aemVersion="6.5.5"`. Se estiver direcionando para 6.4.8+, use `aemVersion="6.4.8"`.

   Observe a `frontendModule=react` propriedade. Isso instrui o Arquétipo de projeto AEM a inicializar o projeto com um iniciador [Base de código do React](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) para ser usado com o editor SPA AEM. Propriedades como `appTitle`, `appId`, `artifactId`, e `groupId` são usados para identificar o projeto e a finalidade.

   Uma lista completa de propriedades disponíveis para configurar um projeto [pode ser encontrado aqui](https://github.com/adobe/aem-project-archetype#available-properties).

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

   Cada pasta representa um módulo Maven individual. Neste tutorial, trabalharemos principalmente com a `ui.frontend` que é o aplicativo React. Mais detalhes sobre módulos individuais podem ser encontrados na [Documentação do Arquétipo de projeto do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=pt-BR).

## Implantar e criar o projeto

Em seguida, compile, crie e implante o código do projeto em uma instância local do AEM usando Maven.

1. Verifique se uma instância do AEM está sendo executada localmente na porta **4502**.
1. Na linha de comando, navegue até o `aem-guides-wknd-spa.react` diretório do projeto.

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

   O perfil Maven `autoInstallSinglePackage` compila os módulos individuais do projeto e implanta um único pacote na instância do AEM. Por padrão, esse pacote é implantado em uma instância AEM executada localmente na porta **4502** e com as credenciais de `admin:admin`.

1. Navegue até **Gerenciador de pacotes** na sua instância local do AEM: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

1. Você deve ver vários pacotes com o prefixo `aem-guides-wknd-spa.react`.

   ![Pacotes SPA WKND](assets/create-project/package-manager.png)

   *Gerenciador de pacotes AEM*

   Todo o código personalizado necessário para o projeto está incluído nesses pacotes e instalado no ambiente AEM.

## Conteúdo do autor

Em seguida, abra o SPA inicial gerado pelo arquétipo e atualize parte do conteúdo.

1. Navegue até a **Sites** console: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   O SPA WKND inclui uma estrutura básica do site com um país, idioma e página inicial. Essa hierarquia é baseada nos valores padrão do arquétipo para `language_country` e `isSingleCountryWebsite`. Esses valores podem ser substituídos pela atualização da variável [propriedades disponíveis](https://github.com/adobe/aem-project-archetype#available-properties) ao gerar um projeto.

2. Abra o **us** > **en** > **Página inicial do WKND SPA React** selecionando a página e clicando no ícone **Editar** botão na barra de menus:

   ![console do site](./assets/create-project/open-home-page.png)

3. A **Texto** já foi adicionado à página. É possível editar esse componente como qualquer outro componente no AEM.

   ![Componente de atualização de texto](./assets/create-project/update-text-component.gif)

4. Adicionar um adicional **Texto** componente à página.

   Observe que a experiência de criação é semelhante àquela de uma página tradicional do AEM Sites. Atualmente, um número limitado de componentes está disponível para uso. Mais informações são adicionadas durante o curso do tutorial.

## Inspect o aplicativo de página única

Em seguida, verifique se este é um aplicativo de página única com o uso das ferramentas de desenvolvedor do seu navegador.

1. No **Editor de páginas**, clique no link **Informações da página** botão > **Exibir como publicado**:

   ![Botão Exibir como Publicado](./assets/create-project/view-as-published.png)

   Isso abrirá uma nova guia com o parâmetro de consulta `?wcmmode=disabled` que desliga efetivamente o editor AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. Visualize a fonte da página e observe que o conteúdo do texto **[!DNL Hello World]** ou qualquer outro conteúdo não for encontrado. Em vez disso, você deve ver HTML como o seguinte:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` O é o SPA do React que é carregado na página e responsável pela renderização do conteúdo.

   No entanto, *de onde vem o conteúdo?*

3. Retorne à guia: [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. Abra as ferramentas de desenvolvedor do navegador e inspecione o tráfego de rede da página durante uma atualização. Exibir o **XHR** solicitações:

   ![Solicitações XHR](./assets/create-project/xhr-requests.png)

   Deve ser feita uma solicitação para [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Ele contém todo o conteúdo, formatado em JSON, que direcionará o SPA.

5. Em uma nova guia, abra [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   A solicitação `en.model.json` representa o modelo de conteúdo que direcionará o aplicativo. Inspect a saída JSON e você poderá encontrar o trecho que representa a variável **[!UICONTROL Texto]** componente(s)

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

   No próximo capítulo, verificaremos como esse conteúdo JSON é mapeado de componentes AEM para componentes SPA, a fim de formar a base da experiência do editor de AEM SPA.

   >[!NOTE]
   >
   > Pode ser útil instalar uma extensão de navegador para formatar automaticamente a saída JSON.

## Parabéns. {#congratulations}

Parabéns, você acabou de criar seu primeiro projeto SPA AEM Editor!

O SPA é bem simples. Nos próximos capítulos, mais funcionalidade será adicionada.

### Próximas etapas {#next-steps}

[Integrar um SPA](integrate-spa.md) - Saiba como o código-fonte do SPA é integrado ao Projeto AEM e entenda as ferramentas disponíveis para desenvolver rapidamente o SPA.
