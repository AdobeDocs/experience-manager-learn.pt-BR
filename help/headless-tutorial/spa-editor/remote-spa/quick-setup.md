---
title: Configuração rápida do Editor de SPA e do SPA Remoto
description: Saiba como começar a trabalhar com um editor remoto de SPA e AEM SPA em 15 minutos!
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
duration: 715
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 2%

---

# Configuração rápida

A configuração rápida é uma apresentação rápida que ilustra como instalar e executar o aplicativo WKND e como um SPA remoto, além de criá-lo usando o editor SPA do AEM.

A configuração rápida leva você diretamente ao estado final deste tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_Uma apresentação em vídeo da configuração rápida_

## Pré-requisitos

Este tutorial requer o seguinte:

+ [SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Pré-requisitos somente para o macOS
   + [Xcode](https://developer.apple.com/xcode/) ou [Ferramentas de linha de comando do Xcode](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip ou superior](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql código-fonte (ramificação: feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Este tutorial pressupõe:

+ [Código do Microsoft® Visual Studio](https://visualstudio.microsoft.com/) como o IDE
+ Um diretório de trabalho de `~/Code/wknd-app`
+ Execução do SDK do AEM como um serviço do autor no `http://localhost:4502`
+ Execução do SDK do AEM com o local `admin` conta com senha `admin`
+ Executando o SPA `http://localhost:3000`

## Iniciar o Quickstart do SDK do AEM

Baixe e instale o Quickstart do SDK do AEM na porta 4502, com padrão `admin/admin` credenciais.

1. [Baixar o SDK do AEM mais recente](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Descompacte o SDK do AEM em `~/aem-sdk`
1. Execute o Quickstart Jar do SDK do AEM

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

O SDK do AEM é iniciado e iniciado automaticamente em [http://localhost:4502](http://localhost:4502). Faça logon usando as seguintes credenciais:

+ Nome de usuário: `admin`
+ Senha: `admin`

## Baixar e instalar o pacote do site WKND

Este tutorial depende do __WKND 2.1.0+__ projeto (para conteúdo).

1. [Baixe a versão mais recente de `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Faça logon no Gerenciador de pacotes do SDK do AEM em [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) com o `admin` credenciais.
1. __Carregar__ o `aem-guides-wknd.all.x.x.x.zip` baixado na etapa 1
1. Toque no __Instalar__ botão para a entrada `aem-guides-wknd.all-x.x.x.zip`

## Baixar e instalar pacotes SPA do aplicativo WKND

Para executar uma configuração rápida, são fornecidos aqui pacotes de AEM que contêm a configuração e o conteúdo finais do AEM do tutorial.

1. [Download ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Download ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Faça logon no Gerenciador de pacotes do SDK do AEM em [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) com o `admin` credenciais.
1. __Carregar__ o `wknd-app.all.x.x.x.zip` baixado na etapa 1
1. Toque no __Instalar__ botão para a entrada `wknd-app.all.x.x.x.zip`
1. __Carregar__ o `wknd-app.ui.content.sample.x.x.x.zip` baixado na etapa 2
1. Toque no __Instalar__ botão para a entrada `wknd-app.ui.content.sample.x.x.x.zip`

## Baixar a origem do aplicativo WKND

Baixe o código-fonte do aplicativo WKND pelo em Github.com e alterne a ramificação que contém as alterações no SPA executadas neste tutorial.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## Iniciar o aplicativo SPA

Na raiz do projeto, instale as dependências npm dos projetos SPA e execute o aplicativo.

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

## Conteúdo de autor no editor SPA AEM

Antes de criar conteúdo, organize as janelas do navegador de modo que o AEM Author (`http://localhost:4502`) está à esquerda e o SPA remoto (`http://localhost:3000`) é executado à direita. Essa disposição permite ver como as alterações no conteúdo originado no AEM são refletidas imediatamente no SPA.

1. Efetue logon no [Serviço de autor do SDK do AEM](http://localhost:4502) as `admin`
1. Navegue até __Sites > Aplicativo WKND > br > pt-BR__
1. Editar __Página inicial do aplicativo WKND__
1. Alternar para __Editar__ modo

### Criar o componente fixo da visualização inicial

1. Toque no texto __Aventuras WKND__ para ativar o componente de Título fixo (codificado na exibição Início do SPA)
1. Toque no __chave inglesa__ ícone na barra de ação do componente Título
1. Altera o conteúdo do componente de Título e salva
1. Atualizar o SPA em execução `http://localhost:3000` e veja se as alterações foram refletidas

### Criar o componente de contêiner de exibição da Página inicial

1. Ao editar o __Página inicial do aplicativo WKND__..
1. Expanda a __Barra lateral do editor de SPA__ (à esquerda)
1. Toque no __Componentes__ ícones
1. Adicionar, alterar ou remover componentes do componente de contêiner que está abaixo do logotipo WKND e acima do componente de Título fixo
1. Atualizar o SPA em execução `http://localhost:3000` e veja se as alterações foram refletidas

### Criar um componente de contêiner em uma rota dinâmica

1. Alternar para __Visualizar__ no Editor de SPA
1. Toque no __Campo de Surf de Bali__ e navegue até o seu roteiro dinâmico
1. Adicionar, alterar ou remover componentes do componente de contêiner que está acima de __Itinerário__ cabeçalho
1. Atualizar o SPA em execução `http://localhost:3000` e veja se as alterações foram refletidas

Novas páginas AEM sob o __Página inicial do aplicativo WKND > Aventura__ _deve_ têm um nome de página AEM que corresponde ao nome do Fragmento de conteúdo da aventura correspondente. Isso ocorre porque o mapeamento da rota do SPA para a página AEM se baseia no último segmento da rota, que é o nome do Fragmento de conteúdo.

## Parabéns.

Você tem uma ideia rápida de como o Editor de SPA AEM pode melhorar seu SPA com áreas controladas e editáveis! Se você estiver interessado, confira o resto do tutorial, mas não deixe de começar do zero, já que nesta configuração rápida seu ambiente de desenvolvimento local agora está no estado final do tutorial!
