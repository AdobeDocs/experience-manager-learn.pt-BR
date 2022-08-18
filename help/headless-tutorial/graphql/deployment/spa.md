---
title: Implantação de SPA para AEM GraphQL
description: Saiba mais sobre as considerações de implantação para aplicativos de página única (SPA) AEM implantações headless.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: 34fbb22916cf8a8df0e3240835c71e0979fd11bd
workflow-type: tm+mt
source-wordcount: '612'
ht-degree: 1%

---


# AEM implantações de SPA headless


AEM implantações de aplicativos de página única (SPA) headless envolvem aplicativos baseados em JavaScript criados com estruturas como o React ou o Vue, que consomem e interagem com conteúdo em AEM sem interface.

Implantar um SPA que interage AEM de maneira headless envolve hospedar o SPA e torná-lo acessível por meio de um navegador da Web.

## Hospede o SPA

Um SPA é composto por uma coleção de recursos nativos da Web: **HTML, CSS e JavaScript**. Esses recursos são gerados durante a _build_ process (por exemplo, `npm run build`) e implantado em um host para consumo por usuários finais.

Há vários **hospedagem** opções dependendo dos requisitos da organização:

1. **Provedores na nuvem** como **Azure** ou **AWS**.

2. **No local** hospedagem em uma empresa **data center**

3. **Plataformas de hospedagem front-end** como **Ampliação do AWS**, **Serviço de Aplicativo do Azure**, **Netlify**, **Heroku**, **Vercel**, etc.

## Configurações de implantação

A principal consideração ao hospedar um SPA que interage com AEM headless é se o SPA é acessado por AEM domínio (ou host) ou em um domínio diferente.  O motivo é que SPA são aplicativos da Web em execução em navegadores da Web e, portanto, estão sujeitos às políticas de segurança dos navegadores da Web.

### Domínio compartilhado

Um SPA e AEM compartilham domínios quando ambos são acessados por usuários finais do mesmo domínio. Por exemplo:

+ AEM é acessado via: `https://wknd.site/`
+ SPA é acessado via `https://wknd.site/spa`

Como AEM e o SPA são acessados do mesmo domínio, os navegadores da Web permitem que o SPA faça XHR para AEM endpoints sem cabeçalho sem a necessidade do CORS e permitem o compartilhamento de cookies HTTP (como AEM `login-token` cookie).

A forma como o tráfego de SPA e AEM é roteado no domínio compartilhado depende de você: CDN com várias origens, servidor HTTP com proxy reverso, hospedando o SPA diretamente em AEM e assim por diante.

Abaixo estão as configurações de implantação necessárias para implantações de produção de SPA, quando hospedadas no mesmo domínio AEM.

| SPA se conecta a | Autor do AEM | AEM Publish | Visualização de AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros do Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Compartilhamento de recursos entre origens (CORS) | ✘ | ✘ | ✘ |
| AEM hosts | ✘ | ✘ | ✘ |

### Domínios diferentes

Um SPA e AEM têm domínios diferentes quando são acessados por usuários finais de um domínio diferente. Por exemplo:

+ AEM é acessado via: `https://wknd.site/`
+ SPA é acessado via `https://wknd-app.site/`

Como o AEM e o SPA são acessados de domínios diferentes, os navegadores da Web impõem políticas de segurança como [compartilhamento de recursos entre origens (CORS)](./configurations/cors.md)e impedir o compartilhamento de cookies HTTP (como AEM `login-token` cookie).

Abaixo estão as configurações de implantação necessárias para implantações de produção de SPA, quando hospedadas em um domínio diferente de AEM.

| SPA se conecta a | Autor do AEM | Publicação do AEM | Visualização de AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros do Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Compartilhamento de recursos entre origens (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM hosts](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### Exemplo SPA implantação em domínios diferentes

Neste exemplo, o SPA é implantado em um domínio Netfy (`https://main--sparkly-marzipan-b20bf8.netlify.app/`) e o SPA consome APIs GraphQL AEM do domínio de publicação do AEM (`https://publish-p65804-e666805.adobeaemcloud.com`). As capturas de tela abaixo destacam o requisito do CORS.

1. O SPA é veiculado a partir de um domínio Netfy, mas faz uma chamada XHR para AEM APIs GraphQL em um domínio diferente. Esta solicitação entre sites requer [CORS](./configurations/cors.md) a ser configurado no AEM para permitir a solicitação do domínio Netfy para acessar seu conteúdo.

   ![Solicitação de SPA servida de hosts SPA e AEM ](assets/spa/cors-requirement.png)

2. Inspecionando a solicitação XHR para a API GraphQL AEM, a variável `Access-Control-Allow-Origin` está presente, indicando para o navegador da Web que AEM permite que a solicitação deste domínio Netfy acesse seu conteúdo.

   Se a AEM [CORS](./configurations/cors.md) estava ausente ou não incluía o domínio Netfy, o navegador da Web falhava na solicitação XHR e relatava um erro de CORS.

   ![Cabeçalho de resposta do CORS AEM API GraphQL](assets/spa/cors-response-headers.png)

## Exemplo de aplicativo de página única

O Adobe fornece um exemplo de aplicativo de página única codificado no React.

<div class="columns is-multiline">
<!-- React app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/react-app.md" title="Reagir aplicativo" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app-card.png" alt="Reagir aplicativo">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/react-app.md" title="Reagir aplicativo">Reagir aplicativo</a></p>
               <p class="is-size-6">Um exemplo de aplicativo de página única, escrito no React, que consome conteúdo AEM APIs GraphQL sem interface.</p>
               <a href="../example-apps/react-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exemplo de exibição</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
