---
title: Configurar as ferramentas de desenvolvimento para o AEM como um Cloud Service development
description: Crie uma máquina de desenvolvimento local com todas as ferramentas básicas necessárias para desenvolver em relação a AEM local.
feature: Developer Tools
version: Cloud Service
kt: 4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '1435'
ht-degree: 2%

---


# Configurar ferramentas de desenvolvimento

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="Configurar ferramentas de desenvolvimento"
>abstract="O desenvolvimento do Adobe Experience Manager (AEM) requer que um conjunto mínimo de ferramentas de desenvolvimento seja instalado e configurado na máquina do desenvolvedor. Essas ferramentas incluem Java, Maven, Adobe I/O CLI, Development IDE e muito mais."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="Diretrizes de desenvolvimento"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="Noções básicas de desenvolvimento"

O desenvolvimento do Adobe Experience Manager (AEM) requer que um conjunto mínimo de ferramentas de desenvolvimento seja instalado e configurado na máquina do desenvolvedor. Estas ferramentas apoiam o desenvolvimento e a criação de projetos AEM.

Observe que `~` é usado como abreviado para o Diretório do usuário. No Windows, este é o equivalente a `%HOMEPATH%`.

## Instalar Java

O Experience Manager é um aplicativo Java e, portanto, requer o SDK do Java para suportar o desenvolvimento e o AEM como um SDK do Cloud Service.

1. [Baixe e instale a versão mais recente do SDK do Java 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Verifique se o SDK do Java 11 está instalado executando o comando:
   + Windows: `java -version`
   + macOS / Linux: `java --version`

![Java](./assets/development-tools/java.png)

## Instalar o Homebrew

_O uso do Homebrew é opcional, mas recomendado._

Homebrew é um gerenciador de pacotes de código aberto para macOS, Windows e Linux. Todas as ferramentas de suporte podem ser instaladas separadamente, o Homebrew fornece uma maneira conveniente de instalar e atualizar uma variedade de ferramentas de desenvolvimento necessárias para o desenvolvimento do Experience Manager.

1. Abra o terminal
1. Verifique se o Homebrew já está instalado executando o comando: `brew --version`.
1. Se o Homebrew não estiver instalado, instale o Homebrew
   + [Instale o Homebrew no macOS](https://brew.sh/)
      + Para homebrew no macOS, é necessário [Xcode](https://apps.apple.com/us/app/xcode/id497799835) ou [Ferramentas de Linha de Comando](https://developer.apple.com/download/more/), instalável por meio do comando:
         + `xcode-select --install`
   + [Instale o Homebrew no Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [Instale o Homebrew no Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. Verifique se o Homebrew está instalado executando o comando: `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

Se estiver usando o Homebrew, siga as instruções __Instalar usando o Homebrew__ nas seções abaixo. Se você estiver __not__ usando o Homebrew, instale as ferramentas usando os links específicos do SO.

## Instalar Git

[](https://git-scm.com/) Obtém o sistema de gerenciamento do controle de origem usado pelo  [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html) e, portanto, é necessário para o desenvolvimento.

+ Instalar o Git usando o Homebrew
   1. Abra o terminal/prompt de comando
   1. Execute o comando: `brew install git`
   1. Verifique se o Git está instalado, usando o comando: `git --version`
+ Ou, baixar e instalar o Git (macOS, Linux ou Windows)
   1. [Baixe e instale o Git](https://git-scm.com/downloads)
   1. Abra o terminal/prompt de comando
   1. Verifique se o Git está instalado, usando o comando: `git --version`

![Git](./assets/development-tools/git.png)

## Instalar o Node.js (e npm){#node-js}

[Node.](https://nodejs.org) jsis é um ambiente de tempo de execução JavaScript usado para trabalhar com os ativos de front-end de um projeto AEM  __ui.__ frontendsub. O Node.js é distribuído com [npm](https://www.npmjs.com/), é o gerenciador de pacotes Node.js defato, usado para gerenciar dependências do JavaScript.

+ Instalar o Node.js usando o Homebrew
   1. Abra o terminal/prompt de comando
   1. Execute o comando: `brew install node`
   1. Verifique se Node.js está instalado, usando o comando: `node -v`
   1. Verifique se o npm está instalado, usando o comando: `npm -v`
+ Ou baixe e instale o Node.js (macOS, Linux ou Windows)
   1. [Baixe e instale o Node.js](https://nodejs.org/en/download/)
   1. Abra o terminal/prompt de comando
   1. Verifique se Node.js está instalado, usando o comando: `node -v`
   1. Verifique se o npm está instalado, usando o comando: `npm -v`

![Node.js e npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM Projetos AEM baseados no Arquétipo](https://github.com/adobe/aem-project-archetype) de Projeto instalam uma versão isolada do Node.js no momento da criação. É bom manter a versão do sistema de desenvolvimento local sincronizada (ou próxima) das versões Node.js e npm especificadas no Reator pom.xml do projeto Maven AEM.
>
>Consulte este exemplo [AEM Project Reator pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) para onde localizar as versões de compilação Node.js e npm.

## Instalar o Maven

O Apache Maven é a ferramenta de linha de comando Java de código aberto usada para criar Projetos AEM gerados pelo Arquétipo Maven do Projeto AEM. Todos os principais IDEs ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Visual Studio Code](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/), etc.) têm suporte integrado para Maven.

+ Instale o Maven usando o Homebrew
   1. Abra o terminal/prompt de comando
   1. Execute o comando: `brew install maven`
   1. Verifique se o Maven está instalado, usando o comando: `mvn -v`
+ Ou, baixar e instalar o Maven (macOS, Linux ou Windows)
   1. [Baixar o Maven](https://maven.apache.org/download.cgi)
   1. [Instalar o Maven](https://maven.apache.org/install.html)
   1. Abra o terminal/prompt de comando
   1. Verifique se o Maven está instalado, usando o comando: `mvn -v`

![Maven](./assets/development-tools/maven.png)

## Configurar a CLI do Adobe I/O{#aio-cli}

O [Adobe I/O CLI](https://github.com/adobe/aio-cli), ou `aio`, fornece acesso de linha de comando a uma variedade de serviços da Adobe, incluindo [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) e [Asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). A CLI do Adobe I/O desempenha um papel integral no desenvolvimento no AEM como Cloud Service, pois oferece aos desenvolvedores a capacidade de:

+ Logs de email do AEM as a Cloud Services services
+ Gerenciar pipelines do Cloud Manager da CLI

### Instale a CLI do Adobe I/O

1. Verifique se [Node.js está instalado](#node-js), pois o Adobe I/O CLI é um módulo npm
   + Execute `node --version` para confirmar
1. Execute `npm install -g @adobe/aio-cli` para instalar o módulo `aio` npm globalmente

### Configurar o plugin Adobe I/O CLI Cloud Manager{#aio-cloud-manager}

O plug-in do Adobe I/O Cloud Manager permite que a CLI do aio interaja com o Adobe Cloud Manager por meio do comando `aio cloudmanager`.

1. Execute `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` para instalar o [plug-in do aio Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager).

### Configurar o plugin Adobe I/O CLI Asset compute{#aio-asset-compute}

O plug-in do Adobe I/O Cloud Manager permite que a CLI do aio gere e execute os trabalhadores do Asset compute por meio do comando `aio asset-compute`.

1. Execute `aio plugins:install @adobe/aio-cli-plugin-asset-compute` para instalar o [plug-in do Asset compute aio](https://github.com/adobe/aio-cli-plugin-asset-compute).

### Configurar a autenticação da CLI do Adobe I/O

Para que a CLI do Adobe I/O se comunique com o Cloud Manager, uma integração do [Cloud Manager deve ser criada no Adobe I/O Console](https://github.com/adobe/aio-cli-plugin-cloudmanager), e as credenciais devem ser obtidas para a autenticação bem-sucedida.

1. Faça logon em [console.adobe.io](https://console.adobe.io)
1. Certifique-se de que a organização que inclui o produto Cloud Manager ao qual se conectar esteja ativa no seletor de organização do Adobe
1. Criar um novo ou abrir um programa de Adobe I/O [existente](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Os programas do Console do Adobe I/O são simplesmente agrupamentos organizacionais de integrações, criar ou usar e programas existentes com base em como você deseja gerenciar suas integrações
   + Se criar um novo projeto, selecione &quot;Projeto vazio&quot;, se solicitado (vs. &quot;Criar de Modelo&quot;)
   + Os programas do console do Adobe I/O são conceitos diferentes dos programas do Cloud Manager
1. Crie uma nova integração da API do Cloud Manager com o perfil &quot;Desenvolvedor - Cloud Service&quot;
1. Obter as credenciais da Conta de Serviço (JWT) precisam preencher [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication) da CLI do Adobe I/O
1. Carregue o arquivo `config.json` na CLI do Adobe I/O
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. Carregue o arquivo `private.key` na CLI do Adobe I/O
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

Inicie [executando comandos](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) para o Cloud Manager por meio da CLI do Adobe I/O.

## Configurar o IDE de desenvolvimento

AEM desenvolvimento consiste principalmente no desenvolvimento de Java e Front-end (JavaScript, CSS etc) e no gerenciamento de XML. Veja a seguir os IDEs mais populares para desenvolvimento de AEM.

### IntelliJ IDEA

__[O IntelliJ ](https://www.jetbrains.com/idea/)__ IDEA é um IDE poderoso para o desenvolvimento do Java. O IntelliJ IDEA tem dois sabores, uma edição comunitária gratuita e uma versão Ultimate comercial (paga). A versão comunitária gratuita é suficiente para AEM desenvolvimento, no entanto, o Ultimate [expande seu conjunto de recursos](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [Baixar o IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [Baixe a ferramenta Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Código do Microsoft Visual Studio

__[O Visual Studio Code](https://code.visualstudio.com/)__  (Código VS) é uma ferramenta gratuita e de código aberto para desenvolvedores front-end. O Visual Studio Code pode ser configurado para integrar sincronização de conteúdo com AEM com a ajuda de uma ferramenta Adobe, __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

O Visual Studio Code é a escolha ideal para desenvolvedores front-end que criam código front-end principalmente; JavaScript, CSS e HTML. Embora o Código VS tenha suporte para Java por meio de [extensões](https://code.visualstudio.com/docs/java/java-tutorial), talvez não tenha alguns dos recursos avançados fornecidos por recursos mais específicos do Java.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Baixar código do Visual Studio](https://code.visualstudio.com/Download)
+ [Baixe a ferramenta Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Baixar extensão de código VS aprimorada](https://aemfed.io/)
+ [Baixar AEM Sincronizar extensão de código VS](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[O Eclipse ](https://www.eclipse.org/ide/)__ IDEs é um IDEs popular para desenvolvimento de Java e oferece suporte ao   __[AEM Developer ](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html?lang=en)__ Toolsplug-in fornecido pelo Adobe, fornecendo uma GUI no IDE para criação e para sincronizar o conteúdo JCR com uma instância de AEM local.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Baixar o Eclipse](https://www.eclipse.org/ide/)
+ [Download de ferramentas de desenvolvimento do Eclipse](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html?lang=en)
