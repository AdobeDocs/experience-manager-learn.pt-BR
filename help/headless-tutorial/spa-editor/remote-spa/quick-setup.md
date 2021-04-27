---
title: Configuração rápida SPA Editor e SPA Remoto
description: Saiba como começar a usar um SPA remoto e AEM editor de SPA em 15 minutos!
topic: Sem periféricos, SPA, desenvolvimento
feature: Editor de SPA, Componentes principais, APIs, Desenvolvimento
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: kt-7629.jpeg
translation-type: tm+mt
source-git-commit: ba45a52b9dac44f0c08c819cf4778dba83f325c9
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 5%

---


# Configuração rápida

A configuração rápida é uma apresentação rápida que ilustra como instalar e executar o aplicativo WKND e como um SPA remoto, e a cria usando AEM Editor SPA.

A configuração rápida leva você diretamente ao estado final deste tutorial.

## Pré-requisitos

Este tutorial requer o seguinte:

+ [SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip ou superior](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql source code](https://github.com/adobe/aem-guides-wknd-graphql)

Este tutorial presume:

+ [Microsoft® Visual Studio ](https://visualstudio.microsoft.com/) Codeas o IDE
+ Um diretório de trabalho de `~/Code/wknd-app`
+ Executar o SDK do AEM como um serviço de Autor em `http://localhost:4502`
+ Execução do SDK AEM com a conta local `admin` com a senha `admin`
+ Execução do SPA em `http://localhost:3000`

## Inicie o Início Rápido do SDK AEM

Baixe e instale o AEM SDK Quickstart na porta 4502, com credenciais `admin/admin` padrão.

1. [Baixar o SDK AEM mais recente](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Descompacte o SDK do AEM para `~/aem-sdk`
1. Execute o AEM SDK Quickstart Jar

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK iniciará e iniciará automaticamente em [http://localhost:4502](http://localhost:4502). Faça logon usando as seguintes credenciais:

+ Nome de usuário: `admin`
+ Senha: `admin`

## Baixe e instale o pacote de site WKND

Este tutorial tem uma dependência no projeto __WKND 0.3.0+__ (para conteúdo).

1. [Baixe a versão mais recente de  `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Faça logon AEM Gerenciador de pacotes do SDK em [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) com as credenciais `admin`.
1. ____ Faça o upload do  `aem-guides-wknd.all.x.x.x.zip` download na etapa 1
1. Toque no botão __Instalar__ para a entrada `aem-guides-wknd.all-x.x.x.zip`

## Baixe e instale pacotes de SPA de aplicativos WKND

Para executar uma configuração rápida, são fornecidos pacotes de AEM que contêm a configuração final de AEM do tutorial e o conteúdo.

1. [Download `wknd-app.all.x.x.x.zip`](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Baixar  `wknd-app.ui.content.sample.x.x.x.zip`](./assets/quick-setup/wknd-app.ui.content.sample-1.0.0.zip)
1. Faça logon AEM Gerenciador de pacotes do SDK em [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) com as credenciais `admin`.
1. ____ Faça o upload do  `wknd-app.all.x.x.x.zip` download na etapa 1
1. Toque no botão __Instalar__ para a entrada `wknd-app.all.x.x.x.zip`
1. ____ Faça o upload do  `wknd-app.ui.content.sample.x.x.x.zip` download na etapa 2
1. Toque no botão __Instalar__ para a entrada `wknd-app.ui.content.sample.x.x.x.zip`

## Baixar a fonte do aplicativo WKND

Baixe o código-fonte do aplicativo WKND em Github.com e alterne a ramificação que contém as alterações no SPA executado neste tutorial.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
$ git checkout -b feature/spa-editor
$ git pull origin feature/spa-editor
```

## Inicie o aplicativo SPA

Na raiz do projeto, instale as dependências do SPA projects npm e execute o aplicativo.

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

## Criar conteúdo no Editor AEM SPA

Antes de criar o conteúdo, organize as janelas do navegador de modo que o AEM Author (`http://localhost:4502`) esteja à esquerda e o SPA remoto (`http://localhost:3000`) seja executado à direita. Esse acordo permite ver como as alterações no conteúdo de fonte AEM são refletidas imediatamente no SPA.

1. Faça logon em [AEM serviço de autor do SDK](http://localhost:4502) como `admin`
1. Navegue até __Sites > Aplicativo WKND > us > en__
1. Editar __Página Inicial do Aplicativo WKND__
1. Alternar para o modo __Editar__

### Crie o componente fixo da exibição inicial

1. Toque no texto __Aventuras WKND__ para ativar o componente Título fixo (codificado na exibição Início SPA)
1. Toque no ícone __chave__ na barra de ação do componente Título
1. Altera o conteúdo do componente Título e salva
1. Atualize o SPA em execução em `http://localhost:3000` e veja se as alterações refletiram

### Crie o componente de contêiner da exibição inicial

1. Enquanto ainda edita a __Página Inicial do Aplicativo WKND__...
1. Expanda a __barra lateral do Editor de SPA__ (à esquerda)
1. Toque nos ícones __Componentes__
1. Adicionar, alterar ou remover componentes do componente de contêiner que se encontra sob o logotipo WKND e acima do componente de Título fixo
1. Atualize o SPA em execução em `http://localhost:3000` e veja se as alterações refletiram

### Criar um componente de contêiner em uma rota dinâmica

1. Alterne para o modo __Visualização__ no Editor SPA
1. Toque no cartão __Bali Surf Camp__ e navegue até a rota dinâmica
1. Adicionar, alterar ou remover componentes do componente de contêiner que estão acima do cabeçalho __Itinerário__
1. Atualize o SPA em execução em `http://localhost:3000` e veja se as alterações refletiram

## Parabéns!

Você acabou de experimentar rapidamente como AEM Editor SPA pode aprimorar seu SPA com áreas controladas e editáveis! Se você estiver interessado - confira o resto do tutorial, mas certifique-se de começar de novo, já que nesta configuração rápida, seu ambiente de desenvolvimento local agora está no estado final do tutorial!
