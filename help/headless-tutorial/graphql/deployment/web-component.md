---
title: AEM implantações de componentes web headless
description: Saiba mais sobre as considerações de implantação para o Componente da Web/implantações de AEM headless puras baseadas em JS.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 2%

---


# AEM implantações de componentes web headless

AEM Cabeça [Componente Web](https://developer.mozilla.org/en-US/docs/Web/Web_Components)As implantações de /JS são aplicativos JavaScript puros executados em um navegador da Web, que consomem e interagem com o conteúdo no AEM de maneira headless. As implantações de Componente Web/JS diferem de [SPA implantações](./spa.md) na medida em que eles não usam uma estrutura de SPA robusta e esperam que sejam incorporados no contexto de qualquer site, do delivery, para o conteúdo da AEM.


## Configurações de implantação

A seguinte configuração de implantação deve estar no local para implantações de Componente Web/JS.

| O aplicativo Web Component/JS se conecta a | Autor do AEM | AEM Publish | Visualização de AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros do Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Compartilhamento de recursos entre origens (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM hosts](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Exemplo de componente da Web

O Adobe fornece um exemplo de Componente da Web.

<div class="columns is-multiline">
    <!-- Web Component -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/web-component.md" title="Componente Web" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/web-component/web-component-card.png" alt="Componente Web">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/web-component.md" title="Componente Web">Componente Web</a></p>
                   <p class="is-size-6">Um exemplo de Componente da Web, escrito em JavaScript puro, que consome conteúdo AEM APIs GraphQL sem periféricos.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exemplo de exibição</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
