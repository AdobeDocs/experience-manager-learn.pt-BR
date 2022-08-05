---
title: Implantações móveis sem periféricos AEM
description: Saiba mais sobre as considerações de implantação para implantações móveis sem cabeçalho AEM.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10796
thumbnail: KT-10796.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 2%

---


# Implantações móveis sem periféricos AEM

AEM implantações móveis headless são aplicativos móveis nativos para iOS, Android™ etc. que consomem e interagem com conteúdo em AEM sem interface.

As implantações móveis exigem configuração mínima, pois as conexões HTTP para AEM APIs sem cabeçalho não são iniciadas no contexto de um navegador.

## Configurações de implantação

A seguinte configuração de implantação deve estar no local para implantações de aplicativos móveis.

| O aplicativo móvel se conecta a | Autor do AEM | AEM Publish | Visualização de AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros do Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Compartilhamento de recursos entre origens (CORS) | ✘ | ✘ | ✘ |
| [AEM hosts](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Exemplo de aplicativos móveis

O Adobe fornece exemplos de aplicativos móveis iOS e Android™.

<div class="columns is-multiline">
    <!-- iOS app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="iOS app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/ios-swiftui-app.md" title="Aplicativo iOS" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/ios-swiftui-app/ios-app-card.png" alt="Aplicativo iOS">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/ios-swiftui-app.md" title="Aplicativo iOS">Aplicativo iOS</a></p>
                   <p class="is-size-6">Um exemplo de aplicativo iOS, gravado em SwiftUI, que consome conteúdo AEM APIs GraphQL sem interface.</p>
                   <a href="../example-apps/ios-swiftui-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exemplo de exibição</span>
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
                   <a href="../example-apps/android-app.md" title="Aplicativo Android™" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/android-java-app/android-app-card.png" alt="aplicativo Android">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/android-app.md" title="Aplicativo Android™">Aplicativo Android™</a></p>
                   <p class="is-size-6">Um exemplo de aplicativo Java™ Android™ que consome conteúdo AEM APIs GraphQL sem interface.</p>
                   <a href="../example-apps/android-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exemplo de exibição</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>


