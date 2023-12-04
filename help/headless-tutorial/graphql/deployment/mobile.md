---
title: Implantações móveis AEM Headless
description: Saiba mais sobre as considerações de implantação para implantações móveis do AEM Headless.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10796
thumbnail: KT-10796.jpg
exl-id: 1f536079-b3ce-4807-be88-804378e75d37
duration: 84
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 2%

---

# Implantações móveis AEM Headless

As implantações móveis sem periféricos de AEM são aplicativos móveis nativos para iOS, Android™ etc. que consomem e interagem com conteúdo no AEM de forma headless.

As implantações móveis exigem configuração mínima, pois as conexões HTTP para APIs AEM Headless não são iniciadas no contexto de um navegador.

## Configurações de implantação

A configuração de implantação a seguir deve estar em vigor para implantações de aplicativos móveis.

| O aplicativo móvel se conecta ao | Autor do AEM | AEM Publish | Visualização do AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros do Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Compartilhamento de recursos entre origens (CORS) | ✘ | ✘ | ✘ |
| [Hospedeiros AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Exemplo de aplicativos móveis

O Adobe fornece exemplos de aplicativos móveis iOS e Android™.

<div class="columns is-multiline">
    <!-- iOS app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="iOS app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/ios-swiftui-app.md" title="aplicativo iOS" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/ios-swiftui-app/ios-app-card.png" alt="aplicativo iOS">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/ios-swiftui-app.md" title="aplicativo iOS">aplicativo iOS</a></p>
                   <p class="is-size-6">Um aplicativo de exemplo do iOS, escrito em SwiftUI, que consome conteúdo de APIs do AEM Headless GraphQL.</p>
                   <a href="../example-apps/ios-swiftui-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir exemplo</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
    <!-- Android app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Android app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/android-app.md" title="aplicativo Android™" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/android-java-app/android-app-card.png" alt="aplicativo Android">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/android-app.md" title="aplicativo Android™">aplicativo Android™</a></p>
                   <p class="is-size-6">Um exemplo de aplicativo Java™ Android™ que consome conteúdo de APIs AEM Headless GraphQL.</p>
                   <a href="../example-apps/android-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir exemplo</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
