---
title: Implantações AEM Headless
description: Saiba mais sobre as várias considerações de implantação para aplicativos AEM Headless.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
exl-id: 6de58ca0-9444-4272-9487-15a9e3c89231
duration: 59
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---

# Implantações AEM Headless

As implantações de cliente do AEM headless assumem muitas formas: AEM hospedado no SPA SPA, externo, site, aplicativo móvel ou até mesmo processo de servidor para servidor.

Dependendo do cliente e de como ele é implantado, as implantações sem periféricos de AEM têm considerações diferentes.

## Arquitetura de serviço do AEM

Antes de explorar as considerações sobre implantação, é fundamental compreender a arquitetura lógica do AEM, bem como a separação e as funções dos níveis de serviço da AEM as a Cloud Service. A AEM as a Cloud Service é composta de dois serviços lógicos:

+ O __AEM Author__ é o serviço no qual as equipes criam, colaboram e publicam fragmentos de conteúdo (e outros ativos).
+ O __AEM Publish__ é o serviço que publicou Os fragmentos de conteúdo (e outros ativos) são replicados para consumo geral.
+ __Visualização do AEM__ é o serviço que imita o AEM Publish no comportamento, mas tem conteúdo publicado para fins de visualização ou revisão. A Visualização do AEM é destinada a públicos-alvo internos, e não para a entrega geral de conteúdo. O uso da Visualização do AEM é opcional, com base no fluxo de trabalho desejado.

![Arquitetura do serviço AEM](./assets/overview/aem-service-architecture.png)

Arquitetura típica de implantação headless do AEM as a Cloud Service_

Os clientes AEM Headless que operam em uma capacidade de produção normalmente interagem com o AEM Publish, que contém o conteúdo publicado e aprovado. Os clientes que interagem com o AEM Author precisam ter cuidado especial, pois o AEM Author é seguro por padrão, exigindo autorização para todas as solicitações, e também pode ter trabalho em andamento ou conteúdo não aprovado.

## Implantações de clientes headless

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="Aplicativo de página única (SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="Aplicativos de página única (SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="Aplicativo de página única (SPA)">Aplicativo de página única (SPA)</a></p>
                   <p class="is-size-6">Saiba mais sobre as considerações de implantação para aplicativos de página única (SPA).</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aprender</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
<!-- Web component/JS -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web component/JS" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./web-component.md" title="Componente da Web/JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="Componente da Web/JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="Componente da Web/JS">Componente da Web/JS</a></p>
               <p class="is-size-6">Saiba mais sobre as considerações de implantação para componentes da Web e consumidores headless do JavaScript baseados em navegador.</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aprender</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Mobile apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Mobile apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./mobile.md" title="Aplicativos móveis" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/mobile/mobile-card.png" alt="Aplicativos móveis">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./mobile.md" title="Aplicativos móveis">Aplicativo móvel</a></p>
               <p class="is-size-6">Saiba mais sobre as considerações de implantação para aplicativos móveis.</p>
               <a href="./mobile.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aprender</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Server-to-server apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./server-to-server.md" title="Aplicativos de servidor para servidor" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/server-to-server/server-to-server-card.png" alt="Aplicativos de servidor para servidor">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./server-to-server.md" title="Aplicativos de servidor para servidor">Aplicativo de servidor para servidor</a></p>
               <p class="is-size-6">Saiba mais sobre as considerações de implantação para aplicativos de servidor para servidor</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aprender</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
