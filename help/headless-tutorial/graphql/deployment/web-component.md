---
title: Implantações de Componente Web AEM Headless
description: Saiba mais sobre as considerações de implantação para implantações headless de AEM baseadas em Web Component/JS puro.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10797
thumbnail: kt-10797.jpg
exl-id: 9d4aab4c-82af-4917-8c1b-3935f19691e6
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 2%

---

# Implantações de Componente Web AEM Headless

AEM Headless [Componente da Web](https://developer.mozilla.org/en-US/docs/Web/Web_Components)As implantações do /JS são aplicativos JavaScript puros executados em um navegador da Web, que consomem e interagem com conteúdo no AEM de maneira headless. As implantações do componente da Web/JS são diferentes das [Implantações de SPA](./spa.md) na medida em que elas não usam uma estrutura robusta de SPA, e espera-se que sejam incorporadas ao contexto de qualquer site, para exibir o conteúdo do AEM.


## Configurações de implantação

A configuração de implantação a seguir deve estar em vigor para implantações de Componente web/JS.

| O componente Web/aplicativo JS se conecta a | Autor do AEM | AEM Publish | Visualização do AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros do Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Compartilhamento de recursos entre origens (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [Hospedeiros AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Exemplo de componente da Web

O Adobe fornece um exemplo de componente da Web.

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
                   <p class="is-size-6">Um componente da Web de exemplo, escrito em JavaScript puro, que consome conteúdo de APIs AEM Headless GraphQL.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir exemplo</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
