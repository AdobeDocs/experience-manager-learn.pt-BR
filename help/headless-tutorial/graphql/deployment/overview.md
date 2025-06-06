---
title: Implantações do AEM Headless
description: Saiba mais sobre as várias considerações de implantação para aplicativos do AEM Headless.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
exl-id: 6de58ca0-9444-4272-9487-15a9e3c89231
duration: 59
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '315'
ht-degree: 100%

---

# Implantações do AEM Headless

As implantações de clientes do AEM Headless assumem muitas formas; SPA hospedado no AEM, SPA externo, site, aplicativo móvel ou até mesmo processo de servidor para servidor.

Dependendo do cliente e de como ele é implantado, as implantações do AEM Headless implicam considerações diferentes.

## Arquitetura de serviço do AEM

Antes de ver as considerações de implantação, é fundamental compreender a arquitetura lógica do AEM, bem como a separação e as funções dos níveis de serviço do AEM as a Cloud Service. O AEM as a Cloud Service consiste em dois serviços lógicos:

+ O __AEM Author__ é o serviço no qual as equipes criam, colaboram e publicam fragmentos de conteúdo (e outros ativos).
+ O __AEM Publish__ é o serviço no qual os fragmentos de conteúdo (e outros ativos) publicados são replicados para consumo geral.
+ O __AEM Preview__ é o serviço que imita o comportamento do AEM Publish, mas o conteúdo é publicado nele para fins de visualização ou revisão. O AEM Preview destina-se a públicos-alvo internos, não à entrega de conteúdo geral. O uso do AEM Preview é opcional, dependendo do fluxo de trabalho desejado.

![Arquitetura de serviço do AEM](./assets/overview/aem-service-architecture.png)

Arquitetura típica de implantação headless do AEM as a Cloud Service_

Os clientes do AEM Headless que operam em uma capacidade de produção normalmente interagem com o AEM Publish, que contém o conteúdo publicado e aprovado. Os clientes que interagem com o AEM Author precisam ter cuidado especial, pois o AEM Author é seguro por padrão, exigindo autorização para todas as solicitações, e também pode ter trabalhos em andamento ou conteúdo não aprovado.

## Implantações headless de clientes

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
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
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
               <a href="./web-component.md" title="Componente da web/JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="Componente da web/JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="Componente da web/JS">Componente da web/JS</a></p>
               <p class="is-size-6">Saiba mais sobre as considerações de implantação para componentes da web e consumidores headless do JavaScript baseados em navegadores.</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
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
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
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
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
