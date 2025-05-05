---
title: Configurar as ferramentas de desenvolvimento do AEM as a Cloud Service
description: Configure uma máquina de desenvolvimento local com todas as ferramentas de linha de base necessárias para desenvolver localmente em relação ao AEM.
feature: Developer Tools
version: Experience Manager as a Cloud Service
jira: KT-4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
duration: 3508
source-git-commit: 3ad201aad77e71b42d46d69fdda50bcc77316151
workflow-type: tm+mt
source-wordcount: '1302'
ht-degree: 6%

---

# Configurar ferramentas de desenvolvimento {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="Configurar ferramentas de desenvolvimento"
>abstract="O desenvolvimento do Adobe Experience Manager (AEM) exige que um conjunto mínimo de ferramentas de desenvolvimento seja instalado e configurado na máquina do desenvolvedor. Essas ferramentas incluem Java, Maven, Adobe I/O CLI, Development IDE e muito mais."
>additional-url="https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines" text="Diretrizes de desenvolvimento"
>additional-url="https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk" text="Noções básicas de desenvolvimento"

O desenvolvimento do Adobe Experience Manager (AEM) exige que um conjunto mínimo de ferramentas de desenvolvimento seja instalado e configurado na máquina do desenvolvedor. Essas ferramentas apoiam o desenvolvimento e a criação de projetos do AEM.

Observe que `~` é usado como abreviação para o Diretório do Usuário. No Windows, é equivalente a `%HOMEPATH%`.

## Instalar o Java

O Experience Manager é um aplicativo Java e, portanto, requer o Java SDK para oferecer suporte ao desenvolvimento e ao AEM as a Cloud Service SDK.

1. [Baixe e instale a versão mais recente do Java 11 SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Verifique se o Oracle Java 11 SDK está instalado executando o comando:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/development-tools/java.png)

## Instalar Homebrew

_O uso do Homebrew é opcional, mas recomendado._

Homebrew é um gerenciador de pacotes de código aberto para macOS, Windows e Linux. Todas as ferramentas de suporte podem ser instaladas separadamente, o Homebrew fornece uma maneira conveniente de instalar e atualizar uma variedade de ferramentas de desenvolvimento necessárias para o desenvolvimento do Experience Manager.

1. Abra o terminal
1. Verifique se o Homebrew já está instalado executando o comando: `brew --version`.
1. Se o Homebrew não estiver instalado, instale o Homebrew

>[!BEGINTABS]

>[!TAB macOS]

O [Homebrew no macOS](https://brew.sh/) requer o [Xcode](https://apps.apple.com/us/app/xcode/id497799835) ou as [Ferramentas de Linha de Comando](https://developer.apple.com/download/more/), instaláveis por meio do comando:

```shell
$ xcode-select --install
```

>[!TAB Windows]

[Instalar o Homebrew no Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!TAB Linux]

[Instalar o Homebrew no Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!ENDTABS]

1. Verifique se o Homebrew está instalado executando o comando: `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

Se você estiver usando o Homebrew, siga as instruções de __Instalar usando o Homebrew__ nas seções abaixo. Se você estiver __não__ usando o Homebrew, instale as ferramentas usando os links específicos do sistema operacional.

## Instalar o Git

O [Git](https://git-scm.com/) é o sistema de gerenciamento de controle do código-fonte usado pelo [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html?lang=pt-BR) e, portanto, é necessário para o desenvolvimento.

>[!BEGINTABS]

>[!TAB Instalar o Git usando o Homebrew]

1. Abra o terminal/prompt de comando
1. Execute o comando: `$ brew install git`
1. Verifique se o Git está instalado usando o comando: `$ git --version`

>[!TAB Baixar e instalar o Git]

1. [Baixar e instalar o Git](https://git-scm.com/downloads)
1. Abra o terminal/prompt de comando
1. Verifique se o Git está instalado usando o comando: `$ git --version`

>[!ENDTABS]

![Git](./assets/development-tools/git.png)

## Instalar Node.js (e npm){#node-js}

[Node.js](https://nodejs.org) é um ambiente de tempo de execução do JavaScript usado para trabalhar com os ativos front-end de um subprojeto __ui.frontend__ de um projeto do AEM. O Node.js é distribuído com [npm](https://www.npmjs.com/), é o gerenciador de pacotes Node.js padrão, usado para gerenciar dependências do JavaScript.

>[!BEGINTABS]

>[!TAB Instalar Node.js usando Homebrew]

1. Abra o terminal/prompt de comando
1. Execute o comando: `$ brew install node`
1. Verifique se Node.js está instalado, usando o comando: `$ node -v`
1. Verifique se o npm está instalado, usando o comando: `$ npm -v`

>[!TAB Baixar e instalar Node.js]

1. [Baixar e instalar Node.js](https://nodejs.org/en/download/)
2. Abra o terminal/prompt de comando
3. Verifique se Node.js está instalado, usando o comando: `$ node -v`
4. Verifique se o npm está instalado, usando o comando: `$ npm -v`

>[!ENDTABS]

![Node.js e npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>Os Projetos AEM baseados no [Arquétipo de projeto do AEM](https://github.com/adobe/aem-project-archetype) instalam uma versão isolada do Node.js no momento da compilação. É bom manter a versão do sistema de desenvolvimento local sincronizada (ou próxima) às versões Node.js e npm especificadas no arquivo pom.xml do Reator do projeto AEM Maven.
>
>Consulte este exemplo [pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) do Reator de Projeto do AEM para saber onde localizar as versões de compilação de Node.js e npm.

## Instalar o Maven

O Apache Maven é a ferramenta de linha de comando Java de código aberto usada para criar projetos do AEM gerados a partir do Arquétipo Maven do projeto do AEM. Todos os principais IDEs ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Visual Studio Code](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/), etc.) têm suporte para Maven integrado.


>[!BEGINTABS]

>[!TAB Instalar o Maven usando o Homebrew]

1. Abra o terminal/prompt de comando
1. Execute o comando: `$ brew install maven`
1. Verifique se o Maven está instalado, usando o comando: `$ mvn -v`

>[!TAB Baixar e instalar o Maven]

1. [Baixar Maven](https://maven.apache.org/download.cgi)
1. [Instalar Maven](https://maven.apache.org/install.html)
1. Abra o terminal/prompt de comando
1. Verifique se o Maven está instalado, usando o comando: `$ mvn -v`

>[!ENDTABS]

![Maven](./assets/development-tools/maven.png)

## Configurar a CLI do Adobe I/O{#aio-cli}

A [CLI do Adobe I/O](https://github.com/adobe/aio-cli) ou `aio` fornece acesso de linha de comando a diversos serviços da Adobe, incluindo o [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) e o [Asset Compute](https://github.com/adobe/aio-cli-plugin-asset-compute). A CLI do Adobe I/O desempenha um papel integral no desenvolvimento no AEM as a Cloud Service, pois fornece aos desenvolvedores a capacidade de:

+ Logs finais dos serviços do AEM as a Cloud Services
+ Gerenciar pipelines do Cloud Manager a partir da CLI
+ Implantar em [AEM Rapid Development Environments](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=pt-BR)

### Instalar a CLI do Adobe I/O

1. Verifique se o [Node.js está instalado](#node-js), pois a CLI do Adobe I/O é um módulo npm
   + Execute `node --version` para confirmar
1. Executar `npm install -g @adobe/aio-cli` para instalar o módulo `aio` npm globalmente

### Configurar o plug-in do Cloud Manager da CLI do Adobe I/O{#aio-cloud-manager}

O plug-in Adobe I/O Cloud Manager permite que a CLI aio interaja com o Adobe Cloud Manager por meio do comando `aio cloudmanager`.

1. Execute `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` para instalar o [plug-in aio do Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager).

#### Configurar a autenticação da CLI do Adobe I/O

Para que a CLI do Adobe I/O se comunique com o Cloud Manager, uma [integração do Cloud Manager deve ser criada no Console do Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager), e as credenciais devem ser obtidas para autenticar com êxito.

1. Faça logon em [console.adobe.io](https://console.adobe.io)
1. Verifique se a sua organização, que inclui o produto Cloud Manager ao qual se conectar, está ativa no alternador da organização da Adobe
1. Criar um novo ou abrir um [programa Adobe I/O](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md) existente
   + Os projetos do Adobe I/O Console são simplesmente agrupamentos organizacionais de integrações, como criar ou usar, e projetos existentes com base em como você deseja gerenciar suas integrações.
   + Se estiver criando um novo projeto, selecione &quot;Projeto vazio&quot; se solicitado (versus &quot;Criar a partir do modelo&quot;)
   + Os programas do Adobe I/O Console são conceitos diferentes dos programas do Cloud Manager
1. Criar uma nova integração da API do Cloud Manager
   + Selecione o tipo de credencial &quot;Oauth Server-to-server&quot;.
   + Selecione o perfil de produto &quot;Gerente de implantação - Cloud Service&quot;.
   + Salvar API configurada
1. Obter as credenciais precisa preencher o [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication) da CLI do Adobe I/O, abrindo as credenciais recém-criadas de &quot;Servidor para servidor OAuth&quot; e selecionando &quot;Baixar JSON&quot; na barra de ação superior direita.
1. Abra o arquivo JSON baixado e renomeado todas as chaves para minúsculas. Por exemplo, `CLIENT_ID` torna-se `client_id`.
1. Carregue o arquivo `config.json` na CLI do Adobe I/O
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager /path/to/downloaded/json --file --json`

Comece a [executar comandos](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) para o Cloud Manager por meio da CLI do Adobe I/O.

### Configurar o plug-in do Ambiente de desenvolvimento rápido do AEM{#rde}

O plug-in Ambiente de desenvolvimento rápido da AEM permite que a CLI do aio interaja com os [Ambientes de desenvolvimento rápido](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=pt-BR) da AEM as a Cloud Service através do comando `aio aem:rde`.

1. Execute `aio plugins:install @adobe/aio-cli-plugin-aem-rde` para instalar o [plug-in do AEM Rapid Development Environments](https://github.com/adobe/aio-cli-plugin-aem-rde).

### Configurar o plug-in do Asset Compute da CLI do Adobe I/O{#aio-asset-compute}

O plug-in Adobe I/O Cloud Manager permite que a CLI aio gere e execute trabalhadores do Asset Compute por meio do comando `aio asset-compute`.

1. Execute `aio plugins:install @adobe/aio-cli-plugin-asset-compute` para instalar o [plug-in aio do Asset Compute](https://github.com/adobe/aio-cli-plugin-asset-compute).

## Configurar o IDE de desenvolvimento

O desenvolvimento do AEM consiste principalmente no desenvolvimento de Java e front-end (JavaScript, CSS etc.) e gerenciamento de XML. A seguir estão os IDEs mais populares para o desenvolvimento do AEM.

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ é um IDE poderoso para desenvolvimento em Java. O IntelliJ IDEA vem em duas versões: uma edição gratuita da Comunidade e uma versão comercial (paga) do Ultimate. A versão gratuita da Comunidade é suficiente para o desenvolvimento de AEM. No entanto, o Ultimate [expande seu conjunto de recursos](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

+ [Baixar IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [Baixe a ferramenta Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Código do Microsoft Visual Studio

O __[Visual Studio Code](https://code.visualstudio.com/)__ (VS Code) é uma ferramenta gratuita e de código aberto para desenvolvedores front-end. O Visual Studio Code pode ser configurado para integrar a sincronização de conteúdo com o AEM com a ajuda de uma ferramenta do Adobe, o __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

O Visual Studio Code é a escolha ideal para desenvolvedores front-end que criam principalmente código front-end; JavaScript, CSS e HTML. Embora o Código VS tenha suporte para Java através de [extensões](https://code.visualstudio.com/docs/java/java-tutorial), ele pode não ter alguns dos recursos avançados fornecidos por outros específicos do Java.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Baixar Código Visual Studio](https://code.visualstudio.com/Download)
+ [Baixe a ferramenta Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Baixar extensão de código VS do AEM Sync](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

O __[Eclipse IDE](https://www.eclipse.org/ide/)__ é um IDE popular para desenvolvimento em Java, e oferece suporte ao plug-in __[Ferramentas de Desenvolvedor do AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=pt-BR)__ fornecido pelo Adobe, fornecendo uma GUI no IDE para criação e para sincronizar conteúdo JCR com uma instância local do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Baixar o Eclipse](https://www.eclipse.org/ide/)
+ [Baixar as Ferramentas de Desenvolvimento do Eclipse](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=pt-BR)
