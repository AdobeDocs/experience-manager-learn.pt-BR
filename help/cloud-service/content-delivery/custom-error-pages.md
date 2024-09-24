---
title: Páginas de erro personalizadas
description: Saiba como implementar páginas de erro personalizadas para seu site hospedado pela AEM as a Cloud Service.
version: Cloud Service
feature: Brand Experiences, Configuring, Developing
topic: Content Management, Development
role: Developer
level: Beginner, Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-09-24T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
source-git-commit: acecb13a5b7f3c75bc6cd8eb36d398f8cebfe0a6
workflow-type: tm+mt
source-wordcount: '1355'
ht-degree: 0%

---


# Páginas de erro personalizadas

Saiba como implementar páginas de erro personalizadas para seu site hospedado pela AEM as a Cloud Service.

Neste tutorial, você aprenderá:

- Páginas de erro padrão
- Páginas de erro veiculadas pelo
   - Tipo de serviço AEM - autor, publicação, visualização
   - CDN gerenciada por Adobe
- Opções para personalizar páginas de erro
   - Diretiva Apache ErrorDocument
   - ACS AEM Commons - Error Page Handler
   - Páginas de erro da CDN

## Páginas de erro padrão

Vamos analisar quando as páginas de erro são exibidas, as páginas de erro padrão e de onde elas são fornecidas.

Páginas de erro são exibidas quando:

- a página não existe (404)
- não autorizado a acessar uma página (403)
- erro do servidor (500) devido a problemas de código ou o servidor está inacessível.

A AEM as a Cloud Service fornece _páginas de erro padrão_ para os cenários acima. É uma página genérica e não corresponde à sua marca.

A página de erro padrão _é servida_ do _tipo de serviço AEM_(autor, publicação, visualização) ou da _CDN gerenciada por Adobe_. Consulte a tabela abaixo para obter mais detalhes.

| Página de erro exibida em | Detalhes |
|---------------------|:-----------------------:|
| Tipo de serviço AEM - autor, publicação, visualização | Quando a solicitação de página é fornecida pelo tipo de serviço AEM, a página de erro é fornecida pelo tipo de serviço AEM. |
| CDN gerenciada por Adobe | Quando o CDN gerenciado por Adobe _não pode alcançar o tipo de serviço AEM_ (servidor de origem), a página de erro é disponibilizada pelo CDN gerenciado por Adobe. **É um evento improvável, mas vale a pena mencionar.** |

No entanto, você pode _personalizar o tipo de serviço do AEM e as páginas de erro da CDN_ gerenciadas por Adobe para corresponder à sua marca e fornecer uma melhor experiência ao usuário.

## Opções para personalizar páginas de erro

As seguintes opções estão disponíveis para personalizar páginas de erro:

| Aplicável a | Nome da opção | Descrição |
|---------------------|:-----------------------:|:-----------------------:|
| Tipos de serviço do AEM - publicar e visualizar | Diretiva ErrorDocument | Use a diretiva [ErrorDocument](https://httpd.apache.org/docs/2.4/custom-error.html) no arquivo de configuração do Apache para especificar o caminho para a página de erro personalizada. Aplicável somente aos tipos de serviço AEM: publicar e visualizar. |
| Tipos de serviço do AEM - autor, publicação, visualização | ACS AEM Commons Error Page Handler | Use o [Manipulador de página de erro ACS AEM](https://adobe-consulting-services.github.io/acs-aem-commons/features/error-handler/index.html) para personalizar o erro em todos os tipos de serviço AEM. |
| CDN gerenciada por Adobe | Páginas de erro da CDN | Use as páginas de erro do CDN para personalizar as páginas de erro quando o CDN gerenciado por Adobe não puder alcançar o tipo de serviço AEM (servidor de origem). |


## Pré-requisitos

Neste tutorial, você aprenderá a personalizar páginas de erro usando a diretiva _ErrorDocument_, o _Manipulador de página de erro AEM ACS_ e as _Páginas de erro CDN_. Para seguir este tutorial, você precisa:

- O [ambiente de desenvolvimento local do AEM](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview) ou o ambiente do AEM as a Cloud Service. A opção _Páginas de Erro de CDN_ é aplicável ao ambiente do AEM as a Cloud Service.

- O [projeto WKND AEM](https://github.com/adobe/aem-guides-wknd) para personalizar páginas de erro.

## Configuração

- Clonar e implantar o projeto AEM WKND no ambiente de desenvolvimento local do AEM seguindo as etapas abaixo:

  ```
  # For local AEM development environment
  $ git clone git@github.com:adobe/aem-guides-wknd.git
  $ cd aem-guides-wknd
  $ mvn clean install -PautoInstallSinglePackage -PautoInstallSinglePackagePublish
  ```

- Para o ambiente do AEM as a Cloud Service, implante o projeto WKND AEM executando o [pipeline de pilha completa](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#full-stack-pipeline). Consulte o exemplo de [pipeline de não produção](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/cloud-manager/cicd-non-production-pipeline).

- Verifique se as páginas do site WKND são renderizadas corretamente.

## Diretiva ErrorDocument Apache para personalizar páginas de erro{#errordocument-directive}

Vamos analisar como o projeto [AEM WKND](https://github.com/adobe/aem-guides-wknd) usa a diretiva do Apache `ErrorDocument` para exibir páginas de erro personalizadas.

- O módulo `ui.content.sample` contém as [páginas de erro](https://github.com/adobe/aem-guides-wknd/tree/main/ui.content.sample/src/main/content/jcr_root/content/wknd/language-masters/en/errors) @ `/content/wknd/language-masters/en/errors` da marca. Revise-as no seu ambiente [AEM](http://localhost:4502/sites.html/content/wknd/language-masters/en/errors) local ou AEM as a Cloud Service `https://author-p<ID>-e<ID>.adobeaemcloud.com/ui#/aem/sites.html/content/wknd/language-masters/en/errors`.

- O arquivo `wknd.vhost` do módulo `dispatcher` contém:
   - A [diretiva ErrorDocument](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L139-L143) que aponta para as [páginas de erro](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/variables/custom.vars#L7-L8) acima.
   - O valor [DispatcherPassError](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L133) está definido como 1 para que o Dispatcher permita que o Apache manipule todos os erros.

  ```
  ...
  # ErrorDocument directive in wknd.vhost file
  ErrorDocument 404 ${404_PAGE}
  ErrorDocument 500 ${500_PAGE}
  ErrorDocument 502 ${500_PAGE}
  ErrorDocument 503 ${500_PAGE}
  ErrorDocument 504 ${500_PAGE}
  
  ...
  # DispatcherPassError value in wknd.vhost file
  <IfModule disp_apache2.c>
      ...
      DispatcherPassError        1
  </IfModule>
  
  # Custom error pages path in custom.vars file
  Define 404_PAGE /content/wknd/us/en/errors/404.html
  Define 500_PAGE /content/wknd/us/en/errors/500.html
  ...
  ```

- Revise as páginas de erro personalizadas do site WKND inserindo um nome de página ou caminho incorreto no seu ambiente, por exemplo [https://publish-p105881-e991000.adobeaemcloud.com/us/en/foo/bar.html](https://publish-p105881-e991000.adobeaemcloud.com/us/en/foo/bar.html).

## ACS AEM Commons-Error Page Handler para personalizar páginas de erro{#acs-aem-commons-error-page-handler}

Para personalizar páginas de erro usando o Manipulador de página de erro ACS AEM Commons, reveja a seção [Como usar](https://adobe-consulting-services.github.io/acs-aem-commons/features/error-handler/index.html#how-to-use).

## Páginas de erro da CDN para personalizar páginas de erro{#cdn-error-pages}

Vamos implementar páginas de erro de CDN para personalizar páginas de erro quando o CDN gerenciado por Adobe não puder alcançar o tipo de serviço AEM (servidor de origem).

>[!IMPORTANT]
>
> Observe que o CDN gerenciado por Adobe não pode atingir o tipo de serviço AEM (servidor de origem) é um evento improvável, mas vale a pena planejar para.


### Visão geral das páginas de erro do CDN

A página de erro CDN é implementada como um aplicativo de página única (SPA) pelo CDN gerenciado por Adobe.

O conteúdo específico da marca WKND deve ser gerado dinamicamente usando o arquivo JavaScript. O arquivo JavaScript deve ser hospedado em um local acessível publicamente. Assim, os seguintes arquivos estáticos devem ser desenvolvidos e hospedados em um local acessível publicamente:

1. **jsUrl**: o URL absoluto do arquivo JavaScript para renderizar o conteúdo da página de erro, criando elementos HTML dinamicamente.
1. **cssUrl**: a URL absoluta do arquivo CSS para o estilo do conteúdo da página de erro.
1. **icoUrl**: a URL absoluta do favicon.

### Desenvolver uma página de erro personalizada

Vamos desenvolver o conteúdo específico da página de erro de marca WKND como um Aplicativo de página única (SPA).

Para fins de demonstração, vamos usar o [React](https://react.dev/). No entanto, você pode usar qualquer estrutura ou biblioteca do JavaScript.

- Crie um novo projeto React executando o seguinte comando:

  ```
  $ npx create-react-app aem-cdn-error-page
  ```

- Abra o projeto em seu editor de código favorito e atualize os arquivos abaixo:

   - `src/App.js`: é o componente principal que renderiza o conteúdo da página de erro.

     ```javascript
     import logo from "./wknd-logo.png";
     import "./App.css";
     
     function App() {
       return (
         <>
           <div className="App">
             <div className="container">
             <img src={logo} className="App-logo" alt="logo" />
             </div>
           </div>
           <div className="container">
             <div className="error-code">CDN Error Page</div>
             <h1 className="error-message">Ruh-Roh! Page Not Found</h1>
             <p className="error-description">
               We're sorry, we are unable to fetch this page!
             </p>
           </div>
         </>
       );
     }
     
     export default App;
     ```

   - `src/App.css`: Estilo do conteúdo da página de erro.

     ```css
         .App {
       text-align: left;
     }
     
     .App-logo {
       height: 14vmin;
       pointer-events: none;
     }
     
     
     body {
       margin-top: 0;
       padding: 0;
       font-family: Arial, sans-serif;
       background-color: #fff;
       color: #333;
       display: flex;
       justify-content: center;
       align-items: center;
     }
     
     .container {
       text-align: letf;
       padding-top: 10px;
     }
     
     .error-code {
       font-size: 4rem;
       font-weight: bold;
       color: #ff6b6b;
     }
     
     .error-message {
       font-size: 2.5rem;
       margin-bottom: 10px;
     }
     
     .error-description {
       font-size: 1rem;
       margin-bottom: 20px;
     }
     ```

   - Adicionar o arquivo `wknd-logo.png` à pasta `src`. Copie o [arquivo](https://github.com/adobe/aem-guides-wknd/blob/main/ui.frontend/src/main/webpack/resources/images/favicons/favicon-512.png) como `wknd-logo.png`.

   - Adicionar o arquivo `favicon.ico` à pasta `public`. Copie o [arquivo](https://github.com/adobe/aem-guides-wknd/blob/main/ui.frontend/src/main/webpack/resources/images/favicons/favicon-32.png) como `favicon.ico`.

   - Verifique o conteúdo da página de erro CDN com marca WKND executando o projeto:

     ```
     $ npm start
     ```

     Abra o navegador e navegue até `http://localhost:3000/` para ver o conteúdo da página de erro CDN.

   - Crie o projeto para gerar os arquivos estáticos:

     ```
     $ npm run build
     ```

     Os arquivos estáticos são gerados na pasta `build`.


Como alternativa, você pode baixar o arquivo [aem-cdn-error-page.zip](./assets/aem-cdn-error-page.zip) que contém os arquivos de projeto do React acima.

Em seguida, hospede os arquivos estáticos acima em um local acessível publicamente.

### Arquivos estáticos de host necessários para a página de erro CDN

Vamos hospedar os arquivos estáticos no Armazenamento de blobs do Azure. No entanto, você pode usar qualquer serviço de hospedagem de arquivos estáticos, como [Netlify](https://www.netlify.com/), [Vercel](https://vercel.com/) ou [AWS S3](https://aws.amazon.com/s3/).

- Siga a documentação oficial do [Armazenamento Azure Blob](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal) para criar um contêiner e carregar os arquivos estáticos.

  >[!IMPORTANT]
  >
  >Se você estiver usando outros serviços de hospedagem de arquivos estáticos, siga a documentação deles para hospedar os arquivos estáticos.

- Verifique se os arquivos estáticos estão acessíveis publicamente. Minhas configurações de conta de armazenamento específicas de demonstração WKND são as seguintes:

   - **Nome da Conta de Armazenamento**: `aemcdnerrorpageresources`
   - **Nome do Contêiner**: `static-files`

  ![Armazenamento Azure Blob](./assets/azure-blob-storage-settings.png)

- No contêiner acima `static-files`, abaixo, os arquivos da pasta `build` são carregados:

   - `error.js`: O arquivo `build/static/js/main.<hash>.js` foi renomeado para `error.js` e [publicamente acessível](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.js).
   - `error.css`: O arquivo `build/static/css/main.<hash>.css` foi renomeado para `error.css` e [publicamente acessível](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.css).
   - `favicon.ico`: O arquivo `build/favicon.ico` é carregado como está e [está acessível publicamente](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/favicon.ico).

Em seguida, configure a regra CDN (errorPages) e faça referência aos arquivos estáticos acima.

### Configurar a regra CDN

Vamos configurar a regra de CDN `errorPages` que usa os arquivos estáticos acima para renderizar o conteúdo da página de erro de CDN.

1. Abra o arquivo `cdn.yaml` na pasta `config` principal do seu projeto AEM. Por exemplo, o arquivo cdn.yaml](https://github.com/adobe/aem-guides-wknd/blob/main/config/cdn.yaml) do projeto [WKND.

1. Adicionar a seguinte regra CDN ao arquivo `cdn.yaml`:

   ```yaml
   kind: "CDN"
   version: "1"
   metadata:
     envTypes: ["dev", "stage", "prod"]
   data:
     # The CDN Error Page configuration. 
     # The error page is displayed when the Adobe-managed CDN is unable to reach the origin server.
     # It is implemented as a Single Page Application (SPA) and WKND branded content must be generated dynamically using the JavaScript file 
     errorPages:
       spa:
         title: Adobe AEM CDN Error Page # The title of the error page
         icoUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/favicon.ico # The PUBLIC URL of the favicon
         cssUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.css # The PUBLIC URL of the CSS file
         jsUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.js # The PUBLIC URL of the JavaScript file
   ```

1. Salve, confirme e envie as alterações para o repositório upstream de Adobe.

### Implantar a regra CDN

Por fim, implante a regra CDN configurada no ambiente do AEM as a Cloud Service usando o pipeline do Cloud Manager.

1. Na Cloud Manager, navegue até a seção **Pipelines**.

1. Crie um novo pipeline ou selecione o pipeline existente que implanta apenas os arquivos **Config**. Para obter etapas detalhadas, consulte [Criar um pipeline de configuração](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager).

1. Clique no botão **Executar** para implantar a regra CDN.

![Implantar Regra CDN](./assets/deploy-cdn-rule-for-cdn-error-pages.png)

### Testar as páginas de erro do CDN

Para testar as páginas de erro do CDN, siga as etapas abaixo:

- Abra o navegador e navegue até a URL de ambiente Publish, anexe o `cdnstatus?code=404` à URL, por exemplo, [https://publish-p105881-e991000.adobeaemcloud.com/cdnstatus?code=404](https://publish-p105881-e991000.adobeaemcloud.com/cdnstatus?code=404), ou acesse usando a [URL de domínio personalizado](https://wknd.enablementadobe.com/cdnstatus?code=404)

  ![WKND - Página de Erro da CDN](./assets/wknd-cdn-error-page.png)

- Os códigos compatíveis são: 403, 404, 406, 500 e 503.

- Verifique a guia de rede do navegador para ver se os arquivos estáticos são carregados do Armazenamento de Blobs do Azure. O documento HTML entregue pelo CDN gerenciado por Adobe contém o conteúdo mínimo básico e o arquivo JavaScript cria dinamicamente o conteúdo da página de erro de marca.

  ![Guia Rede da Página de Erro da CDN](./assets/wknd-cdn-error-page-network-tab.png)

## Resumo

Neste tutorial, você aprendeu a implementar páginas de erro personalizadas para seu site hospedado pela AEM as a Cloud Service.

Você também aprendeu as etapas detalhadas para a opção de páginas de erro CDN para personalizar páginas de erro quando o CDN gerenciado por Adobe não puder alcançar o tipo de serviço AEM (servidor de origem).



