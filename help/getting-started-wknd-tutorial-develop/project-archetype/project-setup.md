---
title: Introdução ao AEM Sites - Configuração do projeto
description: Crie um Projeto de vários módulos Maven para gerenciar o código e as configurações de um site do Experience Manager.
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
workflow-type: tm+mt
source-wordcount: '1684'
ht-degree: 1%

---

# Configuração do projeto {#project-setup}

Este tutorial aborda a criação de um projeto de vários módulos do Maven para gerenciar o código e as configurações de um site do Adobe Experience Manager.

## Pré-requisitos {#prerequisites}

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](./overview.md#local-dev-environment). Verifique se você tem uma nova instância do Adobe Experience Manager disponível localmente e se nenhum pacote de amostra/demonstração adicional foi instalado (além dos Service Packs necessários).

## Objetivo {#objective}

1. Saiba como gerar um novo projeto do AEM usando um arquétipo Maven.
1. Entenda os diferentes módulos gerados pelo Arquétipo de projeto do AEM e como eles trabalham juntos.
1. Entenda como os Componentes principais do AEM são incluídos em um projeto do AEM.

## O que você vai criar {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/36071?quality=12&learn=on&captions=por_br)

Neste capítulo, você gera um novo projeto do Adobe Experience Manager usando o [Arquétipo de Projetos AEM](https://github.com/adobe/aem-project-archetype). Seu projeto do AEM contém código, conteúdo e configurações completos usados para uma implementação do Sites. O projeto gerado neste capítulo serve como base para uma implementação do Site da WKND e será incorporado em capítulos futuros.

**O que é um projeto Maven?** - [Apache Maven](https://maven.apache.org/) é uma ferramenta de gerenciamento de software para compilar projetos. *Todas as implementações do Adobe Experience Manager* usam projetos Maven para compilar, gerenciar e implantar código personalizado sobre o AEM.

**O que é um arquétipo Maven?** - Um [Arquétipo Maven](https://maven.apache.org/archetype/index.html) é um modelo ou padrão para gerar novos projetos. O arquétipo de projeto do AEM ajuda a gerar um novo projeto com um namespace personalizado e incluir uma estrutura de projeto que segue as práticas recomendadas, acelerando bastante o desenvolvimento do projeto.

## Criar o projeto {#create}

Há algumas opções para criar um projeto de vários módulos do Maven para o AEM. Este tutorial usa o [Arquétipo de projeto do Maven AEM **35**](https://github.com/adobe/aem-project-archetype). O Cloud Manager também [fornece um assistente de interface](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/project-creation/using-the-wizard.html?lang=pt-BR) para iniciar a criação de um projeto de aplicativo do AEM. O projeto subjacente gerado pela interface do usuário do Cloud Manager resulta na mesma estrutura que o uso direto do arquétipo.

>[!NOTE]
>
>Este tutorial usa a versão **35** do arquétipo. É sempre uma prática recomendada usar a versão **mais recente** do arquétipo para gerar um novo projeto.

A próxima série de etapas ocorrerá usando um terminal de linha de comando baseado em UNIX®, mas deverá ser semelhante se estiver usando um terminal Windows.

1. Abra um terminal de linha de comando. Verifique se o Maven está instalado:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. Navegue até um diretório no qual deseja gerar o projeto AEM. Pode ser qualquer diretório no qual você deseja manter o código-fonte do projeto. Por exemplo, um diretório chamado `code` abaixo do diretório base do usuário:

   ```shell
   $ cd ~/code
   ```

1. Cole o seguinte na linha de comando para [gerar o projeto no modo de lote](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

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
   > Além disso, sempre use o `archetypeVersion` mais recente referindo-se ao [Arquétipo de Projetos AEM > Uso](https://github.com/adobe/aem-project-archetype#usage)

   Uma lista completa de propriedades disponíveis para configurar um projeto [pode ser encontrada aqui](https://github.com/adobe/aem-project-archetype#available-properties).

1. A seguinte pasta e estrutura de arquivo são geradas pelo arquétipo Maven no sistema de arquivos local:

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

## Implantar e criar o projeto {#build}

Crie e implante o código do projeto em uma instância local do AEM.

1. Verifique se há uma instância de autor do AEM em execução localmente na porta **4502**.
1. Na linha de comando, navegue até o diretório de projeto `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Execute o seguinte comando para criar e implantar o projeto inteiro no AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   A build leva cerca de um minuto e deve terminar com a seguinte mensagem:

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

   O perfil Maven `autoInstallSinglePackage` compila os módulos individuais do projeto e implanta um único pacote na instância do AEM. Por padrão, esse pacote é implantado em uma instância do AEM em execução localmente na porta **4502** e com as credenciais de `admin:admin`.

1. Navegue até o Gerenciador de Pacotes na sua instância do AEM local: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Você deve ver pacotes para `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content` e `aem-guides-wknd.all`.

1. Navegue até o console Sites: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). O site da WKND é um dos sites. Ele inclui uma estrutura de site com uma hierarquia dos EUA e de Idiomas principais. Esta hierarquia de site é baseada nos valores de `language_country` e `isSingleCountryWebsite` ao gerar o projeto usando o arquétipo.

1. Abra a página **US** `>` **Inglês** selecionando a página e clicando no botão **Editar** na barra de menus:

   ![console do site](assets/project-setup/aem-sites-console.png)

1. O conteúdo inicial já foi criado e vários componentes estão disponíveis para serem adicionados a uma página. Experimente esses componentes para ter uma ideia da funcionalidade. Você aprenderá as noções básicas de um componente no próximo capítulo.

   ![Conteúdo inicial da página inicial](assets/project-setup/start-home-page.png)

   *Conteúdo de exemplo gerado pelo Arquétipo*

## Inspecionar o projeto {#project-structure}

O projeto AEM gerado é composto de módulos Maven individuais, cada um com uma função diferente. Este tutorial e a maioria dos desenvolvimentos se concentram nestes módulos:

* [core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html?lang=pt-BR) - Código Java, principalmente desenvolvedores de back-end.
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=pt-BR) - Contém código-fonte para CSS, JavaScript, Sass, TypeScript, principalmente para desenvolvedores de front-end.
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html?lang=pt-BR) - Contém definições de componentes e caixas de diálogo, incorpora CSS e JavaScript compilados como bibliotecas de clientes.
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html?lang=pt-BR) - contém conteúdo estrutural e configurações como modelos editáveis, esquemas de metadados (/content, /conf).

* **todos** - este é um módulo Maven vazio que combina os módulos acima em um único pacote que pode ser implantado em um ambiente AEM.

![Diagrama de projeto Maven](assets/project-setup/project-pom-structure.png)

Consulte a [documentação do Arquétipo de projeto do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=pt-BR) para saber mais detalhes sobre **todos** os módulos Maven.

### Inclusão dos Componentes principais {#core-components}

Os [Componentes principais do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR) são um conjunto de componentes padronizados de WCM (Gerenciamento de Conteúdo da Web) para o AEM. Esses componentes fornecem um conjunto de linha de base de uma funcionalidade e são estilizados, personalizados e estendidos para projetos individuais.

O ambiente do AEM as a Cloud Service inclui a versão mais recente dos [Componentes principais do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR). Portanto, os projetos gerados para o AEM as a Cloud Service **não** incluem uma inserção dos Componentes principais do AEM.

Para projetos gerados pelo AEM 6.5/6.4, o arquétipo incorpora automaticamente [Componentes principais do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR) no projeto. É uma prática recomendada do AEM 6.5/6.4 incorporar os Componentes principais do AEM para garantir que a versão mais recente seja implantada com o seu projeto. Mais informações sobre como os Componentes Principais estão [incluídos no projeto podem ser encontradas aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html?lang=pt-BR#core-components).

## Gerenciamento de controle do Source {#source-control}

É sempre uma boa ideia usar alguma forma de controle do código-fonte para gerenciar o código em seu aplicativo. Este tutorial usa Git e GitHub. Há vários arquivos que são gerados pelo Maven e/ou pelo IDE de escolha que devem ser ignorados pelo SCM.

O Maven cria uma pasta de destino sempre que você cria e instala o pacote de código. A pasta de destino e o conteúdo devem ser excluídos do SCM.

Em, o módulo `ui.apps` observa que muitos arquivos `.content.xml` são criados. Esses arquivos XML mapeiam os tipos de nó e as propriedades do conteúdo instalado no JCR. Estes arquivos são críticos e **não pode** ser ignorado.

O arquétipo de projeto do AEM gera um arquivo de amostra `.gitignore` que pode ser usado como ponto de partida para o qual os arquivos podem ser ignorados com segurança. O arquivo é gerado em `<src>/aem-guides-wknd/.gitignore`.

## Parabéns. {#congratulations}

Parabéns, você criou seu primeiro projeto do AEM!

### Próximas etapas {#next-steps}

Entenda a tecnologia subjacente de um componente do Sites do Adobe Experience Manager (AEM) por meio de um exemplo simples do `HelloWorld` com o tutorial [Noções básicas sobre componentes](component-basics.md).

## Comandos Maven avançados (Bônus) {#advanced-maven-commands}

Durante o desenvolvimento, você pode estar trabalhando apenas com um dos módulos e deseja evitar criar o projeto inteiro para economizar tempo. Você também pode querer implantar diretamente em uma instância de publicação do AEM ou talvez em uma instância do AEM que não esteja em execução na porta 4502.

Em seguida, vamos rever alguns perfis e comandos Maven adicionais que você pode usar para obter mais flexibilidade durante o desenvolvimento.

### Módulo principal {#core-module}

O módulo **[core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html?lang=pt-BR)** contém todo o código Java™ associado ao projeto. A compilação do módulo **core** implanta um pacote OSGi no AEM. Para criar apenas este módulo:

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

1. Navegue até [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Este é o console OSGi da Web e contém informações sobre todos os pacotes instalados na instância do AEM.

1. Alterne a coluna de classificação **Id** e você deverá ver o pacote WKND instalado e ativo.

   ![Pacote principal](assets/project-setup/wknd-osgi-console.png)

1. Você pode ver o local &quot;físico&quot; do jar em [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar):

   ![Local CRXDE do Jar](assets/project-setup/jcr-bundle-location.png)

### Módulos Ui.apps e Ui.content {#apps-content-module}

O módulo maven **[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html?lang=pt-BR)** contém todo o código de renderização necessário para o site abaixo de `/apps`. Isso inclui CSS/JS armazenado em um formato AEM chamado [clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=pt-BR). Também inclui scripts [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html?lang=pt-BR) para renderização de HTML dinâmico. Pense no módulo **ui.apps** como um mapa para a estrutura no JCR, mas em um formato que pode ser armazenado em um sistema de arquivos e comprometido com o controle de origem. O módulo **ui.apps** contém apenas código.

Para criar apenas este módulo:

1. Na linha de comando. Navegue até a pasta `ui.apps` (abaixo de `aem-guides-wknd`):

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

1. Navegue até [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Você deve ver o pacote `ui.apps` como o primeiro pacote instalado e ele deve ter um carimbo de data/hora mais recente do que qualquer um dos outros pacotes.

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

   O perfil `autoInstallPackagePublish` deve implantar o pacote em um ambiente de Publicação em execução na porta **4503**. O erro acima é esperado se uma instância do AEM em execução em http://localhost:4503 não puder ser encontrada.

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

   Novamente, espera-se que ocorra uma falha de compilação se nenhuma instância do AEM em execução na porta **4504** estiver disponível. O parâmetro `aem.port` está definido no arquivo POM em `aem-guides-wknd/pom.xml`.

O módulo **[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html?lang=pt-BR)** está estruturado da mesma forma que o módulo **ui.apps**. A única diferença é que o módulo **ui.content** contém o que é conhecido como conteúdo **mutável**. O conteúdo **Mutável** refere-se essencialmente a configurações que não são de código, como Modelos, Políticas ou estruturas de pastas, armazenadas no controle de origem **mas** podem ser modificadas diretamente em uma instância do AEM. Isso é explorado com mais detalhes no capítulo sobre Páginas e modelos.

Os mesmos comandos Maven usados para criar o módulo **ui.apps** podem ser usados para criar o módulo **ui.content**. Você pode repetir as etapas acima na pasta **ui.content**.

## Resolução de problemas

Se houver algum problema ao gerar o projeto usando o Arquétipo de Projetos AEM, consulte a lista de [problemas conhecidos](https://github.com/adobe/aem-project-archetype#known-issues) e a lista de [problemas](https://github.com/adobe/aem-project-archetype/issues) abertos.

## Parabéns novamente! {#congratulations-bonus}

Parabéns, por ter passado pelo material bônus.

### Próximas etapas {#next-steps-bonus}

Entenda a tecnologia subjacente de um componente do Sites do Adobe Experience Manager (AEM) por meio de um exemplo simples do `HelloWorld` com o tutorial [Noções básicas sobre componentes](component-basics.md).
