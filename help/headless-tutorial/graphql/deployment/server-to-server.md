---
title: Implantações AEM headless de servidor para servidor
description: Saiba mais sobre as considerações de implantação para implantações headless de AEM de servidor para servidor.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10798
thumbnail: kt-10798.jpg
exl-id: d4ae08d9-dc43-4414-ab75-26853186a301
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---

# Implantações AEM headless de servidor para servidor

As implantações do AEM headless de servidor para servidor envolvem aplicativos ou processos do lado do servidor que consomem e interagem com conteúdo no AEM de forma headless.

As implantações de servidor para servidor exigem configuração mínima, já que as conexões HTTP para APIs AEM Headless não são iniciadas no contexto de um navegador.

## Configurações de implantação

A configuração de implantação a seguir deve estar em vigor para implantações de aplicativo de servidor para servidor.

| O aplicativo de servidor para servidor se conecta ao | Autor do AEM | AEM Publish | Visualização do AEM |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros do Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Compartilhamento de recursos entre origens (CORS) | ✘ | ✘ | ✘ |
| [Hospedeiros AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Requisitos de autorização

As solicitações autorizadas para APIs do GraphQL com AEM normalmente ocorrem no contexto de aplicativos de servidor para servidor, já que outros tipos de aplicativos, como [aplicativos de página única](./spa.md), [dispositivo móvel](./mobile.md)ou [Componentes da Web](./web-component.md), normalmente usam autorização, pois é difícil proteger as credenciais.

Ao autorizar solicitações para o AEM as a Cloud Service, use [autenticação de token baseada em credenciais de serviço](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html). Para saber mais sobre como autenticar solicitações para o AEM as a Cloud Service, reveja a [tutorial de autenticação baseada em token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). O tutorial explora a autenticação baseada em token usando [APIs HTTP do AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html) mas os mesmos conceitos e abordagens são aplicáveis a aplicativos que interagem com APIs AEM Headless GraphQL.

## Exemplo de aplicativo de servidor para servidor

O Adobe fornece um exemplo de aplicativo servidor a servidor codificado em Node.js.

<div class="columns is-multiline">
    <!-- Server-to-server app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/server-to-server-app.md" title="Aplicativo de servidor para servidor" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/server-to-server-app/server-to-server-card.png" alt="Aplicativo de servidor para servidor">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/server-to-server-app.md" title="Aplicativo de servidor para servidor">Aplicativo de servidor para servidor</a></p>
                   <p class="is-size-6">Um exemplo de aplicativo de servidor para servidor, escrito em Node.js, que consome conteúdo das APIs do GraphQL do AEM Headless.</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir exemplo</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
