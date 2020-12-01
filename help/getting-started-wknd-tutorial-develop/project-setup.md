---
title: Introdução ao AEM Sites - Configuração do projeto
seo-title: Introdução ao AEM Sites - Configuração do projeto
description: Abrange a criação de um Projeto Maven Multi Module para gerenciar o código e as configurações de um Site AEM.
sub-product: sites
feature: maven-archetype
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
translation-type: tm+mt
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '2468'
ht-degree: 4%

---


# Configuração do projeto {#project-setup}

Este tutorial aborda a criação de um Projeto Maven Multi Module para gerenciar o código e as configurações de um Site da Adobe Experience Manager.

## Pré-requisitos {#prerequisites}

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment). Verifique se você tem uma nova instância da Adobe Experience Manager disponível localmente e se nenhum pacote adicional de amostra/demonstração foi instalado (exceto os Service Packs necessários).

## Objetivo {#objective}

1. Saiba como gerar um novo projeto AEM usando um arquétipo Maven.
1. Entenda os diferentes módulos gerados pelo AEM Project Archetype e como eles trabalham juntos.
1. Entenda como AEM Componentes principais estão incluídos em um Projeto AEM.

## O que você vai criar {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

Neste capítulo, você gerará um novo projeto da Adobe Experience Manager usando o [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). Seu projeto AEM contém todos os códigos, conteúdos e configurações usados para a implementação do Sites. O projeto gerado neste capítulo servirá de base para a implementação do Site da WKND e será desenvolvido em futuros capítulos.

## Segundo plano {#background}

**O que é um projeto Maven?** -  [Apache ](https://maven.apache.org/) Mavenis é uma ferramenta de gerenciamento de software para a construção de projetos. *Todas as implementações do Adobe Experience* Manager usam projetos Maven para criar, gerenciar e implantar código personalizado sobre AEM.

**O que é um arquétipo Maven?** - Um  [arquétipo ](https://maven.apache.org/archetype/index.html) Maven é um modelo ou padrão para gerar novos projetos. O arquétipo AEM Projeto nos permite gerar um novo projeto com uma namespace personalizada e incluir uma estrutura de projeto que segue as práticas recomendadas, acelerando muito nosso projeto.

## Criar o projeto {#create}

Há algumas opções para criar um projeto Maven Multi-module para AEM. Este tutorial aproveitará o [Maven AEM Project Archetype **22**](https://github.com/adobe/aem-project-archetype). O Cloud Manager também [fornece um assistente de interface de usuário](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html) para iniciar a criação de um projeto de aplicativo AEM. O projeto subjacente gerado pela interface do usuário do Cloud Manager resulta na mesma estrutura que o uso direto do tipo de arquétipo.

>[!NOTE]
>
>Para os fins de seguir este tutorial, use a versão **22** do arquétipo. No entanto, é sempre uma prática recomendada usar a versão **mais recente** do arquétipo para gerar um novo projeto.

A próxima série de etapas ocorrerá usando um terminal de linha de comando baseado em UNIX, mas deve ser semelhante se estiver usando um terminal do Windows.

1. Abra um terminal de linha de comando e verifique se Maven foi instalado e adicionado ao caminho da linha de comando:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. Verifique se o perfil **adobe-public** está ativo executando o seguinte comando:

   ```shell
   $ mvn help:effective-settings
       ...
   <activeProfiles>
       <activeProfile>adobe-public</activeProfile>
   </activeProfiles>
   <pluginGroups>
       <pluginGroup>org.apache.maven.plugins</pluginGroup>
       <pluginGroup>org.codehaus.mojo</pluginGroup>
   </pluginGroups>
   </settings>
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  0.856 s
   ```

   Se você **not** consultar o **adobe-public**, isso indica que o Adobe repo não é referenciado corretamente no arquivo `~/.m2/settings.xml`. Revise as etapas para instalar e configurar o Apache Maven em [um ambiente de desenvolvimento local](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven).

1. Navegue até um diretório no qual você deseja gerar o projeto AEM. Pode ser qualquer diretório no qual você deseja manter o código-fonte do seu projeto. Por exemplo, um diretório chamado `code` abaixo do diretório inicial do usuário:

   ```shell
   $ cd ~/code
   ```

1. Cole o seguinte na linha de comando para [gerar o projeto no modo de lote](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

   ```shell
   $ mvn archetype:generate -B \
       -DarchetypeGroupId=com.adobe.granite.archetypes \
       -DarchetypeArtifactId=aem-project-archetype \
       -DarchetypeVersion=22 \
       -DgroupId=com.adobe.aem.guides \
       -Dversion=0.0.1-SNAPSHOT \
       -DappsFolderName=wknd \
       -DartifactId=aem-guides-wknd \
       -Dpackage=com.adobe.aem.guides.wknd \
       -DartifactName="WKND Sites Project" \
       -DcomponentGroupName=WKND \
       -DconfFolderName=wknd \
       -DcontentFolderName=wknd \
       -DcssId=wknd \
       -DisSingleCountryWebsite=n \
       -Dlanguage_country=en_us \
       -DoptionAemVersion=6.5.0 \
       -DoptionDispatcherConfig=none \
       -DoptionIncludeErrorHandler=n \
       -DoptionIncludeExamples=y \
       -DoptionIncludeFrontendModule=y \
       -DpackageGroup=wknd \
       -DsiteName="WKND Site"
   ```

   >[!NOTE]
   >
   >Por padrão, gerar um projeto a partir do arquétipo Maven usa o modo interativo. Para evitar dedos gordos em quaisquer valores que geramos usando o modo de lote. Também é possível criar o projeto Maven AEM usando o plug-in [AEM Developer Tools para Eclipse](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/aem-eclipse.html).

   >[!CAUTION]
   >
   >Se você receber um erro como o seguinte: *Falha ao executar org.apache.maven.plugins:maven-archetype-plugin:3.1.1:generate (default-cli) no pom autônomo do projeto: O arquétipo desejado não existe*. É uma indicação de que o Adobe repo não é corretamente referenciado no arquivo `~/.m2/settings.xml`. Revise as etapas anteriores e verifique se o arquivo settings.xml faz referência ao Adobe repo.

   A tabela a seguir lista os valores usados para este tutorial:

   | Nome | Valores | Descrição |
   |-----------------------------|---------|--------------------|
   | groupId | com.adobe.aem.guides | ID de grupo do Base Maven |
   | artifatoId | aem-guides-wknd | Base Maven ArtiactualId |
   | version | 0.0.1-SNAPSHOT | Versão |
   | package | com.adobe.aem.guides.wknd | Pacote de origem Java |
   | appsFolderName | wknd | /apps nome da pasta |
   | artiactualName | Projeto de Sites WKND | Nome do projeto Maven |
   | componentGroupName | WKND | Nome do grupo de componentes AEM |
   | contentFolderName | wknd | /nome da pasta de conteúdo |
   | confFolderName | wknd | /conf nome da pasta |
   | cssId | wknd | prefixo usado em css gerado |
   | packageGroup | wknd | Nome do grupo de pacotes de conteúdo |
   | siteName | Site WKND | Nome do site AEM |
   | optionAemVersion | 6,5,0 | Versão do público alvo AEM |
   | language_country | en_us | código de idioma/país para criar a estrutura de conteúdo de (por exemplo, en_us) |
   | optionIncludeExamples | y | Incluir um site de exemplo da Biblioteca de componentes |
   | optionIncludeErrorHandler | n | Incluir uma página de resposta 404 personalizada |
   | optionIncludeFrontModule | y | Incluir um módulo de front-end dedicado |
   | isSingleCountryWebsite | n | Criar estrutura principal de idioma no conteúdo de exemplo |
   | optionDispatcherConfig | nenhum | Gerar um módulo de configuração do dispatcher |

1. A seguinte estrutura de pastas e arquivos será gerada pelo arquétipo Maven em seu sistema de arquivos local:

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.content/
           |--- ui.frontend /
           |--- it.launcher/
           |--- it.tests/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## Crie o projeto {#build}

Agora que geramos um novo projeto, podemos implantar o código do projeto em uma instância local do AEM.

1. Verifique se você tem uma instância de AEM em execução localmente na porta **4502**.
1. Na linha de comando, navegue até o diretório do projeto `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Execute o seguinte comando para criar e implantar o projeto inteiro no AEM:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   O perfil Maven `autoInstallSinglePackage` compila os módulos individuais do projeto e implanta um único pacote para a instância AEM. Por padrão, esse pacote será implantado em uma instância AEM executada localmente na porta **4502** e com as credenciais de `admin:admin`.

1. Navegue até Gerenciador de pacotes na instância de AEM local: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Você deve ver três pacotes para `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.content` e `aem-guides-wknd.all`.

   Você também deve ver vários pacotes para [AEM Componentes principais](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/introduction.html) que estão incluídos no projeto pelo arquétipo. Isso será abordado posteriormente no tutorial.

1. Navegue até o console Sites: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). O site WKND será um dos sites. Ela incluirá uma estrutura de site com uma hierarquia de Mestres de idioma e EUA. Essa hierarquia de site se baseia nos valores de `language_country` e `isSingleCountryWebsite` ao gerar o projeto usando o arquétipo.

1. Abra a página **US** `>` **Inglês** selecionando a página e clicando no botão **Editar** na barra de menus:

   ![console do site](assets/project-setup/aem-sites-console.png)

1. Alguns conteúdos já foram criados e vários componentes estão disponíveis para serem adicionados a uma página. Experimente esses componentes para ter uma ideia da funcionalidade. A forma como esta página e os componentes são configurados será explorada detalhadamente posteriormente no tutorial.

## Inspect o projeto {#project-structure}

O arquétipo AEM é composto por módulos Maven individuais:

* [núcleo](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)  - conjunto Java que contém todas as funcionalidades principais, como serviços OSGi, ouvintes ou scheduleres, bem como código Java relacionado a componentes, como servlets ou filtros de solicitação.
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html) - contém as partes /apps do projeto, ou seja, clientlibs JS&amp;CSS, componentes e configurações OSGi
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html) - contém conteúdo estrutural e configurações como modelos editáveis, schemas de metadados (/content, /conf)
* ui.tests - conjunto Java que contém testes JUnit executados no servidor. Este pacote não deve ser implantado na produção.
* ui.launch - contém o código de cola que implanta o pacote ui.testing (e os pacotes dependentes) para o servidor e aciona a execução remota da JUnit
* [ui.front-end](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html) - (opcional) contém os artefatos necessários para usar o módulo de compilação front-end baseado no Webpack.
* all - este é um módulo Maven vazio que combina os módulos acima em um único pacote que pode ser implantado em um ambiente AEM.

![Diagrama do projeto Maven](assets/project-setup/project-pom-structure.png)

Consulte a [AEM Documentação do Project Archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html) para saber mais detalhes sobre os módulos Maven.

## Comandos Maven avançados {#advanced-maven-commands}

Durante o desenvolvimento, talvez você esteja trabalhando com apenas um dos módulos e queira evitar a construção do projeto inteiro para economizar tempo. Você também pode desejar implantar diretamente em uma instância do AEM Publish ou talvez em uma instância de AEM que não esteja em execução na porta 4502.

Em seguida, vamos observar alguns perfis e comandos Maven adicionais que podem ser usados para maior flexibilidade durante o desenvolvimento.

### Módulo principal {#core-module}

O módulo **[core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)** contém todo o código Java associado ao projeto. Quando criado, ele implanta um pacote OSGi para AEM. Para criar apenas este módulo:

1. Navegue até a pasta `core` (abaixo de `aem-guides-wknd`):

   ```shell
   $ cd core/
   ```

1. Execute o seguinte comando:

   ```shell
   $ mvn -PautoInstallBundle clean install
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   [INFO] Finished at: 2019-12-06T13:40:21-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Navegue até [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Este é o console Web OSGi e contém informações sobre todos os pacotes instalados na instância do AEM.

1. Alterne a coluna de classificação **Id** e você deverá ver o conjunto WKND instalado e ativo.

   ![Pacote principal](assets/project-setup/wknd-osgi-console.png)

1. Você pode ver o local &#39;físico&#39; do jar em [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd/install/wknd-sites-guide.core-0.0.1-SNAPSHOT.jar):

   ![Localização do Jar no CRXDE](assets/project-setup/jcr-bundle-location.png)

### Módulos Ui.apps e Ui.content {#apps-content-module}

O módulo **[ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)** contém todo o código de renderização necessário para o site abaixo de `/apps`. Isso inclui CSS/JS que serão armazenados em um formato AEM chamado [clientlibs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html). Isso também inclui scripts [HTL](https://docs.adobe.com/docs/en/htl/overview.html) para renderização de HTML dinâmico. Você pode considerar o módulo **ui.apps** como um mapa para a estrutura no JCR, mas em um formato que pode ser armazenado em um sistema de arquivos e confirmado para o controle de origem. O módulo **ui.apps** contém apenas código.

Para criar este módulo:

1. Na linha de comando. Navegue até a pasta `ui.apps` (abaixo de `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. Execute o seguinte comando:

   ```shell
   $ mvn -PautoInstallPackage clean install
   ...
   Package installed in 122ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.972 s
   [INFO] Finished at: 2019-12-06T14:44:12-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Navegue até [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Você deve ver o pacote `ui.apps` como o primeiro pacote instalado e ele deve ter um carimbo de data e hora mais recente do que qualquer outro pacote.

   ![Pacote Ui.apps instalado](assets/project-setup/ui-apps-package.png)

1. Retorne à linha de comando e execute o seguinte comando (na pasta `ui.apps`):

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.717 s
   [INFO] Finished at: 2019-12-06T14:51:45-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   O perfil `autoInstallPackagePublish` destina-se a implantar o pacote em um ambiente Publish que em execução na porta **4503**. O erro acima é esperado se uma instância AEM em execução em http://localhost:4503 não for encontrada.

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

   Novamente, uma falha de compilação ocorrerá se nenhuma instância AEM em execução na porta **4504** estiver disponível. O parâmetro `aem.port` é definido no arquivo POM em `aem-guides-wknd/pom.xml`.

O módulo **[ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)** está estruturado da mesma forma que o módulo **ui.apps**. A única diferença é que o módulo **ui.content** contém o que é conhecido como conteúdo **mutable**. **O**   **** conteúdo multiablecontent refere-se essencialmente a configurações não-código, como Modelos, Políticas ou estruturas de pastas, que são armazenadas no controle do código-fonte, mas que podem ser modificadas diretamente em uma instância AEM. Isso será explorado com muito mais detalhes no capítulo sobre Páginas e Modelos. Por enquanto, o importante é que os mesmos comandos Maven usados para criar o módulo **ui.apps** podem ser usados para criar o módulo **ui.content**. Sinta-se à vontade para repetir as etapas acima na pasta **ui.content**.

### Módulo Ui.front-end {#ui-frontend-module}

O módulo **[ui.front-end](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)** é um módulo Maven que é na verdade um projeto [webpack](https://webpack.js.org/). Este módulo está configurado para ser um sistema de compilação front-end dedicado que gera arquivos JavaScript e CSS, que por sua vez são implantados em AEM. O módulo **ui.front-end** permite aos desenvolvedores codificar com idiomas como [Sass](https://sass-lang.com/), [TypeScript](https://www.typescriptlang.org/), usar os módulos [npm](https://www.npmjs.com/) e integrar a saída diretamente no AEM.

O módulo **ui.front-end** será abordado com muito mais detalhes no capítulo sobre bibliotecas do lado do cliente e desenvolvimento de front-end. Por enquanto vamos ver como ele é integrado ao projeto.

1. Na linha de comando. Navegue até a pasta `ui.frontend` (abaixo de `aem-guides-wknd`):

   ```shell
   $ cd ../ui.frontend
   ```

1. Execute o seguinte comando:

   ```shell
   $ mvn clean install
   ...
   [INFO] write clientlib asset txt file (type: js): ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js.txt
   [INFO] copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   [INFO]
   [INFO] write clientlib asset txt file (type: css): ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt
   [INFO] copy: dist/clientlib-site/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   [INFO]
   [INFO] --- maven-assembly-plugin:3.1.1:single (default) @ aem-guides-wknd.ui.frontend ---
   [INFO] Reading assembly descriptor: assembly.xml
   [INFO] Building zip: /Users/dgordon/code/aem-guides-wknd/ui.frontend/target/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip
   [INFO]
   [INFO] --- maven-install-plugin:2.5.2:install (default-install) @ aem-guides-wknd.ui.frontend ---
   [INFO] Installing /Users/dgordon/code/aem-guides-wknd/ui.frontend/pom.xml to /Users/dgordon/.m2/repository/com/adobe/aem/guides/aem-guides-wknd.ui.frontend/0.0.1-SNAPSHOT/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.pom
   [INFO] Installing /Users/dgordon/code/aem-guides-wknd/ui.frontend/target/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip to /Users/dgordon/.m2/repository/com/adobe/aem/guides/aem-guides-wknd.ui.frontend/0.0.1-SNAPSHOT/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  13.520 s
   [INFO] Finished at: 2019-12-06T15:26:16-08:00
   ```

   Observe as linhas como `copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js`. Isso indica que o CSS e o JS compilados estão sendo copiados para a pasta `ui.apps`.

1. Visualização o carimbo de data e hora modificado para o arquivo `aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt`. Ele deve ser atualizado mais recentemente do que os outros arquivos no módulo `ui.apps`.

   Ao contrário dos outros módulos que observamos, o módulo **ui.frontende** não é implantado diretamente no AEM. Em vez disso, o CSS e o JS são copiados no módulo **ui.apps** e o módulo **ui.apps** é implantado no AEM. Se você observar a ordem de compilação desde o primeiro comando Maven, verá que **ui.frontendent** é sempre criado *antes* **ui.apps**.

   Mais tarde, no tutorial, vamos analisar os recursos avançados do módulo **ui.frontende** e do servidor de desenvolvimento de webpack incorporado para um desenvolvimento rápido.

## Inclusão dos componentes principais {#core-components}

O arquétipo incorpora automaticamente [AEM Componentes Principais](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html) no projeto. Anteriormente, ao inspecionar os pacotes implantados em AEM, vários pacotes relacionados aos Componentes principais foram incluídos. Os componentes principais são um conjunto de componentes básicos projetado para acelerar o desenvolvimento de um projeto da AEM Sites. Os Componentes principais são de código aberto e estão disponíveis em [GitHub](https://github.com/adobe/aem-core-wcm-components). Mais informações sobre como os Componentes principais estão [incluídos no projeto podem ser encontradas aqui](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html#core-components).

1. Usando seu editor de texto favorito, abra `aem-guides-wknd/pom.xml`.

1. Pesquisar `core.wcm.components.version`. Isso mostrará qual versão dos Componentes principais está incluída:

   ```xml
       <core.wcm.components.version>2.x.x</core.wcm.components.version>
   ```

   >[!NOTE]
   >
   > O AEM Project Archetype incluirá uma versão AEM Componentes principais, no entanto, esses projetos têm ciclos de lançamento diferentes e, portanto, a versão incluída dos Componentes principais pode não ser a mais recente. Como prática recomendada, você deve sempre procurar aproveitar a versão mais recente dos Componentes principais. Novos recursos e correções de erros são atualizados com frequência. As informações mais recentes da versão [podem ser encontradas no GitHub](https://github.com/adobe/aem-core-wcm-components/releases).

1. Se você rolar para baixo até a seção `dependencies`, verá as dependências individuais do Componente principal:

   ```xml
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.core</artifactId>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.content</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.config</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.examples</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   ```

## Gerenciamento do controle de origem {#source-control}

É sempre uma boa ideia usar alguma forma de controle de origem para gerenciar o código em seu aplicativo. Este tutorial usa git e GitHub. Há vários arquivos que são gerados pela Maven e/ou pelo IDE de escolha que devem ser ignorados pelo SCM.

O Maven criará uma pasta de públicos alvos sempre que você criar e instalar o pacote de códigos. A pasta e o conteúdo do público alvo devem ser excluídos do SCM.

Abaixo de ui.apps, você também observará muitos arquivos .content.xml criados. Esses arquivos XML mapeiam os tipos de nó e as propriedades do conteúdo instalado no JCR. Esses arquivos são críticos e **e não** devem ser ignorados.

O arquétipo de projeto AEM gerará um arquivo de amostra `.gitignore` que pode ser usado como um ponto de partida para o qual os arquivos podem ser ignorados com segurança. O arquivo é gerado em `<src>/aem-guides-wknd/.gitignore`.

## Análise {#chapter-review}

>[!VIDEO](https://video.tv.adobe.com/v/30153/?quality=12&learn=on)

## Parabéns! {#congratulations}

Parabéns, você acabou de criar seu primeiro Projeto AEM!

### Próximas etapas {#next-steps}

Entenda a tecnologia subjacente de um Componente do Adobe Experience Manager (AEM) Sites através de um exemplo simples `HelloWorld` com o tutorial [Informações básicas sobre componentes](component-basics.md).
