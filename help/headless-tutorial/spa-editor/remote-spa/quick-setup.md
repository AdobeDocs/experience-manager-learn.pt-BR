---
title: Configuração rápida do Editor de SPA e SPA Remoto
description: Saiba como começar a usar um SPA remoto e um Editor SPA do AEM em 15 minutos!
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
duration: 647
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 10%

---

# Configuração rápida

{{spa-editor-deprecation}}

A configuração rápida é uma apresentação rápida que ilustra como instalar e executar o aplicativo WKND como um SPA remoto, e a cria usando o Editor SPA do AEM.

A configuração rápida leva você diretamente ao estado final deste tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_Apresentação em vídeo da configuração rápida_

## Pré-requisitos

Este tutorial requer o seguinte:

+ [SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=pt-BR)
+ [Node.js v18](https://nodejs.org/pt)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Pré-requisitos somente para o macOS
   + [Xcode](https://developer.apple.com/xcode/) ou [Ferramentas de linha de comando Xcode](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip ou superior](https://github.com/adobe/aem-guides-wknd/releases)
+ [código-fonte aem-guides-wknd-graphql (ramificação: recurso/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Este tutorial pressupõe que você possui:

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) como IDE
+ Um diretório de trabalho de `~/Code/wknd-app`
+ Execução do SDK do AEM como um serviço de criação no `http://localhost:4502`
+ Executar o SDK do AEM com a conta `admin` local e a senha `admin`
+ Execução do SPA em `http://localhost:3000`

## Iniciar o AEM SDK Quickstart

Baixe e instale o AEM SDK Quickstart na porta 4502, com as credenciais `admin/admin` padrão.

1. [Baixar o AEM SDK mais recente](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=1)
1. Descompacte o SDK do AEM em `~/aem-sdk`
1. Execute o AEM SDK Quickstart Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

O AEM SDK é iniciado e iniciado automaticamente em [http://localhost:4502](http://localhost:4502). Faça logon usando as seguintes credenciais:

+ Nome de Usuário: `admin`
+ Senha: `admin`

## Baixar e instalar o pacote do site WKND

Este tutorial tem uma dependência no projeto __WKND 2.1.0+__ (para conteúdo).

1. [Baixar a última versão de `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Faça logon no Gerenciador de Pacotes do AEM SDK em [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) com as credenciais `admin`.
1. __Carregar__ o `aem-guides-wknd.all.x.x.x.zip` baixado na etapa 1
1. Toque no botão __Instalar__ para a entrada `aem-guides-wknd.all-x.x.x.zip`

## Baixe e instale pacotes de SPA do aplicativo WKND

Para executar uma configuração rápida, são fornecidos aqui pacotes AEM com a configuração e o conteúdo finais do AEM do tutorial.

1. [Download ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Download ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Faça logon no Gerenciador de Pacotes do AEM SDK em [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) com as credenciais `admin`.
1. __Carregar__ o `wknd-app.all.x.x.x.zip` baixado na etapa 1
1. Toque no botão __Instalar__ para a entrada `wknd-app.all.x.x.x.zip`
1. __Carregar__ o `wknd-app.ui.content.sample.x.x.x.zip` baixado na etapa 2
1. Toque no botão __Instalar__ para a entrada `wknd-app.ui.content.sample.x.x.x.zip`

## Baixar a origem do aplicativo WKND

Baixe o código-fonte do aplicativo WKND pelo em Github.com e alterne a ramificação que contém as alterações no SPA executadas neste tutorial.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## Iniciar o aplicativo SPA

Na raiz do projeto, instale as dependências npm dos projetos de SPA e execute o aplicativo.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

Se houver erros ao executar `npm install`, tente as seguintes etapas:

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Verifique se o SPA está em execução em [http://localhost:3000](http://localhost:3000).

## Conteúdo de autor no Editor SPA do AEM

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

Você tem uma ideia rápida de como o AEM SPA Editor pode aprimorar seu SPA com áreas controladas e editáveis! Se você estiver interessado, confira o resto do tutorial, mas não deixe de começar do zero, já que nesta configuração rápida seu ambiente de desenvolvimento local agora está no estado final do tutorial!
