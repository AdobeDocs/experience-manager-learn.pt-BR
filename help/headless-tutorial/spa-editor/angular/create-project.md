---
title: Projeto do SPA Editor | Introdução ao AEM SPA Editor e Angular
description: Saiba como usar um projeto Maven do Adobe Experience Manager (AEM) como ponto de partida para um aplicativo do Angular integrado ao AEM SPA Editor.
feature: SPA Editor, AEM Project Archetype
version: Experience Manager as a Cloud Service
jira: KT-5309
thumbnail: 5309-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 49fcd603-ab1a-4f1e-ae1f-49d3ff373439
duration: 252
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '1002'
ht-degree: 1%

---

# Projeto do SPA Editor {#create-project}

{{spa-editor-deprecation}}

Saiba como usar um projeto Maven do Adobe Experience Manager (AEM) como ponto de partida para um aplicativo do Angular integrado ao AEM SPA Editor.

## Objetivo

1. Entenda a estrutura de um novo projeto do Editor SPA do AEM criado a partir de um arquétipo Maven.
2. Implante o projeto inicial em uma instância local do AEM.

## O que você vai criar

Neste capítulo, um novo projeto do AEM é implantado, com base no [Arquétipo de Projetos AEM](https://github.com/adobe/aem-project-archetype). O projeto do AEM é inicializado com um ponto de partida muito simples para o SPA do Angular. O projeto usado neste capítulo servirá de base para uma implementação da SPA da WKND e será desenvolvido em capítulos futuros.

![Projeto inicial do WKND SPA Angular](./assets/create-project/what-you-will-build.png)

*Uma mensagem clássica do Hello World.*

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment). Verifique se uma nova instância do Adobe Experience Manager, iniciada no modo **author**, está em execução localmente.

## Obter o projeto

Há várias opções para criar um projeto de vários módulos do Maven para o AEM. Este tutorial usou o [Arquétipo de projeto do AEM](https://github.com/adobe/aem-project-archetype) mais recente como base para o código do tutorial. Foram feitas modificações no código do projeto para oferecer suporte a várias versões do AEM. Revise [a observação sobre compatibilidade com versões anteriores](overview.md#compatibility).

>[!CAUTION]
>
>É uma prática recomendada usar a versão **mais recente** do [arquétipo](https://github.com/adobe/aem-project-archetype) para gerar um novo projeto para uma implementação real. Os projetos do AEM devem ter como alvo uma única versão do AEM usando a propriedade `aemVersion` do arquétipo.

1. Baixe o ponto de partida para este tutorial pelo Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/create-project-start
   ```

2. A seguinte estrutura de pasta e arquivo representa o projeto do AEM que foi gerado pelo arquétipo Maven no sistema de arquivos local:

   ```plain
   |--- aem-guides-wknd-spa
       |--- all/
       |--- core/
       |--- dispatcher/
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.content/
       |--- ui.frontend /
       |--- it.tests/
       |--- pom.xml
       |--- README.md
       |--- .gitignore
       |--- archetype.properties
   ```

3. As seguintes propriedades foram usadas ao gerar o projeto AEM a partir do [Arquétipo de projeto do AEM](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14):

   | Propriedade | Valor |
   |-----------------|---------------------------------------|
   | aemVersion | nuvem |
   | appTitle | WKND SPA ANGULAR |
   | appId | wknd-spa-angular |
   | groupId | com.adobe.aem.guides |
   | frontendModule | angular |
   | pacote | com.adobe.aem.guides.wknd.spa.angular |
   | includeExamples | n |

   >[!NOTE]
   >
   > Observe a propriedade `frontendModule=angular`. Isso instrui o Arquétipo de Projetos AEM a inicializar o projeto com uma [base de código Angular](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html) inicial a ser usada com o Editor SPA do AEM.

## Criar o projeto

Em seguida, compile, crie e implante o código do projeto em uma instância local do AEM usando o Maven.

1. Verifique se uma instância do AEM está sendo executada localmente na porta **4502**.
2. No terminal de linha de comando, verifique se o Maven está instalado:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. Execute o comando Maven abaixo no diretório `aem-guides-wknd-spa` para compilar e implantar o projeto no AEM:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ```

   Se estiver usando o [AEM 6.x](overview.md#compatibility):

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   Os vários módulos do projeto devem ser compilados e implantados no AEM.

   ```plain
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for wknd-spa-angular 1.0.0-SNAPSHOT:
   [INFO] 
   [INFO] wknd-spa-angular ................................... SUCCESS [  0.473 s]
   [INFO] WKND SPA Angular - Core ............................ SUCCESS [ 54.866 s]
   [INFO] wknd-spa-angular.ui.frontend - UI Frontend ......... SUCCESS [02:10 min]
   [INFO] WKND SPA Angular - Repository Structure Package .... SUCCESS [  0.694 s]
   [INFO] WKND SPA Angular - UI apps ......................... SUCCESS [  6.351 s]
   [INFO] WKND SPA Angular - UI content ...................... SUCCESS [  2.885 s]
   [INFO] WKND SPA Angular - All ............................. SUCCESS [  1.736 s]
   [INFO] WKND SPA Angular - Integration Tests Bundles ....... SUCCESS [  2.563 s]
   [INFO] WKND SPA Angular - Integration Tests Launcher ...... SUCCESS [  1.846 s]
   [INFO] WKND SPA Angular - Dispatcher ...................... SUCCESS [  0.270 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   O perfil Maven ***autoInstallSinglePackage*** compila os módulos individuais do projeto e implanta um único pacote na instância do AEM. Por padrão, este pacote é implantado em uma instância do AEM em execução localmente na porta **4502** e com as credenciais de **admin:admin**.

4. Navegue até **[!UICONTROL Gerenciador de Pacotes]** na sua instância do AEM local: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. Você deve ver três pacotes para `wknd-spa-angular.all`, `wknd-spa-angular.ui.apps` e `wknd-spa-angular.ui.content`.

   ![Pacotes de SPA do WKND](./assets/create-project/package-manager.png)

   Todo o código personalizado necessário para o projeto é incorporado a esses pacotes e instalado no tempo de execução do AEM.

6. Você também deve ver vários pacotes para `spa.project.core` e `core.wcm.components`. Essas são dependências incluídas automaticamente pelo arquétipo. Mais informações sobre os [Componentes Principais do AEM podem ser encontradas aqui](https://experienceleague.adobe.com/pt-br/docs/experience-manager-core-components/using/introduction).

## Conteúdo do autor

Em seguida, abra o SPA inicial gerado pelo arquétipo e atualize parte do conteúdo.

1. Navegue até o console **[!UICONTROL Sites]**: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   O WKND SPA inclui uma estrutura básica do site com um país, idioma e página inicial. Esta hierarquia é baseada nos valores padrão do arquétipo para `language_country` e `isSingleCountryWebsite`. Estes valores podem ser substituídos atualizando as [propriedades disponíveis](https://github.com/adobe/aem-project-archetype#available-properties) ao gerar um projeto.

2. Abra a página **[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA Angular Home Page]** selecionando a página e clicando no botão **[!UICONTROL Editar]** na barra de menus:

   ![console do site](./assets/create-project/open-home-page.png)

3. Um componente **[!UICONTROL Texto]** já foi adicionado à página. Você pode editar esse componente como qualquer outro componente no AEM.

   ![Atualizar componente de Texto](./assets/create-project/update-text-component.gif)

4. Adicionar um componente **[!UICONTROL Texto]** adicional à página.

   Observe que a experiência de criação é semelhante àquela de uma página tradicional do AEM Sites. Atualmente, um número limitado de componentes está disponível para uso. Mais informações são adicionadas durante o curso do tutorial.

## Inspecionar o aplicativo de página única

Em seguida, verifique se este é um aplicativo de página única com o uso das ferramentas de desenvolvedor do seu navegador.

1. No **[!UICONTROL Editor de páginas]**, clique no menu **[!UICONTROL Informações da Página]** > **[!UICONTROL Exibir como Publicado]**:

   ![Botão Exibir como Publicado](./assets/create-project/view-as-published.png)

   Isso abrirá uma nova guia com o parâmetro de consulta `?wcmmode=disabled` que desativa efetivamente o editor do AEM: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. Exiba a fonte da página e observe que o conteúdo do texto **[!DNL Hello World]** ou qualquer outro conteúdo não foi encontrado. Em vez disso, você deve ver o HTML da seguinte maneira:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-angular/clientlibs/clientlib-angular.min.js"></script>
       ...
   </body>
   ...
   ```

   `clientlib-angular.min.js` é o SPA do Angular carregado na página e responsável pela renderização do conteúdo.

   *De onde vem o conteúdo?*

3. Retorne à guia: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. Abra as ferramentas de desenvolvedor do navegador e inspecione o tráfego de rede da página durante uma atualização. Exibir as solicitações de **XHR**:

   ![Solicitações XHR](./assets/create-project/xhr-requests.png)

   Deve haver uma solicitação para [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Ele contém todo o conteúdo, formatado em JSON, que direcionará o SPA.

5. Em uma nova guia, abra [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   A solicitação `en.model.json` representa o modelo de conteúdo que direcionará o aplicativo. Inspecione a saída JSON e você poderá encontrar o trecho que representa o(s) componente(s) **[!UICONTROL Texto]**.

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
   },
   ...
   ```

   No próximo capítulo, verificaremos como o conteúdo JSON é mapeado dos componentes do AEM para os componentes SPA, para formar a base da experiência do Editor SPA do AEM.

   >[!NOTE]
   >
   > Pode ser útil instalar uma extensão de navegador para formatar automaticamente a saída JSON.

## Parabéns! {#congratulations}

Parabéns, você acabou de criar seu primeiro projeto do Editor SPA do AEM.

É bastante simples neste momento, mas nos próximos capítulos é adicionada mais funcionalidade.

### Próximas etapas {#next-steps}

[Integrar o SPA](integrate-spa.md) - Saiba como o código-fonte do SPA é integrado ao Projeto do AEM e entenda as ferramentas disponíveis para desenvolver rapidamente o SPA.
