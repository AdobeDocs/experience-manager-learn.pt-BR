---
title: Implantação do AEM para SPA GraphQL
description: SPA Saiba mais sobre as considerações de implantação para implantações headless de AEM (aplicativo de página única).
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
exl-id: 3fe175f7-6213-439a-a02c-af3f82b6e3b7
duration: 136
source-git-commit: f1b13bba9e83ac1d25f2af23ff2673554726eb19
workflow-type: tm+mt
source-wordcount: '655'
ht-degree: 1%

---

# Implantações de AEM Headless SPA

As implantações de aplicativo de página única (SPA) do AEM envolvem aplicativos baseados em JavaScript AEM criados com o uso de estruturas como o React ou o Vue, que consomem e interagem com conteúdo no de forma headless.

Implantar um SPA que interage AEM de forma headless envolve hospedar o SPA e torná-lo acessível por meio de um navegador da web.

## Hospede o SPA

Um SPA é composto de uma coleção de recursos nativos da Web: **HTML, CSS e JavaScript**. Esses recursos são gerados durante o processo de _compilação_ (por exemplo, `npm run build`) e implantados em um host para consumo pelos usuários finais.

Há várias opções de **hospedagem** dependendo das necessidades da sua organização:

1. **Provedores de nuvem**, como **Azure** ou **AWS**.

2. **Hospedagem no local** em um **data center** corporativo

3. **Plataformas de hospedagem front-end**, como **AWS Amplify**, **Azure App Service**, **Netlify**, **Heroku**, **Vercel** etc.

## Configurações de implantação

A principal consideração ao hospedar um SPA que interage com o AEM headless é se o SPA é acessado por meio do domínio do AEM (ou host) ou em um domínio diferente.  O motivo é que o SPA são aplicações Web executadas em navegadores da Web e, portanto, estão sujeitas a políticas de segurança de navegadores da Web.

### Domínio compartilhado

Um AEM e SPA compartilham domínios quando ambos são acessados por usuários finais do mesmo domínio. Por exemplo:

+ AEM acessado via: `https://wknd.site/`
+ SPA acessado via `https://wknd.site/spa`

Como AEM e SPA são acessados do mesmo domínio, os navegadores da Web permitem que o SPA faça XHR para o AEM AEM Headless endpoints sem a necessidade do CORS e permitem o compartilhamento de cookies HTTP (como o cookie do `login-token`).

Como o tráfego de AEM e SPA é roteado no domínio compartilhado, depende de você: CDN com várias origens, servidor HTTP com proxy reverso, hospedagem do SPA diretamente no AEM e assim por diante.

Abaixo estão as configurações de implantação necessárias para implantações de produção de SPA, quando hospedadas no mesmo domínio que AEM.

| SPA se conecta a → | Autor do AEM | Publicação no AEM | Visualização do AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros do Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Compartilhamento de recursos entre origens (CORS) | ✘ | ✘ | ✘ |
| Hospedeiros AEM | ✘ | ✘ | ✘ |

### Domínios diferentes

Um AEM e SPA têm domínios diferentes quando são acessados por usuários finais de um domínio diferente. Por exemplo:

+ AEM acessado via: `https://wknd.site/`
+ SPA acessado via `https://wknd-app.site/`

Como o AEM e o SPA são acessados de domínios diferentes, os navegadores da Web impõem políticas de segurança, como o [CORS (cross-origin resource sharing, compartilhamento de recursos entre origens)](./configurations/cors.md), e impedem o compartilhamento de cookies HTTP (como o cookie AEM `login-token`).

Abaixo estão as configurações de implantação necessárias para implantações de produção de SPA, quando hospedadas em um domínio diferente do AEM.

| SPA se conecta a → | Autor do AEM | Publicação no AEM | Visualização do AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros do Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Compartilhamento de recursos entre origens (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [Hosts AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### Exemplo de implantação do SPA em diferentes domínios

Neste exemplo, o SPA é implantado em um domínio Netlify (`https://main--sparkly-marzipan-b20bf8.netlify.app/`) e o SPA consome APIs AEM GraphQL AEM do domínio Publish (`https://publish-p65804-e666805.adobeaemcloud.com`). As capturas de tela abaixo destacam o requisito do CORS.

1. O SPA é disponibilizado a partir de um domínio Netlify, mas faz uma chamada XHR para APIs AEM GraphQL em um domínio diferente. Esta solicitação entre sites requer que o [CORS](./configurations/cors.md) seja configurado no AEM para permitir que a solicitação do domínio Netlify acesse seu conteúdo.

   ![Solicitação de SPA veiculada a partir dos hosts SPA e AEM ](assets/spa/cors-requirement.png)

2. Inspecionando a solicitação XHR para a API AEM GraphQL, o `Access-Control-Allow-Origin` está presente, indicando ao navegador da web que o AEM permite que a solicitação desse domínio Netlify acesse seu conteúdo.

   Se o AEM [CORS](./configurations/cors.md) estivesse ausente ou não incluísse o domínio Netlify, o navegador da Web falharia na solicitação XHR e relataria um erro do CORS.

   ![API AEM GraphQL do Cabeçalho de Resposta do CORS](assets/spa/cors-response-headers.png)

## Exemplo de aplicativo de página única

Adobe fornece um exemplo de aplicativo de página única codificado no React.

<div class="columns is-multiline">
<!-- React app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/react-app.md" title="aplicativo React" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app-card.png" alt="aplicativo React">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/react-app.md" title="aplicativo React">aplicativo React</a></p>
               <p class="is-size-6">Um exemplo de aplicativo de página única, escrito no React, que consome conteúdo de APIs AEM Headless GraphQL.</p>
               <a href="../example-apps/react-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir exemplo</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Next.js app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Next.js app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/next-js.md" title="Aplicativo Next.js" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/next-js/next-js-card.png" alt="Aplicativo Next.js">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/next-js.md" title="Aplicativo Next.js">Aplicativo Next.js</a></p>
               <p class="is-size-6">Um aplicativo de página única de exemplo, escrito em Next.js, que consome conteúdo de APIs AEM Headless GraphQL.</p>
               <a href="../example-apps/next-js.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir exemplo</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
