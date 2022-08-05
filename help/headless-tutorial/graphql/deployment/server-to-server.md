---
title: AEM implantações headless de servidor para servidor
description: Saiba mais sobre as considerações de implantação para implantações headless de AEM de servidor para servidor.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10798
thumbnail: kt-10798.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# AEM implantações headless de servidor para servidor

AEM As implantações headless de servidor para servidor envolvem aplicativos ou processos do lado do servidor que consomem e interagem com conteúdo em AEM de maneira headless.

As implantações de servidor para servidor exigem configuração mínima, pois as conexões HTTP para AEM APIs sem cabeçalho não são iniciadas no contexto de um navegador.

## Configurações de implantação

A seguinte configuração de implantação deve estar no local para implantações de aplicativos de servidor para servidor.

| O aplicativo servidor para servidor se conecta ao | Autor do AEM | AEM Publish | Visualização de AEM |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros do Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Compartilhamento de recursos entre origens (CORS) | ✘ | ✘ | ✘ |
| [AEM hosts](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Requisitos de autorização

Solicitações autorizadas para AEM APIs GraphQL normalmente ocorrem no contexto de aplicativos de servidor para servidor, já que outros tipos de aplicativos, como [aplicativos de página única](./spa.md), [dispositivo móvel](./mobile.md)ou [Componentes da Web](./web-component.md), normalmente usam a autorização , pois é difícil proteger as credenciais .

Ao autorizar solicitações para AEM as a Cloud Service, use [autenticação de token com base em credenciais de serviço](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html). Para saber mais sobre como autenticar solicitações para AEM as a Cloud Service, analise o [tutorial de autenticação baseada em token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). O tutorial explora a autenticação baseada em token usando [APIs HTTP do AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html) mas os mesmos conceitos e abordagens são aplicáveis a aplicativos que interagem com AEM APIs GraphQL sem periféricos.

## Exemplo de aplicativo servidor para servidor

O Adobe fornece um exemplo de aplicativo servidor para servidor codificado em Node.js.

<div class="columns is-multiline">
    <!-- Server-to-server app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/server-to-server-app.md" title="Aplicativo servidor a servidor" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/server-to-server-app/server-to-server-card.png" alt="Aplicativo servidor a servidor">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/server-to-server-app.md" title="Aplicativo servidor a servidor">Aplicativo servidor a servidor</a></p>
                   <p class="is-size-6">Um exemplo de aplicativo servidor para servidor, gravado em Node.js, que consome conteúdo AEM APIs GraphQL sem cabeçalho.</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exemplo de exibição</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>