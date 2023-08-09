---
title: Configurar as ferramentas de desenvolvimento para o desenvolvimento as a Cloud Service do AEM
description: Configurar uma máquina de desenvolvimento local com todas as ferramentas de linha de base necessárias para desenvolver localmente contra AEM.
feature: Developer Tools
version: Cloud Service
kt: 4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
source-git-commit: 9073c1d41c67ec654b232aea9177878f11793d07
workflow-type: tm+mt
source-wordcount: '1484'
ht-degree: 7%

---

# Configurar ferramentas de desenvolvimento {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="Configurar ferramentas de desenvolvimento"
>abstract="O desenvolvimento do Adobe Experience Manager (AEM) exige que um conjunto mínimo de ferramentas de desenvolvimento seja instalado e configurado na máquina do desenvolvedor. Essas ferramentas incluem Java, Maven, Adobe I/O CLI, Development IDE e muito mais."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=pt-BR" text="Diretrizes de desenvolvimento"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=pt-BR" text="Noções básicas de desenvolvimento"

O desenvolvimento do Adobe Experience Manager (AEM) exige que um conjunto mínimo de ferramentas de desenvolvimento seja instalado e configurado na máquina do desenvolvedor. Essas ferramentas apoiam o desenvolvimento e a construção de projetos de AEM.

Observe que `~` é usado como abreviação para o Diretório do usuário. No Windows, é equivalente a `%HOMEPATH%`.

## Instalar o Java

O Experience Manager é um aplicativo Java e, portanto, requer o SDK do Java para oferecer suporte ao desenvolvimento e ao SDK as a Cloud Service AEM.

1. [Baixe e instale o SDK Java 11 da versão mais recente](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=11)
1. Verifique se o SDK do Java 11 do Oracle está instalado executando o comando:

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

_O uso de Homebrew é opcional, mas recomendado._

Homebrew é um gerenciador de pacotes de código aberto para macOS, Windows e Linux. Todas as ferramentas de suporte podem ser instaladas separadamente, o Homebrew fornece uma maneira conveniente de instalar e atualizar uma variedade de ferramentas de desenvolvimento necessárias para o desenvolvimento de Experience Manager.

1. Abra o terminal
1. Verifique se o Homebrew já está instalado executando o comando: `brew --version`.
1. Se o Homebrew não estiver instalado, instale o Homebrew
   + [Instalar o Homebrew no macOS](https://brew.sh/)
      + Homebrew no macOS requer [Xcode](https://apps.apple.com/us/app/xcode/id497799835) ou [Ferramentas de Linha de Comando](https://developer.apple.com/download/more/), instalável por meio do comando:
         + `xcode-select --install`
   + [Instalar o Homebrew no Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [Instalar o Homebrew no Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. Verifique se o Homebrew está instalado executando o comando: `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

Se você estiver usando o Homebrew, siga as __Instalar usando o Homebrew__ instruções nas seções abaixo. Se você estiver __não__ usando o Homebrew, instale as ferramentas usando os links específicos do sistema operacional.

## Instalar o Git

[Git](https://git-scm.com/) é o sistema de gerenciamento de controle de origem usado pelo [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html)e, portanto, é necessário para o desenvolvimento.

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

[Node.js](https://nodejs.org) é um ambiente de tempo de execução JavaScript usado para trabalhar com os ativos de front-end de um projeto AEM __ui.frontend__ subprojeto. O Node.js é distribuído com o [npm](https://www.npmjs.com/), é o gerenciador de pacotes Node.js padrão, usado para gerenciar dependências do JavaScript.

>[!BEGINTABS]

>[!TAB Instalar o Node.js usando o Homebrew]

1. Abra o terminal/prompt de comando
1. Execute o comando: `$ brew install node`
1. Verifique se o Node.js está instalado usando o comando: `$ node -v`
1. Verifique se o npm está instalado usando o comando: `$ npm -v`

>[!TAB Baixe e instale o Node.js]

1. [Baixe e instale o Node.js](https://nodejs.org/en/download/)
2. Abra o terminal/prompt de comando
3. Verifique se o Node.js está instalado usando o comando: `$ node -v`
4. Verifique se o npm está instalado usando o comando: `$ npm -v`

>[!ENDTABS]

![Node.js e npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype)Projetos AEM baseados em instalam uma versão isolada do Node.js no momento da criação. É bom manter a versão do sistema de desenvolvimento local sincronizada (ou próxima) às versões Node.js e npm especificadas no Reator pom.xml do projeto AEM Maven.
>
>Veja este exemplo [Pom.xml do Reator de Projeto AEM](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) para onde localizar as versões de build do Node.js e npm.

## Instalar o Maven

O Apache Maven é a ferramenta de linha de comando Java de código aberto usada para construir projetos AEM gerados a partir do Arquétipo Maven do projeto AEM. Todos os IDEs principais ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Código do Visual Studio](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/), etc.) têm suporte para Maven integrado.


>[!BEGINTABS]

>[!TAB Instalar o Maven usando o Homebrew]

1. Abra o terminal/prompt de comando
1. Execute o comando: `$ brew install maven`
1. Verifique se o Maven está instalado, usando o comando: `$ mvn -v`

>[!TAB Baixar e instalar o Maven]

1. [Baixar Maven](https://maven.apache.org/download.cgi)
1. [Instalar o Maven](https://maven.apache.org/install.html)
1. Abra o terminal/prompt de comando
1. Verifique se o Maven está instalado, usando o comando: `$ mvn -v`

>[!ENDTABS]

![Maven](./assets/development-tools/maven.png)

## Configurar o Adobe I/O CLI{#aio-cli}

A variável [CLI do Adobe I/O](https://github.com/adobe/aio-cli)ou `aio`, fornece acesso de linha de comando a vários serviços da Adobe, incluindo [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) e [Asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). A CLI do Adobe I/O desempenha um papel integral no desenvolvimento do AEM as a Cloud Service, pois fornece aos desenvolvedores a capacidade de:

+ Registros finais de AEM como um serviço Cloud Services
+ Gerenciar pipelines do Cloud Manager a partir da CLI
+ Implantar em [Ambientes de desenvolvimento rápido AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html)

### Instale o Adobe I/O CLI

1. Assegurar [O Node.js está instalado](#node-js) como o Adobe I/O CLI é um módulo npm
   + Executar `node --version` para confirmar
1. Executar `npm install -g @adobe/aio-cli` para instalar o `aio` módulo npm globalmente

### Configurar o plug-in do Adobe I/O CLI Cloud Manager{#aio-cloud-manager}

O plug-in do Adobe I/O Cloud Manager permite que a CLI do aio interaja com o Adobe Cloud Manager por meio da `aio cloudmanager` comando.

1. Executar `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` para instalar o [Plug-in do aio Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager).

#### Configurar a autenticação da CLI do Adobe I/O

Para que a CLI do Adobe I/O se comunique com o Cloud Manager, um [A integração do Cloud Manager deve ser criada no Console do Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager)As credenciais do e do devem ser obtidas para a autenticação bem-sucedida.

1. Efetue logon no [console.adobe.io](https://console.adobe.io)
1. Verifique se a organização que inclui o produto Cloud Manager ao qual se conectar está ativa no alternador da organização Adobe
1. Criar um novo ou abrir um existente [programa Adobe I/O](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Os programas do Console do Adobe I/O são simplesmente agrupamentos organizacionais de integrações, criar ou usar e programas existentes com base em como você deseja gerenciar suas integrações
   + Se estiver criando um novo projeto, selecione &quot;Projeto vazio&quot; se solicitado (versus &quot;Criar a partir do modelo&quot;)
   + Os programas do console do Adobe I/O são conceitos diferentes dos programas do Cloud Manager
1. Crie uma nova integração da API do Cloud Manager com o perfil &quot;Desenvolvedor - Cloud Service&quot;
1. Obter as credenciais da conta de serviço (JWT) necessárias para preencher as CLIs do Adobe I/O [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)
1. Carregue o `config.json` arquivo na CLI do Adobe I/O
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. Carregue o `private.key` arquivo na CLI do Adobe I/O
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

Iniciar [execução de comandos](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) para o Cloud Manager por meio da CLI do Adobe I/O.

### Configurar o plug-in do Ambiente de desenvolvimento rápido para AEM{#rde}

O plug-in AEM Rapid Development Environment permite que o aio CLI interaja com o AEM as a Cloud Service [Ambientes de desenvolvimento rápido](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html) por meio da `aio aem:rde` comando.

1. Executar `aio plugins:install @adobe/aio-cli-plugin-aem-rde` para instalar o [Plug- in para ambientes de desenvolvimento rápido do AEMName](https://github.com/adobe/aio-cli-plugin-aem-rde).

### Configurar o plug-in de Asset compute CLI do Adobe I/O{#aio-asset-compute}

O plug-in do Adobe I/O Cloud Manager permite que a CLI aio gere e execute trabalhadores de Asset compute por meio do `aio asset-compute` comando.

1. Executar `aio plugins:install @adobe/aio-cli-plugin-asset-compute` para instalar o [plug-in de Asset compute aio](https://github.com/adobe/aio-cli-plugin-asset-compute).

## Configurar o IDE de desenvolvimento

O desenvolvimento do AEM consiste principalmente no desenvolvimento de Java e front-end (JavaScript, CSS etc.) e gerenciamento de XML. A seguir estão os IDEs mais populares para desenvolvimento de AEM.

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ O é um IDE poderoso para desenvolvimento em Java. IntelliJ IDEA vem em dois sabores, uma edição gratuita da Comunidade e uma versão comercial (paga) Ultimate. A versão gratuita da Comunidade é suficiente para o desenvolvimento do AEM, no entanto, o Ultimate [expande seu conjunto de recursos](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

+ [Baixar IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [Baixar a ferramenta Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Código do Microsoft Visual Studio

__[Código do Visual Studio](https://code.visualstudio.com/)__ O (VS Code) é uma ferramenta gratuita e de código aberto para desenvolvedores de front-end. O Visual Studio Code pode ser configurado para integrar a sincronização de conteúdo com o AEM com a ajuda de uma ferramenta Adobe, __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

O Visual Studio Code é a escolha ideal para desenvolvedores front-end que criam principalmente código front-end; JavaScript, CSS e HTML. Embora o código VS tenha suporte para Java via [extensões](https://code.visualstudio.com/docs/java/java-tutorial), pode faltar alguns dos recursos avançados fornecidos pelo mais específico do Java.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Baixar Código do Visual Studio](https://code.visualstudio.com/Download)
+ [Baixar a ferramenta Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Baixar extensão de código VS do aemfed](https://aemfed.io/)
+ [Baixar a extensão de código do AEM Sync VS](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ O é um IDEs popular para desenvolvimento em Java e oferece suporte ao  __[Ferramentas para desenvolvedores do AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)__ plug-in fornecido pelo Adobe, fornecendo uma interface gráfica interna do IDE para criação e para sincronizar o conteúdo JCR com uma instância local do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Baixar o Eclipse](https://www.eclipse.org/ide/)
+ [Download das ferramentas de desenvolvimento do Eclipse](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)
