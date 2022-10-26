---
title: AEM implantações headless
description: Saiba mais sobre as várias considerações de implantação para AEM aplicativos headless.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---


# AEM implantações headless

AEM as implantações de clientes headless assumem várias formas; SPA hospedados pela AEM, SPA externos, site, aplicativo móvel ou até mesmo processo servidor a servidor.

Dependendo do cliente e de como ele é implantado, AEM as implantações headless têm diferentes considerações.

## Arquitetura de serviço AEM

Antes de explorar as considerações sobre implantação, é imperativo entender AEM arquitetura lógica e a separação e as funções dos níveis de serviço AEM as a Cloud Service. AEM as a Cloud Service é composto por dois serviços lógicos:

+ __Autor do AEM__ é o serviço onde as equipes criam, colaboram e publicam Fragmentos de conteúdo (e outros ativos).
+ __Publicação do AEM__ é o serviço que foi publicado e os Fragmentos de conteúdo (e outros ativos) são replicados para consumo geral.
+ __Visualização de AEM__ é o serviço que imita o AEM Publish no comportamento, mas tem conteúdo publicado nele para fins de visualização ou revisão. AEM a Visualização destina-se a públicos internos, e não para a entrega geral de conteúdo. O uso de Visualização de AEM é opcional, com base no fluxo de trabalho desejado.

![Arquitetura de serviço AEM](./assets/overview/aem-service-architecture.png)

Arquitetura de implantação normal típica AEM as a Cloud Service

AEM clientes headless operando em uma capacidade de produção normalmente interagem com o AEM Publish, que contém o conteúdo aprovado e publicado. Os clientes que interagem com o autor do AEM precisam ter cuidado especial, pois o autor do AEM é seguro por padrão, requer autorização para todas as solicitações e também pode conter trabalho em andamento ou conteúdo não aprovado.

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
               <a href="./web-component.md" title="Componente da Web/JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="Componente da Web/JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="Componente da Web/JS">Componente da Web/JS</a></p>
               <p class="is-size-6">Saiba mais sobre as considerações de implantação para componentes da Web e consumidores sem periféricos de JavaScript baseados em navegador.</p>
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
               <a href="./mobile.md" title="Aplicativos para dispositivos móveis" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/mobile/mobile-card.png" alt="Aplicativos para dispositivos móveis">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./mobile.md" title="Aplicativos para dispositivos móveis">Aplicativo móvel</a></p>
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
               <p class="headline is-size-6 has-text-weight-bold"><a href="./server-to-server.md" title="Aplicativos de servidor para servidor">Aplicativo servidor a servidor</a></p>
               <p class="is-size-6">Saiba mais sobre as considerações de implantação para aplicativos de servidor para servidor</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>