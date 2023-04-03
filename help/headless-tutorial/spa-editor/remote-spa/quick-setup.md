---
title: Configuração rápida SPA Editor e SPA Remoto
description: Saiba como começar a usar um SPA remoto e AEM editor de SPA em 15 minutos!
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 5%

---

# Configuração rápida

A configuração rápida é uma apresentação rápida que ilustra como instalar e executar o aplicativo WKND e como um SPA remoto, e a cria usando AEM Editor SPA.

A configuração rápida leva você diretamente ao estado final deste tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_Uma apresentação em vídeo da configuração rápida_

## Pré-requisitos

Este tutorial requer o seguinte:

+ [SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v16+](https://nodejs.org/en/)
+ [npm v8+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Pré-requisitos somente do macOS
   + [Xcode](https://developer.apple.com/xcode/) ou [Ferramentas de linha de comando do Xcode](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip ou superior](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-chart-source code (ramificação: feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Este tutorial presume:

+ [Código Microsoft® Visual Studio](https://visualstudio.microsoft.com/) como o IDE
+ Um diretório de trabalho de `~/Code/wknd-app`
+ Execução do SDK do AEM como um serviço de autor em `http://localhost:4502`
+ Execução do SDK do AEM com o `admin` conta com senha `admin`
+ Execução do SPA em `http://localhost:3000`

## Inicie o Início Rápido do SDK AEM

Baixe e instale o AEM SDK Quickstart na porta 4502, com o padrão `admin/admin` credenciais.

1. [Baixar o SDK AEM mais recente](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Descompacte o SDK do AEM para `~/aem-sdk`
1. Execute o AEM SDK Quickstart Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK é iniciado e iniciado automaticamente em [http://localhost:4502](http://localhost:4502). Faça logon usando as seguintes credenciais:

+ Nome de usuário: `admin`
+ Senha: `admin`

## Baixe e instale o pacote de site WKND

Este tutorial tem uma dependência de __WKND 2.1.0+&#39;s__ projeto (para conteúdo).

1. [Baixe a versão mais recente de `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Faça logon AEM Gerenciador de pacotes do SDK em [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) com o `admin` credenciais.
1. __Upload__ o `aem-guides-wknd.all.x.x.x.zip` baixado na etapa 1
1. Toque no __Instalar__ botão para a entrada `aem-guides-wknd.all-x.x.x.zip`

## Baixe e instale pacotes de SPA de aplicativos WKND

Para executar uma configuração rápida, AEM pacotes são fornecidos aqui que contêm a configuração final de AEM e o conteúdo do tutorial.

1. [Download ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Download ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Faça logon AEM Gerenciador de pacotes do SDK em [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) com o `admin` credenciais.
1. __Upload__ o `wknd-app.all.x.x.x.zip` baixado na etapa 1
1. Toque no __Instalar__ botão para a entrada `wknd-app.all.x.x.x.zip`
1. __Upload__ o `wknd-app.ui.content.sample.x.x.x.zip` baixado na etapa 2
1. Toque no __Instalar__ botão para a entrada `wknd-app.ui.content.sample.x.x.x.zip`

## Baixar a fonte do aplicativo WKND

Baixe o código-fonte do aplicativo WKND em Github.com e alterne a ramificação que contém as alterações no SPA executado neste tutorial.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## Inicie o aplicativo SPA

Na raiz do projeto, instale as dependências do SPA projects npm e execute o aplicativo.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

Se houver erros ao executar `npm install` tente as seguintes etapas:

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Verifique se o SPA está em execução em [http://localhost:3000](http://localhost:3000).

## Criar conteúdo no Editor AEM SPA

Antes de criar conteúdo, organize as janelas do navegador de modo que o AEM Author (`http://localhost:4502`) está à esquerda e o SPA remoto (`http://localhost:3000`) é executado à direita. Esse acordo permite ver como as alterações no conteúdo de fonte AEM são refletidas imediatamente no SPA.

1. Faça logon em [Serviço de autor do SDK AEM](http://localhost:4502) as `admin`
1. Navegar para __Sites > Aplicativo WKND > eua > pt__
1. Editar __Página inicial do aplicativo WKND__
1. Mudar para __Editar__ modo

### Crie o componente fixo da exibição inicial

1. Toque no texto __Aventuras WKND__ para ativar o componente Título fixo (codificado na exibição Início do SPA)
1. Toque no __chave inglesa__ ícone na barra de ação do componente de Título
1. Altera o conteúdo do componente Título e salva
1. Atualize o SPA em execução em `http://localhost:3000` e observar que as alterações refletiram

### Crie o componente de contêiner da exibição inicial

1. Enquanto ainda edita o __Página inicial do aplicativo WKND__...
1. Expanda o __Barra lateral do Editor de SPA__ (à esquerda)
1. Toque no __Componentes__ ícones
1. Adicionar, alterar ou remover componentes do componente de contêiner que se encontra sob o logotipo WKND e acima do componente de Título fixo
1. Atualize o SPA em execução em `http://localhost:3000` e observar que as alterações refletiram

### Criar um componente de contêiner em uma rota dinâmica

1. Mudar para __Visualizar__ modo no Editor SPA
1. Toque em __Campo de Surf de Bali__ cartão e navegue até sua rota dinâmica
1. Adicionar, alterar ou remover componentes do componente de contêiner que os sites estão acima da __Itinerário__ título
1. Atualize o SPA em execução em `http://localhost:3000` e observar que as alterações refletiram

Novas páginas de AEM na __Página inicial do aplicativo WKND > Aventura__ _must_ tem um nome de página AEM que corresponda ao nome do Fragmento de conteúdo da aventura correspondente. Isso ocorre porque a rota SPA para AEM mapeamento de página é baseada no último segmento da rota, que é o nome do Fragmento de conteúdo.

## Parabéns!

Você acabou de experimentar rapidamente como AEM Editor SPA pode aprimorar seu SPA com áreas controladas e editáveis! Se você estiver interessado - confira o resto do tutorial, mas certifique-se de começar de novo, já que nesta configuração rápida, seu ambiente de desenvolvimento local agora está no estado final do tutorial!
