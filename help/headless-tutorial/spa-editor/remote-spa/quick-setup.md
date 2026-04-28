---
title: Configuração rápida do Editor de SPA e SPA Remoto
description: Saiba como começar a usar um SPA remoto e um Editor SPA do AEM em 15 minutos!
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer
level: Beginner
jira: KT-7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
duration: 647
hide: true
source-git-commit: f95907146983d2315d48f793d38ebb1172a7bae4
workflow-type: tm+mt
source-wordcount: '838'
ht-degree: 13%

---

# Configuração rápida

{{spa-editor-deprecation}}

A configuração rápida é uma apresentação rápida que ilustra como instalar e executar o aplicativo WKND como um SPA remoto, e a cria usando o Editor SPA do AEM.

A configuração rápida leva você diretamente ao estado final deste tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_Apresentação em vídeo da configuração rápida_

## Pré-requisitos

Este tutorial requer o seguinte:

+ [SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v18](https://nodejs.org/pt)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Pré-requisitos somente para o macOS
   + [Xcode](https://developer.apple.com/xcode/) ou [Ferramentas de linha de comando Xcode](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip ou superior](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql código-fonte (ramificação: feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Este tutorial pressupõe que você possui:

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) como IDE
+ Um diretório de trabalho de `~/Code/wknd-app`
+ Execução do SDK do AEM como um serviço de criação no `http://localhost:4502`
+ Executar o SDK do AEM com a conta `admin` local e a senha `admin`
+ Execução do SPA em `http://localhost:3000`

## Iniciar o AEM SDK Quickstart

Baixe e instale o AEM SDK Quickstart na porta 4502, com as credenciais `admin/admin` padrão.

1. [Baixe o AEM SDK mais recente](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=1)
1. Descompacte o SDK do AEM em `~/aem-sdk`
1. Execute o AEM SDK Quickstart Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

O AEM SDK é iniciado e iniciado automaticamente em [http://localhost:4502](http://localhost:4502). Log in using the following credentials:

+ Username: `admin`
+ Password: `admin`

## Download and install WKND Site package

This tutorial has a dependency on __WKND 2.1.0+&#39;s__ project (for content).

1. [Download the latest version of `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Log in to AEM SDK&#39;s Package Manager at [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) with the `admin` credentials.
1. __Upload__ the `aem-guides-wknd.all.x.x.x.zip` downloaded in step 1
1. Tap the __Install__ button for the entry `aem-guides-wknd.all-x.x.x.zip`

## Download and install WKND App SPA packages

To perform a quick setup, AEM packages are provided here that contain the tutorial&#39;s final  AEM configuration and content.

1. [Download `wknd-app.all.x.x.x.zip`](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Download `wknd-app.ui.content.sample.x.x.x.zip`](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Log in to AEM SDK&#39;s Package Manager at [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) with the `admin` credentials.
1. __Upload__ the `wknd-app.all.x.x.x.zip` downloaded in step 1
1. Tap the __Install__ button for the entry `wknd-app.all.x.x.x.zip`
1. __Upload__ the `wknd-app.ui.content.sample.x.x.x.zip` downloaded in step 2
1. Tap the __Install__ button for the entry `wknd-app.ui.content.sample.x.x.x.zip`

## Download the WKND App source

Download the WKND App&#39;s source code by from Github.com, and switch the branch containing the changes to the SPA performed in this tutorial.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## Start the SPA application

From the project&#39;s root, install the SPA projects npm dependencies and run the application.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

If there are errors when running `npm install` try the following steps:

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Verify that the SPA is running at [http://localhost:3000](http://localhost:3000).

## Author content in AEM SPA Editor

Antes de criar o conteúdo, organize as janelas do navegador de forma que o Autor do AEM (`http://localhost:4502`) fique à esquerda e o SPA remoto (`http://localhost:3000`) seja executado à direita. Essa organização permite ver como as alterações no conteúdo de origem do AEM são refletidas imediatamente no SPA.

1. Faça logon no [Serviço de Autor do AEM SDK](http://localhost:4502) como `admin`
1. Navegue até __Sites > Aplicativo WKND > us > en__
1. Editar __Página Inicial do Aplicativo WKND__
1. Alternar para o modo __Editar__

### Criar o componente fixo da visualização inicial

1. Toque no texto __Aventuras WKND__ para ativar o componente de Título fixo (codificado na exibição Início do SPA)
1. Toque no ícone __chave inglesa__ na barra de ação do componente Título
1. Altera o conteúdo do componente de Título e salva
1. Atualize o SPA em execução em `http://localhost:3000` e veja se as alterações foram refletidas

### Criar o componente de contêiner de exibição da Página inicial

1. Ao editar a __Página inicial do aplicativo WKND__...
1. Expanda a __barra lateral do Editor SPA__ (à esquerda)
1. Toque nos ícones __Componentes__
1. Adicionar, alterar ou remover componentes do componente de contêiner que está abaixo do logotipo WKND e acima do componente de Título fixo
1. Atualize o SPA em execução em `http://localhost:3000` e veja se as alterações foram refletidas

### Criar um componente de contêiner em uma rota dinâmica

1. Alternar para o modo __Visualização__ no Editor SPA
1. Toque no cartão __Campo de Surf de Bali__ e navegue até sua rota dinâmica
1. Adicionar, alterar ou remover componentes do componente de contêiner que está acima do cabeçalho __Itinerário__
1. Atualize o SPA em execução em `http://localhost:3000` e veja se as alterações foram refletidas

As novas páginas do AEM na __Página inicial do aplicativo WKND > Aventura__ _devem_ ter um nome de página do AEM que corresponda ao nome do Fragmento de conteúdo da aventura correspondente. Isso ocorre porque a rota de SPA para o mapeamento de página do AEM se baseia no último segmento da rota, que é o nome do Fragmento de conteúdo.

## Parabéns!

Você tem uma ideia rápida de como o AEM SPA Editor pode aprimorar seu SPA com áreas controladas e editáveis! If you&#39;re interested - check out the rest of the tutorial, but make sure to start fresh, since in this quick setup your local development environment is now in  end state of the tutorial!
