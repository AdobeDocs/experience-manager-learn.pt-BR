---
title: Visão geral das APIs do AEM
description: Saiba mais sobre os diferentes tipos de APIs no Adobe Experience Manager (AEM) e entenda qual API escolher para sua integração.
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-17425
thumbnail: KT-17425.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 23b2be0d-a8d4-4521-96ba-78b70f4e9cba
source-git-commit: e4cf47e14fa7dfc39ab4193d35ba9f604eabf99f
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 2%

---

# Visão geral das APIs do AEM{#aem-apis-overview}

Saiba mais sobre os diferentes tipos de APIs no Adobe Experience Manager (AEM) e entenda qual API escolher para sua integração.

Para criar, ler, atualizar e excluir conteúdo, ativos e formulários no AEM, os desenvolvedores podem usar uma grande variedade de APIs. Essas APIs permitem que os desenvolvedores criem aplicativos personalizados que interagem com o AEM.

Vamos explorar os diferentes tipos de APIs no AEM e entender qual API escolher para sua integração.

## Tipos de APIs do AEM{#types-of-aem-apis}

O AEM oferece as seguintes APIs para interagir com seus tipos de serviço de autoria e publicação.

| Tipo de API do AEM | Descrição | Disponibilidade | Caso de uso | Exemplos de API |
| --- | --- | --- | --- | --- |
| APIs do AEM baseadas em OpenAPI | APIs padronizadas e legíveis por máquina para Assets, Sites e Forms. | **Somente AEM as a Cloud Service** | Desenvolvimento de API First, aplicativos modernos | [API do Autor do Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/), [API de Pastas](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/), [API do AEM Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/), [Forms Acrobat Services](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) e outros |
| APIs RESTful | Endpoints REST tradicionais para interação com recursos do AEM. | AEM 6.X, AEM as a Cloud Service | Operações CRUD, aplicativos modernos | [API HTTP do Assets](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), [API REST do fluxo de trabalho](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api), [Exportador JSON para Serviços de Conteúdo](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter) e outros |
| APIs do GraphQL | Otimizado para recuperar conteúdo estruturado com eficiência com consultas flexíveis. | AEM 6.X, AEM as a Cloud Service | CMS headless, SPAs, aplicativos móveis | [API GraphQL](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| APIs tradicionais (não RESTful) | APIs mais antigas, como JCR, Modelos Sling, Construtor de consultas e outras. | AEM 6.X, AEM as a Cloud Service | Integrações herdadas, compatibilidade com versões anteriores | [API do Construtor de Consultas](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api) e outras |

Para obter mais detalhes, consulte a página [APIs do Adobe Experience Manager as a Cloud Service](https://developer.adobe.com/experience-cloud/experience-manager-apis/).

## Qual API escolher{#which-api-to-choose}

Ao selecionar uma API para sua integração, considere os seguintes fatores:

- **Caso de uso**: determine se a API do AEM oferece suporte ao seu caso de uso. Sempre que possível, _use APIs do AEM baseadas em OpenAPI_, pois elas fornecem uma abordagem moderna e padronizada para interagir com o AEM. Se as APIs baseadas em OpenAPI não estiverem disponíveis, considere usar APIs RESTful ou APIs do GraphQL e, como último recurso, APIs tradicionais.

- **Compatibilidade**: verifique se a API selecionada é compatível com sua versão do AEM. Por exemplo, as _APIs do AEM baseadas em OpenAPI são exclusivas do AEM as a Cloud Service_ e não estão disponíveis no AEM 6.X.

- **Tipo de serviço do AEM: Autor vs. Publicação**: a escolha da API também depende se ela é executada no serviço Autor ou Publicação, já que seus modelos de acesso são diferentes. O serviço do Autor do AEM é usado para criação de conteúdo e sempre requer autenticação. O serviço de Publicação do AEM é usado para entrega de conteúdo e pode não exigir autenticação, dependendo do caso de uso.

- **Autenticação**: verifique se a API dá suporte ao método de autenticação que você planeja usar. Por exemplo:
   - **APIs do AEM baseadas em OpenAPI**: oferecem suporte à autenticação OAuth 2.0, incluindo os tipos de concessão Credenciais de Cliente (Servidor para Servidor), Código de Autorização (Aplicativo Web) e Chave de Prova para Troca de Código (Aplicativo de Página Única). Outras APIs do AEM não são compatíveis com a autenticação OAuth 2.0.
   - **APIs RESTful**: suporta a autenticação JSON Web Token (JWT), também conhecida como autenticação baseada em token.

## Diferença entre o JSON Web Token (JWT) e o OAuth 2.0{#difference-between-jwt-and-oauth}

Vamos comparar o JSON Web Token (JWT) e o OAuth 2.0, dois mecanismos de autenticação comuns usados nas APIs do AEM:

| Destaque | JSON Web Token (JWT) | OAuth 2.0 |
| --- | --- | --- |
| Usado em | APIs RESTful | APIs do AEM baseadas em OpenAPI (não compatíveis com RESTful ou outras APIs) |
| Propósito | Autenticação de serviço | Autenticação de usuário ou serviço |
| Interação do usuário | Não é necessária nenhuma interação com o usuário | Interação do usuário necessária para os tipos de concessão Código de autorização e Aplicativo de página única |
| Mais adequado para | Chamadas de API de servidor para servidor | Acesso seguro e permitido para aplicativos e usuários |
| Informações necessárias | Chave privada para assinatura do JWT | ID do cliente e segredo do cliente para OAuth 2.0 |
| Expiração do token | De vida curta, geralmente precisa de atualização | O token de acesso tem vida curta. O token de atualização tem vida longa e é usado para obter um novo token de acesso |
| Gerenciamento de credenciais | [AEM Developer Console](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console) | [Adobe Developer Console](https://developer.adobe.com/developer-console/) |

## APIs do AEM baseadas em OpenAPI

Saiba mais sobre as APIs do AEM baseadas em OpenAPI e os conceitos importantes de acesso às APIs do Adobe no [guia de APIs do AEM baseadas em OpenAPI](./openapis/overview.md).

### Casos de uso

<!-- CARDS
{target = _self}

* ./openapis/use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./openapis/assets/s2s/OAuth-S2S.png}
* ./openapis/use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./openapis/assets/web-app/OAuth-WebApp.png}  
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" title="Chamar API usando autenticação de servidor para servidor" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./openapis/assets/s2s/OAuth-S2S.png" alt="Chamar API usando autenticação de servidor para servidor"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="Chamar API usando autenticação de servidor para servidor">Invocar API usando autenticação de Servidor para Servidor</a>
                    </p>
                    <p class="is-size-6">Saiba como chamar APIs do AEM baseadas em OpenAPI de um aplicativo NodeJS personalizado usando a autenticação de servidor para servidor do OAuth.</p>
                </div>
                <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" title="Chamar API usando autenticação do Aplicativo Web" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./openapis/assets/web-app/OAuth-WebApp.png" alt="Chamar API usando autenticação do Aplicativo Web"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="Chamar API usando autenticação do Aplicativo Web">Invocar API usando autenticação de Aplicativo Web</a>
                    </p>
                    <p class="is-size-6">Saiba como chamar APIs do AEM baseadas em OpenAPI de um aplicativo web personalizado usando a autenticação do OAuth Web App.</p>
                </div>
                <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



## APIs do GraphQL - Exemplos

Saiba mais sobre as APIs do GraphQL e como usá-las na [Introdução ao AEM Headless - GraphQL](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview)

### Casos de uso

<!-- CARDS
{target = _self}

* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app
  {title = Single Page Application (SPA)}
  {description = Learn how to build a Single Page Application (SPA) that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/react-app-card.png}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps
  {title = Mobile App}
  {description = Learn how to build a mobile app that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/ios-app-card.png}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component
  {title = Web Component}
  {description = Learn how to build a web component that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/web-component-card.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single Page Application (SPA)">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" title="Aplicativo de página única (SPA)" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/react-app-card.png" alt="Aplicativo de página única (SPA)"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" target="_self" rel="referrer" title="Aplicativo de página única (SPA)">Aplicativo de Página Única (SPA)</a>
                    </p>
                    <p class="is-size-6">Saiba como criar um Aplicativo de página única (SPA) que busca conteúdo do AEM usando APIs do GraphQL.</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Mobile App">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" title="Aplicativo móvel" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/ios-app-card.png" alt="Aplicativo móvel"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" target="_self" rel="referrer" title="Aplicativo móvel">Aplicativo móvel</a>
                    </p>
                    <p class="is-size-6">Saiba como criar um aplicativo móvel que busque conteúdo do AEM usando APIs do GraphQL.</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" title="Componente da Web" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-component-card.png" alt="Componente da Web"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" target="_self" rel="referrer" title="Componente da Web">Componente da Web</a>
                    </p>
                    <p class="is-size-6">Saiba como criar um componente da Web que busque conteúdo do AEM usando APIs do GraphQL.</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## RESTful APIs - Exemplos

Saiba mais sobre as APIs RESTful, como [API HTTP do Assets](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets) e [Exportador JSON](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter).

### Casos de uso

<!-- CARDS
{target = _self}

* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to build a native mobile app that fetches content from AEM using Content Services RESTful APIs.}
  {image = ./assets/RESTful-Content-Service.png}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview
  {title = Token-based Authentication for RESTful APIs}
  {description = Learn how to invoke RESTful APIs using JSON Web Token (JWT) authentication.}
  {image = ./assets/RESTful-TokenAuth.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" title="Chamar API usando autenticação de servidor para servidor" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/RESTful-Content-Service.png" alt="Chamar API usando autenticação de servidor para servidor"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" target="_self" rel="referrer" title="Chamar API usando autenticação de servidor para servidor">Invocar API usando autenticação de Servidor para Servidor</a>
                    </p>
                    <p class="is-size-6">Saiba como criar um aplicativo móvel nativo que busque conteúdo do AEM usando as APIs RESTful do Content Services.</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Token-based Authentication for RESTful APIs">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" title="Autenticação baseada em token para APIs RESTful" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/RESTful-TokenAuth.png" alt="Autenticação baseada em token para APIs RESTful"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" target="_self" rel="referrer" title="Autenticação baseada em token para APIs RESTful">Autenticação baseada em token para APIs RESTful</a>
                    </p>
                    <p class="is-size-6">Saiba como invocar APIs RESTful usando autenticação JSON Web Token (JWT).</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


