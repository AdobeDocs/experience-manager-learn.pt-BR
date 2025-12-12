---
title: Configuração do desenvolvimento local
description: Saiba como configurar um ambiente de desenvolvimento local para o editor universal a fim de editar o conteúdo de um aplicativo React de amostra.
version: Experience Manager as a Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 189
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 47bef697-5253-493a-b9f9-b26c27d2db56
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '787'
ht-degree: 100%

---

# Configuração do desenvolvimento local

Saiba como configurar um ambiente de desenvolvimento local para editar o conteúdo de um aplicativo React com o editor universal do AEM.

## Pré-requisitos

Os seguintes elementos são necessários para seguir este tutorial:

- Habilidades básicas de HTML e JavaScript.
- As seguintes ferramentas devem ser instaladas localmente:
   - [Node.js](https://nodejs.org/en/download/)
   - [Git](https://git-scm.com/downloads)
   - Um IDE ou editor de código, como o [Visual Studio Code](https://code.visualstudio.com/)
- Baixe e instale o seguinte:
   - [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime#download-the-aem-as-a-cloud-service-sdk): contém o Quickstart Jar usado para executar o AEM Author e Publish localmente para fins de desenvolvimento.
   - [Serviço do editor universal](https://experienceleague.adobe.com/pt-br/docs/experience-cloud/software-distribution/home): uma cópia local do serviço do editor universal que contém um subconjunto de recursos e pode ser baixada do portal de distribuição de softwares.
   - [local-ssl-proxy](https://www.npmjs.com/package/local-ssl-proxy#local-ssl-proxy): um proxy HTTP SSL local simples que usa um certificado autoassinado para desenvolvimento local. O editor universal do AEM requer o URL HTTPS do aplicativo React para carregá-lo no editor.

## Configuração local

Siga as etapas abaixo para configurar o ambiente de desenvolvimento local:

### SDK do AEM

Para fornecer o conteúdo do aplicativo React WKND Teams, instale os seguintes pacotes no SDK do AEM local.

- [WKND Teams: pacote de conteúdo](./assets/basic-tutorial-solution.content.zip): contém os modelos de fragmento de conteúdo, fragmentos de conteúdo e consultas persistentes do GraphQL.
- [WKND Teams: pacote de configuração](./assets/basic-tutorial-solution.ui.config.zip): contém as configurações de compartilhamento de recursos entre origens (CORS, na sigla em inglês) e do tratador de autenticação de tokens. O CORS facilita que propriedades da web que não forem do AEM façam chamadas do lado do cliente baseadas no navegador para as APIs do GraphQL do AEM, enquanto o tratador de autenticação de tokens é usado para autenticar cada solicitação recebida pelo AEM.

  ![WKND Teams - Pacotes](./assets/wknd-teams-packages.png)

### Aplicativo React

Para configurar o aplicativo React WKND Teams, siga as etapas abaixo:

1. Clone o [aplicativo React WKND Teams](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial) a partir da ramificação da solução `basic-tutorial`.

   ```bash
   $ git clone -b solution/basic-tutorial git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Navegue até o diretório `basic-tutorial` e abra-o no editor de código.

   ```bash
   $ cd aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

1. Instale as dependências e inicie o aplicativo React.

   ```bash
   $ npm install
   $ npm start
   ```

1. Abra o aplicativo React WKND Teams no navegador, em [http://localhost:3000](http://localhost:3000). Ele exibe uma lista de membros da equipe e seus detalhes. O conteúdo do aplicativo React é fornecido pelo SDK do AEM local, usando as APIs do GraphQL (`/graphql/execute.json/my-project/all-teams`), que você pode verificar por meio da guia de rede do navegador.

   ![WKND Teams: aplicativo React](./assets/wknd-teams-react-app.png)

### Serviço do editor universal

Para configurar o serviço do editor universal **local**, siga as etapas abaixo:

1. Baixe a versão mais recente do serviço do editor universal do [Portal de distribuição de softwares](https://experience.adobe.com/downloads).

   ![Distribuição de softwares: serviço do editor universal](./assets/universal-editor-service.png)

1. Extraia o arquivo zip baixado e copie o arquivo `universal-editor-service.cjs` para um novo diretório chamado `universal-editor-service`.

   ```bash
   $ unzip universal-editor-service-vproduction-<version>.zip
   $ mkdir universal-editor-service
   $ cp universal-editor-service.cjs universal-editor-service
   ```

1. Crie o arquivo `.env` no diretório `universal-editor-service` e adicione as seguintes variáveis de ambiente:

   ```bash
   # The port on which the Universal Editor service runs
   UES_PORT=8000
   # Disable SSL verification
   UES_TLS_REJECT_UNAUTHORIZED=false
   ```

1. Inicie o serviço do editor universal local.

   ```bash
   $ cd universal-editor-service
   $ node universal-editor-service.cjs
   ```

O comando acima inicia o serviço do editor universal na porta `8000`, e você deverá ver a seguinte saída:

```bash
Either no private key or certificate was set. Starting as HTTP server
Universal Editor Service listening on port 8000 as HTTP Server
```

### Proxy HTTP SSL local

O editor universal do AEM exige que o aplicativo React seja distribuído por HTTPS. Vamos configurar um proxy HTTP SSL local que usa um certificado autoassinado para desenvolvimento local.

Siga as etapas abaixo para configurar o proxy HTTP SSL local e fornecer o SDK do AEM SDK e o serviço do editor universal por HTTPS:

1. Instale o pacote `local-ssl-proxy` globalmente.

   ```bash
   $ npm install -g local-ssl-proxy
   ```

1. Inicie duas instâncias do proxy HTTP SSL local para os seguintes serviços:

   - Proxy HTTP SSL local do SDK do AEM na porta `8443`.
   - Proxy HTTP SSL local do serviço do editor universal na porta `8001`.

   ```bash
   # AEM SDK local SSL HTTP proxy on port 8443
   $ local-ssl-proxy --source 8443 --target 4502
   
   # Universal Editor service local SSL HTTP proxy on port 8001
   $ local-ssl-proxy --source 8001 --target 8000
   ```

### Atualizar o aplicativo React para usar HTTPS

Para habilitar o HTTPS para o aplicativo React WKND Teams, siga as etapas abaixo:

1. Pare o React, pressionando `Ctrl + C` no terminal.
1. Atualize o arquivo `package.json` para incluir a variável de ambiente `HTTPS=true` no script `start`.

   ```json
   "scripts": {
       "start": "HTTPS=true react-scripts start",
       ...
   }
   ```

1. Atualize o `REACT_APP_HOST_URI` no arquivo `.env.development` para usar o protocolo HTTPS e a porta do proxy HTTP SSL local do SDK do AEM.

   ```bash
   REACT_APP_HOST_URI=https://localhost:8443
   ...
   ```

1. Atualize o arquivo `../src/proxy/setupProxy.auth.basic.js` para usar as configurações de SSL relaxadas, usando a opção `secure: false`.

   ```javascript
   ...
   module.exports = function(app) {
   app.use(
       ['/content', '/graphql'],
       createProxyMiddleware({
       target: REACT_APP_HOST_URI,
       changeOrigin: true,
       secure: false, // Ignore SSL certificate errors
       // pass in credentials when developing against an Author environment
       auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`
       })
   );
   };
   ```

1. Inicie o aplicativo React.

   ```bash
   $ npm start
   ```

## Verificar a configuração

Depois de configurar o ambiente de desenvolvimento local por meio das etapas acima, vamos verificar a configuração.

### Verificação local

Verifique se os seguintes serviços estão sendo executados localmente em HTTPS. Talvez seja necessário aceitar o aviso de segurança no navegador referente ao certificado autoassinado:

1. Aplicativo React WKND Teams em [https://localhost:3000](https://localhost:3000)
1. SDK do AEM em [https://localhost:8443](https://localhost:8443)
1. Serviço do editor universal em [https://localhost:8001](https://localhost:8001)

### Carregar o aplicativo React WKND Teams no editor universal

Vamos carregar o aplicativo React WKND Teams no editor universal para verificar a configuração:

1. Abra o editor universal (https://experience.adobe.com/#/aem/editor) no seu navegador. Se solicitado, faça logon com o seu Adobe ID.

1. Insira o URL do aplicativo React WKND Teams no campo de inserção de URL do site do editor universal e clique em `Open`.

   ![Editor universal: URL do site](./assets/universal-editor-site-url.png)

1. O aplicativo React WKND Teams será carregado no editor universal, **mas você ainda não poderá editar o conteúdo**. É necessário instrumentar o aplicativo React para habilitar a edição de conteúdo com o editor universal.

   ![Editor universal: aplicativo React WKND Teams](./assets/universal-editor-wknd-teams.png)


## Próxima etapa

Saiba como [instrumentar o aplicativo React para editar o conteúdo](./instrument-to-edit-content.md).
