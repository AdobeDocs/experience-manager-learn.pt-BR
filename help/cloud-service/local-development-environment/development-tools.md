---
title: Configure as ferramentas de desenvolvimento para AEM como um desenvolvimento de Cloud Service
description: Configure uma máquina de desenvolvimento local com todas as ferramentas básicas necessárias para se desenvolver em relação ao AEM local.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4267
thumbnail: 25907.jpg
translation-type: tm+mt
source-git-commit: cb5f3c323c433c9321ba26ac1194be0cd225a405
workflow-type: tm+mt
source-wordcount: '1366'
ht-degree: 1%

---


# Configurar ferramentas de desenvolvimento

O desenvolvimento do Adobe Experience Manager (AEM) requer que um conjunto mínimo de ferramentas de desenvolvimento seja instalado e configurado na máquina do desenvolvedor. Estas ferramentas apoiam o desenvolvimento e a construção de projetos AEM.

Observe que `~` é usado como abreviação para o Diretório do usuário. No Windows, isso equivale a `%HOMEPATH%`.

## Instalar Java

O Experience Manager é um aplicativo Java e, portanto, requer que o Java SDK suporte o desenvolvimento e o AEM como um SDK Cloud Service.

1. [Baixe e instale a versão mais recente do Java 11 SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=lista&amp;p.offset=0&amp;p.limit=14)
1. Verifique se o Java 11 SDK está instalado executando o comando:
   + Windows: `java -version`
   + macOS / Linux: `java --version`

![Java](./assets/development-tools/java.png)

## Instalar o Homebrew

_O uso de Homebrew é opcional, mas recomendado._

Homebrew é um gerenciador de pacote de código aberto para macOS, Windows e Linux. Todas as ferramentas de suporte podem ser instaladas separadamente, o Homebrew fornece uma maneira conveniente de instalar e atualizar uma variedade de ferramentas de desenvolvimento necessárias para o desenvolvimento de Experience Manager.

1. Abrir o terminal
1. Verifique se o Homebrew já está instalado executando o comando: `brew --version`.
1. Se o Homebrew não estiver instalado, instale o Homebrew
   + [Instale o Homebrew no macOS](https://brew.sh/)
      + Homebrew no macOS requer [Xcode](https://apps.apple.com/us/app/xcode/id497799835) ou [Ferramentas de Linha de Comando](https://developer.apple.com/download/more/), instalável através do comando:
         + `xcode-select --install`
   + [Instale o Homebrew no Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [Instale o Homebrew no Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. Verifique se o Homebrew está instalado executando o comando: `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

Se estiver usando o Homebrew, siga as instruções de __Instalar usando o Homebrew__ nas seções abaixo. Se você __não__ estiver usando o Homebrew, instale as ferramentas usando os links específicos do SO.

## Instalar Git

[Gite ](https://git-scm.com/) o sistema de gerenciamento de controle de origem usado pelo  [Adobe Cloud Manager](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/requirements/source-code-repository.html) e, portanto, é necessário para o desenvolvimento.

+ Instale o Git usando o Homebrew
   1. Abra o terminal/prompt de comando
   1. Execute o comando: `brew install git`
   1. Verifique se o Git está instalado, usando o comando: `git --version`
+ Ou, baixe e instale o Git (macOS, Linux ou Windows)
   1. [Baixar e instalar o Git](https://git-scm.com/downloads)
   1. Abra o terminal/prompt de comando
   1. Verifique se o Git está instalado, usando o comando: `git --version`

![Git](./assets/development-tools/git.png)

## Instalar o Node.js (e npm){#node-js}

[Node.](https://nodejs.org) jsis um ambiente de tempo de execução JavaScript usado para trabalhar com os ativos front-end de um projeto AEM  __ui.__ frontendsub-projeto. Node.js é distribuído com [npm](https://www.npmjs.com/), é o gerenciador de pacote Node.js, usado para gerenciar dependências JavaScript.

+ Instalar o Node.js usando o Homebrew
   1. Abra o terminal/prompt de comando
   1. Execute o comando: `brew install node`
   1. Verifique se Node.js está instalado, usando o comando: `node -v`
   1. Verifique se npm está instalado, usando o comando: `npm -v`
+ Ou baixe e instale o Node.js (macOS, Linux ou Windows)
   1. [Baixe e instale o Node.js](https://nodejs.org/en/download/)
   1. Abra o terminal/prompt de comando
   1. Verifique se Node.js está instalado, usando o comando: `node -v`
   1. Verifique se npm está instalado, usando o comando: `npm -v`

![Node.js e npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM Projetos baseados no AEM Project Archetype](https://github.com/adobe/aem-project-archetype) instalam uma versão isolada do Node.js no momento da criação. É bom manter a versão do sistema de desenvolvimento local sincronizada (ou próxima) das versões Node.js e npm especificadas no AEM Maven project&#39;s Reator pom.xml.
>
>Consulte este exemplo [AEM Project Reator pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) para saber onde localizar as versões de compilação Node.js e npm.

## Instalar o Maven

O Apache Maven é a ferramenta de linha de comando Java de código aberto usada para criar AEM Projetos gerados pelo Arquétipo de Maven do Projeto AEM. Todos os principais IDEs ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Código do Visual Studio](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/), etc.) tenham suporte integrado ao Maven.

+ Instale o Maven usando o Homebrew
   1. Abra o terminal/prompt de comando
   1. Execute o comando: `brew install maven`
   1. Verifique se o Maven está instalado, usando o comando: `mvn -v`
+ Ou, baixe e instale o Maven (macOS, Linux ou Windows)
   1. [Baixar o Maven](https://maven.apache.org/download.cgi)
   1. [Instalar o Maven](https://maven.apache.org/install.html)
   1. Abra o terminal/prompt de comando
   1. Verifique se o Maven está instalado, usando o comando: `mvn -v`

![Maven](./assets/development-tools/maven.png)

## Configurar Adobe I/O CLI{#aio-cli}

O [Adobe I/O CLI](https://github.com/adobe/aio-cli), ou `aio`, fornece acesso de linha de comando a uma variedade de serviços de Adobe, incluindo [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) e [Asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). A CLI da Adobe I/O desempenha um papel integral no desenvolvimento da AEM como Cloud Service, pois oferece aos desenvolvedores a capacidade de:

+ Registros de caixa de AEM como serviços Cloud Services
+ Gerenciar pipelines do Cloud Manager da CLI

### Instale a CLI do Adobe I/O

1. Verifique se [Node.js está instalado](#node-js), pois a CLI do Adobe I/O é um módulo npm
   + Execute `node --version` para confirmar
1. Execute `npm install -g @adobe/aio-cli` para instalar o módulo `aio` npm globalmente

### Configurar o plug-in Adobe I/O CLI Cloud Manager{#aio-cloud-manager}

O plug-in do Adobe I/O Cloud Manager permite que a CLI do rádio interaja com o Adobe Cloud Manager por meio do comando `aio cloudmanager`.

1. Execute `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` para instalar o [plug-in do Gerenciador de nuvem do rádio](https://github.com/adobe/aio-cli-plugin-cloudmanager).

### Configurar o plug-in do Asset compute Adobe I/O CLI{#aio-asset-compute}

O plug-in do Adobe I/O Cloud Manager permite que a CLI do rádio gere e execute funcionários do Asset compute por meio do comando `aio asset-compute`.

1. Execute `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` para instalar o [plug-in do Asset compute aio](https://github.com/adobe/aio-cli-plugin-asset-compute).

### Configurar a autenticação CLI do Adobe I/O

Para que a CLI da Adobe I/O se comunique com o Cloud Manager, é necessário criar uma integração do Gerenciador de nuvem no Adobe I/O Console e obter credenciais para a autenticação bem-sucedida.

>[!VIDEO](https://video.tv.adobe.com/v/35094?quality=12&learn=on)

1. Faça logon em [console.adobe.io](https://console.adobe.io)
1. Certifique-se de que a organização que inclui o produto Cloud Manager ao qual se conectar esteja ativa no comutador Adobe Org
1. Crie um novo ou abra um programa Adobe I/O [existente](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Os programas do Adobe I/O Console são simplesmente agrupamentos organizacionais de integrações, criar ou usar e programas existentes com base em como você deseja gerenciar suas integrações
   + Se estiver criando um novo projeto, selecione &quot;Projeto vazio&quot; se solicitado (vs. Criar de modelo)
   + programas do Adobe I/O Console são conceitos diferentes para programas do Cloud Manager
1. Criar uma nova integração da API do Cloud Manager com o perfil &quot;Desenvolvedor - Cloud Service&quot;
1. Obtenha as credenciais da Conta de Serviço (JWT) necessárias para preencher o [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication) do Adobe I/O CLI
1. Carregue o arquivo `config.json` no Adobe I/O CLI
   + `$ aio config:set jwt-auth PATH_TO_CONFIG_JSON_FILE --file --json`
1. Carregue o arquivo `private.key` no Adobe I/O CLI
   + `$ aio config:set jwt-auth.jwt_private_key PATH_TO_PRIVATE_KEY_FILE --file`

Comece a [executar comandos](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) para o Cloud Manager por meio da CLI do Adobe I/O.

## Configurar o IDE de desenvolvimento

AEM desenvolvimento consiste principalmente em desenvolvimento Java e Front-end (JavaScript, CSS etc) e gerenciamento XML. Os seguintes são os IDEs mais populares para o desenvolvimento AEM.

### IntelliJ IDEA

__[O IntelliJ ](https://www.jetbrains.com/idea/)__ IDEA é um IDE poderoso para o desenvolvimento do Java. A IntelliJ IDEA é composta de dois sabores, uma edição comunitária gratuita e uma versão comercial (paga) Ultimate. A versão gratuita da Comunidade é suficiente para AEM desenvolvimento, no entanto, o Ultimate [expande seu conjunto de recursos](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [Baixar o IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [Download da ferramenta Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Código do Microsoft Visual Studio

__[Visual Studio Code](https://code.visualstudio.com/)__ (Código VS) é uma ferramenta gratuita de código aberto para desenvolvedores front-end. O código do Visual Studio pode ser configurado para integrar sincronização de conteúdo com AEM com a ajuda de uma ferramenta Adobe, __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

O código do Visual Studio é a escolha ideal para desenvolvedores front-end criando primeiramente código front-end; JavaScript, CSS e HTML. Embora o código VS tenha suporte para Java por meio de [extensões](https://code.visualstudio.com/docs/java/java-tutorial), pode faltar alguns dos recursos avançados fornecidos por mais específicos para Java.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Baixar código do Visual Studio](https://code.visualstudio.com/Download)
+ [Download da ferramenta Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Baixar extensão de código VS alimentada por email](https://aemfed.io/)
+ [Baixar AEM Sincronizar extensão de código VS](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[O Eclipse ](https://www.eclipse.org/ide/)__ IDEs é um popular IDEs para desenvolvimento Java e oferece suporte à   __[AEM ](https://eclipse.adobe.com/aem/dev-tools/)__ Toolsplug-in do desenvolvedor fornecido pelo Adobe, fornecendo uma GUI no IDE para criação e para sincronizar o conteúdo do JCR com uma instância AEM local.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Baixar Eclipse](https://www.eclipse.org/ide/)
+ [Baixar Ferramentas De Desenvolvimento Do Eclipse](https://eclipse.adobe.com/aem/dev-tools/)
