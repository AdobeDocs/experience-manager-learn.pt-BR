---
title: Introdução ao AEM Sites - Configuração de projetos
description: Crie um projeto com vários módulos do Maven para gerenciar o código e as configurações de um site do Experience Manager.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-3418
thumbnail: 30152.jpg
doc-type: Tutorial
exl-id: bb0cae58-79bd-427f-9116-d46afabdca59
recommendations: noDisplay, noCatalog
duration: 502
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1695'
ht-degree: 100%

---

# Configuração do projeto {#project-setup}

Este tutorial aborda a criação de um projeto com vários módulos do Maven para gerenciar o código e as configurações de um site do Adobe Experience Manager.

## Pré-requisitos {#prerequisites}

Consulte as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](./overview.md#local-dev-environment). Verifique se você dispõe de uma nova instância do Adobe Experience Manager localmente e se nenhum pacote de amostra/demonstração adicional foi instalado (além dos pacotes de serviços necessários).

## Objetivo {#objective}

1. Aprender a gerar um novo projeto do AEM com um arquétipo do Maven.
1. Entender os diferentes módulos gerados pelo arquétipo de projeto do AEM e como eles trabalham juntos.
1. Entender como os componentes principais do AEM são incluídos em um projeto do AEM.

## O que você criará {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/36071?captions=por_br&quality=12&learn=on)

Neste capítulo, você gerará um novo projeto do Adobe Experience Manager, usando o [Arquétipo de projeto do AEM](https://github.com/adobe/aem-project-archetype). O seu projeto do AEM contém o código completo, além de todo o conteúdo e configurações usados em uma implementação do Sites. O projeto gerado neste capítulo servirá de base para uma implementação do site da WKND e será incorporado em capítulos futuros.

**O que é um projeto do Maven?** - O [Apache Maven](https://maven.apache.org/) é uma ferramenta de gerenciamento de softwares para compilar projetos. *Todas as implementações do Adobe Experience Manager* usam projetos do Maven para compilar, gerenciar e implantar códigos personalizados no AEM.

**O que é um arquétipo do Maven?** - Um [Arquétipo do Maven](https://maven.apache.org/archetype/index.html) é um modelo ou padrão para gerar novos projetos. O arquétipo de projeto do AEM ajuda a gerar um novo projeto com um namespace personalizado e incluir uma estrutura de projeto que segue as práticas recomendadas, acelerando bastante o desenvolvimento do projeto.

## Criar o projeto {#create}

Há algumas opções para criar um projeto com vários módulos do Maven no AEM. Este tutorial usa o [Arquétipo de projeto do do AEM do Maven **35**](https://github.com/adobe/aem-project-archetype). O Cloud Manager também [fornece um assistente de IU](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/project-creation/using-the-wizard.html?lang=pt-BR) para iniciar a criação de um projeto de aplicativo do AEM. O projeto subjacente gerado pela IU do Cloud Manager resulta na mesma estrutura que o uso direto do arquétipo.

>[!NOTE]
>
>Este tutorial usa a versão **35** do arquétipo. É considerada uma prática recomendada usar sempre a **versão mais recente** do arquétipo para gerar um novo projeto.

A próxima série de etapas ocorrerá por meio de um terminal de linha de comando baseado no UNIX®, mas será semelhante ao uso de um terminal do Windows.

1. Abra um terminal de linha de comando. Verifique se o Maven está instalado:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. Navegue até o diretório no qual deseja gerar o projeto do AEM. Pode ser qualquer diretório no qual você deseja manter o código-fonte do projeto. Por exemplo, um diretório chamado `code` abaixo do diretório de base do usuário:

   ```shell
   $ cd ~/code
   ```

1. Cole o seguinte na linha de comando para [gerar o projeto no modo de lotes](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=39 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides" \
       -D artifactId="aem-guides-wknd" \
       -D package="com.adobe.aem.guides.wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Para direcionar o AEM 6.5.14+, substitua `aemVersion="cloud"` por `aemVersion="6.5.14"`.
   >
   > Além disso, sempre use o `archetypeVersion` mais recente, consultando [Arquétipo de projeto do AEM > Uso](https://github.com/adobe/aem-project-archetype#usage)

   Uma lista completa de propriedades disponíveis para configurar um projeto [pode ser encontrada aqui](https://github.com/adobe/aem-project-archetype#available-properties).

1. As seguintes pasta e estrutura de arquivos são geradas pelo arquétipo do Maven no sistema de arquivos local:

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.apps.structure/
           |--- ui.config/
           |--- ui.content/
           |--- ui.frontend/
           |--- ui.tests /
           |--- it.tests/
           |--- dispatcher/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## Implantar e compilar o projeto {#build}

Compile e implante o projeto em uma instância local do AEM:

1. Verifique se há uma instância de criação do AEM em execução localmente na porta **4502**.
1. A partir da linha de comando, navegue até o diretório do projeto `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Execute o seguinte comando para compilar e implantar o projeto inteiro no AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   A compilação leva cerca de um minuto e deve terminar com a seguinte mensagem:

   ```
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for WKND Sites Project 0.0.1-SNAPSHOT:
   [INFO] 
   [INFO] WKND Sites Project ................................. SUCCESS [  0.113 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  3.136 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [  4.461 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.359 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  1.732 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  0.956 s]
   [INFO] WKND Sites Project - UI config ..................... SUCCESS [  0.064 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  8.229 s]
   [INFO] WKND Sites Project - Integration Tests ............. SUCCESS [  3.329 s]
   [INFO] WKND Sites Project - Dispatcher .................... SUCCESS [  0.027 s]
   [INFO] WKND Sites Project - UI Tests ...................... SUCCESS [  0.032 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  23.189 s
   [INFO] Finished at: 2023-01-10T11:12:23-05:00
   [INFO] ------------------------------------------------------------------------    
   ```

   O perfil do Maven `autoInstallSinglePackage` compila os módulos individuais do projeto e implanta um pacote unificado na instância do AEM. Por padrão, esse pacote é implantado em uma instância do AEM em execução localmente na porta **4502** e com as credenciais de `admin:admin`.

1. Navegue até o gerenciador de pacotes na sua instância do AEM local: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Você verá pacotes para `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content` e `aem-guides-wknd.all`.

1. Navegue até o console “Sites”: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). O site da WKND é um dos sites. Ele inclui uma estrutura do site com uma hierarquia dos EUA e de idiomas principais. Essa hierarquia do site é baseada nos valores de `language_country` e `isSingleCountryWebsite` ao gerar o projeto com base no arquétipo.

1. Abra a página **EUA** `>` **Inglês**, selecionando a página e clicando no botão **Editar** na barra do menu:

   ![console do site](assets/project-setup/aem-sites-console.png)

1. O conteúdo inicial já foi criado, e vários componentes estão disponíveis para ser adicionados a uma página. Experimente esses componentes para ter uma ideia de como funcionam. Você aprenderá as noções básicas dos componentes no próximo capítulo.

   ![Conteúdo inicial da página inicial](assets/project-setup/start-home-page.png)

   *Conteúdo de amostra gerado pelo arquétipo*

## Inspecionar o projeto {#project-structure}

O projeto do AEM gerado é composto por módulos do Maven individuais, cada um com uma função diferente. Este tutorial e a maioria dos desenvolvimentos focam-se nestes módulos:

* [core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html?lang=pt-BR): código em Java, principalmente para desenvolvedores de back-end.
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=pt-BR): contém código-fonte em CSS, JavaScript, Sass, TypeScript, principalmente para desenvolvedores de front-end.
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html?lang=pt-BR): contém definições de componentes e caixas de diálogo, incorpora o CSS e o JavaScript compilados como bibliotecas de clientes.
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html?lang=pt-BR): inclui o conteúdo estrutural e configurações como modelos editáveis, esquemas de metadados (/content, /conf).

* **all**: este é um módulo do Maven vazio que combina os módulos acima em um mesmo pacote que pode ser implantado em um ambiente do AEM.

![Diagrama do projeto do Maven](assets/project-setup/project-pom-structure.png)

Consulte a [documentação do arquétipo de projeto do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=pt-BR) para mais detalhes sobre **todos** os módulos do Maven.

### Inclusão de componentes principais {#core-components}

Os [Componentes principais do AEM](https://experienceleague.adobe.com/pt-br/docs/experience-manager-core-components/using/introduction) são um conjunto de componentes padronizados de gerenciamento de conteúdo da web (WCM, na sigla em inglês) para o AEM. Esses componentes fornecem um conjunto de linha de base de uma funcionalidade e são estilizados, personalizados e estendidos para projetos individuais.

O ambiente do AEM as a Cloud Service inclui a versão mais recente dos [Componentes principais do AEM](https://experienceleague.adobe.com/pt-br/docs/experience-manager-core-components/using/introduction). Portanto, os projetos gerados para o AEM as a Cloud Service **não** incluem uma integração dos componentes principais do AEM.

Para projetos gerados no AEM 6.5/6.4, o arquétipo incorpora os [Componentes principais do AEM](https://experienceleague.adobe.com/pt-br/docs/experience-manager-core-components/using/introduction) ao projeto automaticamente. É uma prática recomendada do AEM 6.5/6.4 incorporar os componentes principais do AEM para garantir que a versão mais recente seja implantada com o seu projeto. Mais informações sobre como os componentes principais são [incluídos no projeto podem ser encontradas aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html?lang=pt-BR#core-components).

## Gerenciamento de controle de origem {#source-control}

É sempre uma boa ideia usar alguma forma de controle do código-fonte para gerenciar o código no seu aplicativo. Este tutorial usa o Git e o GitHub. Há vários arquivos gerados pelo Maven e/ou pelo IDE da sua escolha que devem ser ignorados pelo SCM.

O Maven cria uma pasta de destino sempre que você cria e instala o pacote de código. A pasta de destino e o conteúdo devem ser excluídos do SCM.

No módulo `ui.apps`, observe que muitos arquivos `.content.xml` são criados. Esses arquivos XML mapeiam os tipos de nó e as propriedades do conteúdo instalados no JCR. Esses arquivos são cruciais e **não podem** ser ignorados.

O arquétipo de projeto do AEM gera um arquivo de amostra `.gitignore` que pode ser usado como ponto de partida e cujos arquivos podem ser ignorados com segurança. O arquivo é gerado em `<src>/aem-guides-wknd/.gitignore`.

## Parabéns! {#congratulations}

Parabéns, você criou o seu primeiro projeto do AEM!

### Próximas etapas {#next-steps}

Entenda a tecnologia subjacente a um componente de site do Adobe Experience Manager (AEM) por meio de um exemplo simples de `HelloWorld` com o tutorial [Noções básicas dos componentes](component-basics.md).

## Comandos avançados do Maven (bônus) {#advanced-maven-commands}

Durante o desenvolvimento, talvez você esteja trabalhando apenas com um dos módulos e queira evitar criar o projeto inteiro para economizar tempo. Você também pode querer implantar diretamente em uma instância de publicação do AEM, ou talvez em uma instância do AEM que não esteja em execução na porta 4502.

Em seguida, vamos rever alguns perfis e comandos do Maven adicionais que você pode usar para obter mais flexibilidade durante o desenvolvimento.

### Módulo principal {#core-module}

O módulo **[core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html?lang=pt-BR)** contém todo o código Java™ associado ao projeto. A compilação do módulo **core** implanta um pacote da OSGi no AEM. Para compilar apenas este módulo:

1. Navegue até a pasta `core` (abaixo de `aem-guides-wknd`):

   ```shell
   $ cd core/
   ```

1. Execute o seguinte comando:

   ```shell
   $ mvn clean install -PautoInstallBundle
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   ```

1. Navegue até [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Esse é o console da web da OSGi que contém informações sobre todos os pacotes instalados na instância do AEM.

1. Alterne a coluna de classificação **Id** para ver o pacote da WKND instalado e ativo.

   ![Pacote principal](assets/project-setup/wknd-osgi-console.png)

1. Você pode ver a localização “físico” do jar no [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar):

   ![Localização do Jar no CRXDE](assets/project-setup/jcr-bundle-location.png)

### Módulos Ui.apps e Ui.content {#apps-content-module}

O módulo **[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html?lang=pt-BR)** do Maven contém todo o código de renderização necessário para o site sob `/apps`. Isso inclui o CSS/JS que será armazenado em um formato do AEM chamado [clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=pt-BR). Também inclui scripts [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html?lang=pt-BR) para renderização de HTML dinâmico. Pense no módulo **ui.apps** como um mapa da estrutura no JCR, mas em um formato que pode ser armazenado em um sistema de arquivos e sujeitado ao controle de origem. O módulo **ui.apps** contém apenas código.

Para compilar apenas este módulo:

1. A partir da linha de comando. Navegue até a pasta `ui.apps` (abaixo de `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. Execute o seguinte comando:

   ```shell
   $ mvn clean install -PautoInstallPackage
   ...
   Package installed in 70ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  2.987 s
   [INFO] Finished at: 2023-01-10T11:35:28-05:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Navegue até [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Você verá o pacote `ui.apps` como o primeiro pacote instalado, e ele terá um carimbo de data/hora mais recente que qualquer um dos outros pacotes.

   ![Pacote Ui.apps instalado](assets/project-setup/ui-apps-package.png)

1. Retorne à linha de comando e execute o seguinte comando (na pasta `ui.apps`):

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/sachinmali/Desktop/code/wknd-tutorial/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  2.812 s
   [INFO] Finished at: 2023-01-10T11:37:28-05:00
   [INFO] ------------------------------------------------------------------------
   [ERROR] Failed to execute goal com.day.jcr.vault:content-package-maven-plugin:1.0.2:install (install-package-publish) on project aem-guides-wknd.ui.apps: Connection refused (Connection refused) -> [Help 1]
   ```

   O perfil `autoInstallPackagePublish` deve implantar o pacote em um ambiente de publicação em execução na porta **4503**. O erro acima é esperado quando uma instância do AEM em execução em http://localhost:4503 não é encontrada.

1. Por fim, execute o seguinte comando para implantar o pacote `ui.apps` na porta **4504**:

   ```shell
   $ mvn -PautoInstallPackage clean install -Daem.port=4504
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4504/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] --------------------------------------------------------------------
   ```

   Novamente, espera-se que ocorra uma falha de compilação quando nenhuma instância do AEM em execução na porta **4504** está disponível. O parâmetro `aem.port` está definido no arquivo POM, em `aem-guides-wknd/pom.xml`.

O módulo **[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html?lang=pt-BR)** está estruturado da mesma forma que o módulo **ui.apps**. A única diferença é que o módulo **ui.content** contém o que é conhecido como conteúdo **mutável**. O conteúdo **mutável** refere-se essencialmente a configurações que não são de código, como modelos, políticas ou estruturas de pastas, armazenadas no controle de origem, **mas** que podem ser modificadas diretamente em uma instância do AEM. Isso será abordado mais detalhadamente no capítulo sobre páginas e modelos.

Os mesmos comandos do Maven usados para criar o módulo **ui.apps** podem ser usados para criar o módulo **ui.content**. Você pode repetir as etapas acima na pasta **ui.content**.

## Resolução de problemas

Se houver algum problema ao gerar o projeto com o arquétipo de projeto do AEM, consulte a lista de [problemas conhecidos](https://github.com/adobe/aem-project-archetype#known-issues) e a lista de [problemas](https://github.com/adobe/aem-project-archetype/issues) em aberto.

## Parabéns novamente! {#congratulations-bonus}

Parabéns por ter passado pelo material de bônus.

### Próximas etapas {#next-steps-bonus}

Entender a tecnologia subjacente a um componente de site do Adobe Experience Manager (AEM) por meio de um exemplo simples do `HelloWorld` com o tutorial [Noções básicas dos componentes](component-basics.md).
