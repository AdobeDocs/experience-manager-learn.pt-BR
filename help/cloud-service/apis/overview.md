---
title: Visão geral das APIs do AEM
description: Saiba mais sobre os diferentes tipos de APIs no Adobe Experience Manager (AEM) e obtenha uma visão geral das APIs baseadas em especificação do OpenAPI, geralmente conhecidas como APIs do AEM baseadas em OpenAPI.
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2024-11-20T00:00:00Z
duration: 0
exl-id: 23b2be0d-a8d4-4521-96ba-78b70f4e9cba
source-git-commit: 2b5f7a033921270113eb7f41df33444c4f3d7723
workflow-type: tm+mt
source-wordcount: '1024'
ht-degree: 1%

---

# Visão geral das APIs do AEM{#aem-apis-overview}

Saiba mais sobre os diferentes tipos de APIs no Adobe Experience Manager (AEM) as a Cloud Service e obtenha uma visão geral das APIs do AEM AEM baseadas em [OpenAPI Specification (OAS)](https://swagger.io/specification/), geralmente conhecidas como APIs do baseadas em OpenAPI.

O AEM as a Cloud Service fornece uma grande variedade de APIs para criar, ler, atualizar e excluir conteúdo, ativos e formulários. Essas APIs permitem que os desenvolvedores criem aplicativos personalizados que interagem com o AEM.

Vamos explorar os diferentes tipos de APIs no AEM e entender os principais conceitos de acesso às APIs Adobe.

## Tipos de APIs de AEM{#types-of-aem-apis}

A AEM oferece APIs herdadas e modernas para interagir com seus tipos de serviço de autoria e publicação.

- **APIs herdadas**: introduzidas em versões anteriores do AEM, as APIs herdadas ainda são compatíveis com compatibilidade com versões anteriores.

- **APIs modernas**: com base na especificação REST e OpenAPI, essas APIs seguem as práticas recomendadas de design de API atuais e são recomendadas para novas integrações.


| Tipo de API AEM | Especificações | Disponibilidade | Caso de uso | Exemplo |
| --- | --- | --- | --- | --- |
| APIs tradicionais (não RESTful) | Sling Servlets | AEM 6.X, AEM as a Cloud Service | Integrações herdadas, compatibilidade com versões anteriores | [API do Construtor de Consultas](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api) e outras |
| APIs RESTful | HTTP, JSON | AEM 6.X, AEM as a Cloud Service | Operações CRUD, aplicativos modernos | [API HTTP do Assets](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), [API REST do fluxo de trabalho](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api), [Exportador JSON para Serviços de Conteúdo](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter) e outros |
| APIs do GraphQL | GraphQL | AEM 6.X, AEM as a Cloud Service | CMS headless, SPA, aplicativos móveis | [API GraphQL](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| APIs AEM baseadas em OpenAPI | REST, OpenAPI | **Somente AEM as a Cloud Service** | Desenvolvimento de API First, aplicativos modernos | [API do Autor do Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/), [API de Pastas](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/), [API do AEM Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/), [Forms Acrobat Services](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) e outros |

>[!IMPORTANT]
>
>As APIs de AEM baseadas em OpenAPI só estão disponíveis no AEM as a Cloud Service e não são compatíveis com o AEM 6.X.

Para obter mais detalhes sobre APIs AEM, consulte as [APIs Adobe Experience Manager as a Cloud Service](https://developer.adobe.com/experience-cloud/experience-manager-apis/).

Vamos analisar mais detalhadamente as APIs de AEM baseadas em OpenAPI e os importantes conceitos de acesso às APIs de Adobe.

## APIs AEM baseadas em OpenAPI{#openapi-based-aem-apis}

>[!AVAILABILITY]
>
>As APIs de AEM baseadas em OpenAPI estão disponíveis como parte de um programa de acesso antecipado. Se você estiver interessado em acessá-las, recomendamos enviar um email para [aem-apis@adobe.com](mailto:aem-apis@adobe.com) com uma descrição do seu caso de uso.

A [Especificação de OpenAPI](https://swagger.io/specification/) (anteriormente conhecida como Swagger) é um padrão amplamente usado para definir APIs RESTful. O AEM as a Cloud Service fornece várias APIs baseadas em especificação de OpenAPI (ou simplesmente APIs AEM baseadas em OpenAPI), facilitando a criação de aplicativos personalizados que interagem com os tipos de serviço de autoria ou publicação do AEM. Abaixo estão alguns exemplos:

**Sites**

- [APIs do Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/): APIs para trabalhar com Fragmentos de conteúdo.

**Assets**

- [API de pastas](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/): APIs para trabalhar com pastas como criar, listar e excluir pastas.

- [API do autor do Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/): APIs para trabalhar com ativos e seus metadados.

**Forms**

- [APIs de comunicações do Forms](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/): APIs para trabalhar com formulários e documentos.

Em versões futuras, mais APIs de AEM baseadas em OpenAPI serão adicionadas para oferecer suporte a casos de uso adicionais.

### Suporte à autenticação{#authentication-support}

As APIs de AEM baseadas em OpenAPI são compatíveis com os seguintes métodos de autenticação:

- **Credencial do servidor para servidor OAuth**: ideal para serviços de back-end que precisam de acesso à API sem interação com o usuário. Ele usa o tipo de concessão _client_credentials_, permitindo o gerenciamento de acesso seguro no nível do servidor. Para obter mais informações, consulte [Credencial do servidor para servidor do OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential).

- **Credencial do Aplicativo Web OAuth**: adequada para aplicativos Web com componentes de front-end e _back-end_ que acessam APIs AEM em nome dos usuários. Ela usa o tipo de concessão _authorization_code_, em que o servidor de back-end gerencia segredos e tokens de forma segura. Para obter mais informações, consulte [Credencial do aplicativo Web OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential).

- **Credencial de aplicativo de página única do OAuth**: projetada para o SPA em execução no navegador, que precisa acessar APIs em nome de um usuário sem um servidor back-end. Ele usa o tipo de concessão _authorization_code_ e depende de mecanismos de segurança do lado do cliente usando PKCE (Chave de Prova para Troca de Código) para proteger o fluxo do código de autorização. Para obter mais informações, consulte [Credencial de aplicativo de página única do OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential).

### Diferença entre credenciais de servidor para servidor do OAuth e do aplicativo da Web do OAuth/aplicativo de página única{#difference-between-oauth-server-to-server-and-oauth-web-app-single-page-app-credentials}

| | Servidor para servidor do OAuth | Autenticação de usuário OAuth (aplicativo web) |
| --- | --- | --- |
| Finalidade de autenticação | Projetado para interações máquina a máquina. | Projetado para interações orientadas pelo usuário. |
| Comportamento do token | Emite tokens de acesso que representam o próprio aplicativo cliente. | Emite tokens de acesso em nome de um usuário autenticado. |
| Casos de uso | Serviços de back-end que precisam de acesso à API sem interação com o usuário. | Aplicativos web com componentes de front-end e back-end acessando APIs em nome dos usuários. |
| Considerações sobre segurança | Armazene credenciais confidenciais (`client_id`, `client_secret`) com segurança em sistemas back-end. | O usuário se autentica e recebe seu próprio token de acesso temporário. Armazene credenciais confidenciais (`client_id`, `client_secret`) com segurança em sistemas back-end. |
| Tipo de concessão | _credenciais_do_cliente_ | _authorization_code_ |

### Acesso às APIs do Adobe e aos conceitos relacionados{#accessing-adobe-apis-and-related-concepts}

Antes de acessar as APIs Adobe, é essencial compreender estes conceitos principais:

- **[Adobe Developer Console](https://developer.adobe.com/)**: o hub do desenvolvedor para acessar APIs de Adobe, SDKs, eventos em tempo real, funções sem servidor e muito mais. Observe que é diferente do Developer Console _AEM_, que é usado para depurar aplicativos AEM.

- **[Projeto do Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/projects/)**: local central para gerenciar integrações de API, eventos e funções de tempo de execução. Aqui, você configura APIs, define autenticação e gera as credenciais necessárias.

- **[Perfis de Produtos](https://helpx.adobe.com/br/enterprise/using/manage-product-profiles.html)**: os Perfis de Produtos fornecem uma predefinição de permissão que permite controlar o acesso do usuário ou do aplicativo a produtos Adobe, como AEM, Adobe Target, Adobe Analytics e outros. Cada produto do Adobe tem perfis de produto predefinidos associados a ele.

- **Serviços**: os serviços definem as permissões reais e estão associados ao Perfil de Produto. Para reduzir ou aumentar a predefinição de permissões, é possível desmarcar ou selecionar os serviços associados ao Perfil do produto. Dessa forma, você pode controlar o nível de acesso ao produto e suas APIs. No AEM as a Cloud Service, os serviços representam grupos de usuários com Listas de controle de acesso (ACLs) predefinidas para nós de repositório, permitindo o gerenciamento granular de permissões.

## Próximas etapas{#next-steps}

Compreender os diferentes tipos de API do AEM, incluindo
APIs AEM baseadas em OpenAPI e os principais conceitos de acesso às APIs Adobe, agora você está pronto para começar a criar aplicativos personalizados que interagem com o AEM.

Vamos começar com:

- [Invocar APIs AEM baseadas em OpenAPI para autenticação de servidor para servidor](invoke-openapi-based-aem-apis.md), que demonstra como acessar APIs AEM baseadas em OpenAPI _usando credenciais OAuth de servidor para servidor_.
- [Invoque APIs AEM baseadas em OpenAPI com o tutorial de autenticação de usuário de um aplicativo Web](invoke-openapi-based-aem-apis-from-web-app.md), que demonstra como acessar APIs AEM baseadas em OpenAPI de um _aplicativo Web usando credenciais do OAuth Web App_.
