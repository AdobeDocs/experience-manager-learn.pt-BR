---
title: Implantação de SPA para o AEM GraphQL
description: Saiba mais sobre as considerações de implantação para implantações headless de aplicativo de página única (SPA) do AEM.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
exl-id: 3fe175f7-6213-439a-a02c-af3f82b6e3b7
duration: 136
source-git-commit: 6425188da75f789b0661ec9bfb79624b5704c92b
workflow-type: tm+mt
source-wordcount: '640'
ht-degree: 5%

---

# Implantações de SPA do AEM Headless

As implantações de aplicativo de página única (SPA) do AEM Headless envolvem aplicativos do JavaScript criados com o uso de estruturas como o React ou o Vue, que consomem e interagem com conteúdo no AEM de forma headless.

A implantação de um SPA que interage com o AEM de forma headless envolve hospedar o SPA e torná-lo acessível por meio de um navegador da Web.

## Hospede o SPA

Um SPA é composto de uma coleção de recursos nativos da Web: **HTML, CSS e JavaScript**. Esses recursos são gerados durante o processo de _compilação_ (por exemplo, `npm run build`) e implantados em um host para consumo pelos usuários finais.

Há várias opções de **hospedagem** dependendo das necessidades da sua organização:

1. **Provedores de nuvem**, como **Azure** ou **AWS**.

2. **Hospedagem no local** em um **data center** corporativo

3. **Plataformas de hospedagem front-end**, como **AWS Amplify**, **Azure App Service**, **Netlify**, **Heroku**, **Vercel** etc.

## Configurações de implantação

A principal consideração ao hospedar um SPA que interage com o AEM headless é se o SPA é acessado por meio do domínio (ou host) do AEM ou em um domínio diferente.  O motivo é que os SPAs são aplicativos da Web em execução em navegadores da Web e, portanto, estão sujeitos às políticas de segurança dos navegadores da Web.

### Domínio compartilhado

Um SPA e o AEM compartilham domínios quando ambos são acessados por usuários finais do mesmo domínio. Por exemplo:

+ AEM acessado via: `https://wknd.site/`
+ SPA acessado via `https://wknd.site/spa`

Como o AEM e o SPA são acessados do mesmo domínio, os navegadores da Web permitem que o SPA faça XHR nos pontos de extremidade headless do AEM sem a necessidade do CORS e permitem o compartilhamento de cookies HTTP (como o cookie `login-token` do AEM).

Como o tráfego de SPA e AEM é roteado no domínio compartilhado depende de você: CDN com várias origens, servidor HTTP com proxy reverso, hospedagem do SPA diretamente no AEM e assim por diante.

Abaixo estão as configurações de implantação necessárias para implantações de produção de SPA, quando hospedadas no mesmo domínio que o AEM.

| SPA conecta-se a → | Autor do AEM | AEM Publish | Visualização do AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros do Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Compartilhamento de recursos entre origens (CORS) | ✘ | ✘ | ✘ |
| Hosts do AEM | ✘ | ✘ | ✘ |

### Domínios diferentes

Um SPA e o AEM têm domínios diferentes quando são acessados por usuários finais de um domínio diferente. Por exemplo:

+ AEM acessado via: `https://wknd.site/`
+ SPA acessado via `https://wknd-app.site/`

Como o AEM e o SPA são acessados de domínios diferentes, os navegadores da Web impõem políticas de segurança, como o [CORS (compartilhamento de recursos entre origens)](./configurations/cors.md), e impedem o compartilhamento de cookies HTTP (como o cookie `login-token` do AEM).

Abaixo estão as configurações de implantação necessárias para implantações de produção de SPA, quando hospedadas em um domínio diferente do AEM.

| SPA conecta-se a → | Autor do AEM | AEM Publish | Visualização do AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros do Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Compartilhamento de recursos entre origens (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [Hosts AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### Exemplo de implantação de SPA em domínios diferentes

Neste exemplo, o SPA é implantado em um domínio Netlify (`https://main--sparkly-marzipan-b20bf8.netlify.app/`) e o SPA consome APIs AEM GraphQL do domínio de publicação AEM (`https://publish-p65804-e666805.adobeaemcloud.com`). As capturas de tela abaixo destacam o requisito do CORS.

1. O SPA é disponibilizado a partir de um domínio Netlify, mas faz uma chamada XHR para as APIs do AEM GraphQL em um domínio diferente. Esta solicitação entre sites requer que o [CORS](./configurations/cors.md) seja configurado no AEM para permitir que a solicitação do domínio Netlify acesse seu conteúdo.

   ![Solicitação de SPA atendida de hosts SPA e AEM ](assets/spa/cors-requirement.png)

2. Inspecionando a solicitação XHR para a API GraphQL do AEM, o `Access-Control-Allow-Origin` está presente, indicando ao navegador da Web que o AEM permite que a solicitação deste domínio Netlify acesse seu conteúdo.

   Se o AEM [CORS](./configurations/cors.md) estiver ausente ou não incluir o domínio Netlify, o navegador da Web falhará a solicitação XHR e relatará um erro do CORS.

   ![API GraphQL do AEM de Cabeçalho de Resposta do CORS](assets/spa/cors-response-headers.png)

## Exemplo de aplicativo de página única

O Adobe fornece um exemplo de aplicativo de página única codificado no React.

<!-- CARDS 

* ../example-apps/react-app.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React App - AEM Headless Example">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../example-apps/react-app.md" title="Aplicativo React - Exemplo de AEM Headless" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app.png" alt="Aplicativo React - Exemplo de AEM Headless"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../example-apps/react-app.md" target="_blank" rel="referrer" title="Aplicativo React - Exemplo de AEM Headless">Aplicativo React - Exemplo do AEM Headless</a>
                    </p>
                    <p class="is-size-6">Os exemplos de aplicativos são uma ótima maneira de conhecer os recursos sem cabeçalho do Adobe Experience Manager (AEM). Este aplicativo React demonstra como consultar conteúdo usando as APIs GraphQL do AEM usando consultas persistentes.</p>
                </div>
                <a href="../example-apps/react-app.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


