---
title: Implantações do componente Web headless do AEM
description: Saiba mais sobre as considerações de implantação para implantações headless do AEM baseadas em Web Component/JS puro.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
jira: KT-10797
thumbnail: kt-10797.jpg
exl-id: 9d4aab4c-82af-4917-8c1b-3935f19691e6
duration: 31
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 7%

---

# Implantações do componente Web headless do AEM

As implantações do [Componente Web](https://developer.mozilla.org/en-US/docs/Web/Web_Components)/JS do AEM Headless são aplicativos puros do JavaScript executados em um navegador da Web, que consomem e interagem com conteúdo no AEM de forma headless. As implantações de Componentes Web/JS são diferentes das [implantações de SPA](./spa.md), pois não usam uma estrutura de SPA robusta e espera-se que sejam incorporadas ao contexto de qualquer site, sejam entregues e exibam conteúdo do AEM.


## Configurações de implantação

A configuração de implantação a seguir deve estar em vigor para implantações de Componente web/JS.

| O componente Web/aplicativo JS se conecta a → | Autor do AEM | AEM Publish | Visualização do AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros do Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Compartilhamento de recursos entre origens (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [Hosts AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Exemplo de componente da Web

O Adobe fornece um exemplo de componente Web.

<div class="columns is-multiline">
    <!-- Web Component -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/web-component.md" title="Componente da Web" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/web-component/web-component-card.png" alt="Componente da Web">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/web-component.md" title="Componente da Web">Componente da Web</a></p>
                   <p class="is-size-6">Um componente da Web de exemplo, escrito em JavaScript puro, que consome conteúdo das APIs do AEM Headless GraphQL.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir exemplo</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
