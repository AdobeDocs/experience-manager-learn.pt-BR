---
title: APIs do AEM baseadas em OpenAPI
description: Saiba mais sobre as APIs do AEM baseadas em OpenAPI, incluindo suporte à autenticação, conceitos-chave e como acessar APIs do Adobe.
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
source-git-commit: e4cf47e14fa7dfc39ab4193d35ba9f604eabf99f
workflow-type: tm+mt
source-wordcount: '835'
ht-degree: 0%

---


# APIs do AEM baseadas em OpenAPI

>[!IMPORTANT]
>
>As APIs do AEM baseadas em OpenAPI só estão disponíveis no AEM as a Cloud Service e não são compatíveis com o AEM 6.X.

Saiba mais sobre as APIs do AEM baseadas em OpenAPI, incluindo suporte à autenticação, conceitos-chave e como acessar APIs do Adobe.

A [Especificação de OpenAPI](https://swagger.io/specification/) (anteriormente conhecida como Swagger) é um padrão amplamente usado para definir APIs RESTful. O AEM as a Cloud Service fornece várias APIs baseadas em especificação OpenAPI (ou simplesmente APIs do AEM baseadas em OpenAPI), facilitando a criação de aplicativos personalizados que interagem com os tipos de serviço de criação ou publicação do AEM. Abaixo estão alguns exemplos:

**Sites**

- [APIs do Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/): APIs para trabalhar com Fragmentos de conteúdo.

**Assets**

- [API de pastas](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/): APIs para trabalhar com pastas como criar, listar e excluir pastas.

- [API do autor do Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/): APIs para trabalhar com ativos e seus metadados.

**Forms**

- [APIs de comunicações do Forms](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/): APIs para trabalhar com formulários e documentos.

Em versões futuras, mais APIs do AEM baseadas em OpenAPI serão adicionadas para oferecer suporte a casos de uso adicionais.

>[!AVAILABILITY]
>
>As APIs do AEM baseadas em OpenAPI estão disponíveis como parte de um programa de acesso antecipado. Se você estiver interessado em acessá-las, recomendamos enviar um email para [aem-apis@adobe.com](mailto:aem-apis@adobe.com) com uma descrição do seu caso de uso.

## Suporte à autenticação{#authentication-support}

As APIs do AEM baseadas em OpenAPI oferecem suporte à autenticação OAuth 2.0, incluindo os seguintes tipos de concessão:

- **Credencial do servidor para servidor OAuth**: ideal para serviços de back-end que precisam de acesso à API sem interação com o usuário. Ele usa o tipo de concessão _client_credentials_, permitindo o gerenciamento de acesso seguro no nível do servidor. Para obter mais informações, consulte [Credencial do servidor para servidor do OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential).

- **Credencial do Aplicativo Web OAuth**: adequada para aplicativos Web com componentes de front-end e _back-end_ que acessam APIs do AEM em nome dos usuários. Ela usa o tipo de concessão _authorization_code_, em que o servidor de back-end gerencia segredos e tokens de forma segura. Para obter mais informações, consulte [Credencial do aplicativo Web OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential).

- **Credencial de aplicativo de página única do OAuth**: projetada para SPAs em execução no navegador, que precisa acessar APIs em nome de um usuário sem um servidor back-end. Ele usa o tipo de concessão _authorization_code_ e depende de mecanismos de segurança do lado do cliente usando PKCE (Chave de Prova para Troca de Código) para proteger o fluxo do código de autorização. Para obter mais informações, consulte [Credencial de aplicativo de página única do OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential).

## Diferença entre credenciais de servidor para servidor do OAuth e do aplicativo da Web do OAuth/aplicativo de página única{#difference-between-oauth-server-to-server-and-oauth-web-app-single-page-app-credentials}

| | Servidor para servidor do OAuth | Autenticação de usuário OAuth (aplicativo web) |
| --- | --- | --- |
| Finalidade de autenticação | Projetado para interações máquina a máquina. | Projetado para interações orientadas pelo usuário. |
| Comportamento do token | Emite tokens de acesso que representam o próprio aplicativo cliente. | Emite tokens de acesso em nome de um usuário autenticado. |
| Casos de uso | Serviços de back-end que precisam de acesso à API sem interação com o usuário. | Aplicativos web com componentes de front-end e back-end acessando APIs em nome dos usuários. |
| Considerações sobre segurança | Armazene credenciais confidenciais (`client_id`, `client_secret`) com segurança em sistemas back-end. | O usuário se autentica e recebe seu próprio token de acesso temporário. Armazene credenciais confidenciais (`client_id`, `client_secret`) com segurança em sistemas back-end. |
| Tipo de concessão | _credenciais_do_cliente_ | _authorization_code_ |

## Acesso às APIs do Adobe e conceitos relacionados{#accessing-adobe-apis-and-related-concepts}

Antes de acessar as APIs do Adobe, é essencial compreender estas construções principais:

- **[Adobe Developer Console](https://developer.adobe.com/)**: o hub do desenvolvedor para acessar APIs, SDKs, eventos em tempo real, funções sem servidor da Adobe e muito mais. Observe que ele é diferente do Developer Console _AEM_, que é usado para depurar aplicativos do AEM.

- **[Projeto do Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/projects/)**: local central para gerenciar integrações de API, eventos e funções de tempo de execução. Aqui, você configura APIs, define autenticação e gera as credenciais necessárias.

- **[Perfis de Produto](https://helpx.adobe.com/br/enterprise/using/manage-product-profiles.html)**: os Perfis de Produto fornecem uma predefinição de permissão que permite controlar o acesso do usuário ou do aplicativo a produtos da Adobe, como AEM, Adobe Target, Adobe Analytics e outros. Todos os produtos da Adobe têm perfis de produto predefinidos associados a eles.

- **Serviços**: os serviços definem as permissões reais e estão associados ao Perfil de Produto. Para reduzir ou aumentar a predefinição de permissões, é possível desmarcar ou selecionar os serviços associados ao Perfil do produto. Dessa forma, você pode controlar o nível de acesso ao produto e suas APIs. No AEM as a Cloud Service, os serviços representam grupos de usuários com Listas de controle de acesso (ACLs) predefinidas para nós de repositório, permitindo o gerenciamento granular de permissões.

## Introdução

Saiba como configurar seu ambiente do AEM as a Cloud Service e um projeto do Adobe Developer Console para habilitar o acesso às APIs do AEM baseadas em OpenAPI. Acesse também a API do AEM usando o navegador para verificar a configuração e revisar a solicitação e a resposta.

<!-- CARDS
{target = _self}

* ./setup.md
  {title = Set up OpenAPI-based AEM APIs}
  {description = Learn how to set up your AEM as a Cloud Service environment to enable access to the OpenAPI-based AEM APIs.}
  {image = ./assets/setup/OpenAPI-Setup.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up OpenAPI-based AEM APIs">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="Configurar APIs do AEM baseadas em OpenAPI" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/OpenAPI-Setup.png" alt="Configurar APIs do AEM baseadas em OpenAPI"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="Configurar APIs do AEM baseadas em OpenAPI">Configurar APIs do AEM baseadas em OpenAPI</a>
                    </p>
                    <p class="is-size-6">Saiba como configurar o ambiente do AEM as a Cloud Service para habilitar o acesso às APIs do AEM baseadas em OpenAPI.</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Tutoriais da API

Saiba como usar as APIs do AEM baseadas em OpenAPI usando diferentes métodos de autenticação OAuth:

<!-- CARDS
{target = _self}

* ./use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./assets/s2s/OAuth-S2S.png}
* ./use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./assets/web-app/OAuth-WebApp.png}  
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-s2s.md" title="Chamar API usando autenticação de servidor para servidor" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/s2s/OAuth-S2S.png" alt="Chamar API usando autenticação de servidor para servidor"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="Chamar API usando autenticação de servidor para servidor">Invocar API usando autenticação de Servidor para Servidor</a>
                    </p>
                    <p class="is-size-6">Saiba como chamar APIs do AEM baseadas em OpenAPI de um aplicativo NodeJS personalizado usando a autenticação de servidor para servidor do OAuth.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-web-app.md" title="Chamar API usando autenticação do Aplicativo Web" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-app/OAuth-WebApp.png" alt="Chamar API usando autenticação do Aplicativo Web"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="Chamar API usando autenticação do Aplicativo Web">Invocar API usando autenticação de Aplicativo Web</a>
                    </p>
                    <p class="is-size-6">Saiba como chamar APIs do AEM baseadas em OpenAPI de um aplicativo web personalizado usando a autenticação do OAuth Web App.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

