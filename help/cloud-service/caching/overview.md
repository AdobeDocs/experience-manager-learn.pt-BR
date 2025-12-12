---
title: Armazenamento em cache do AEM as a Cloud Service
description: Visão geral do armazenamento em cache do AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: e76ed4c5-3220-4274-a315-a75e549f8b40
duration: 36
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 100%

---

# Armazenamento em cache do AEM as a Cloud Service

No AEM as a Cloud Service, é fundamental entender o armazenamento em cache. O armazenamento em cache envolve o armazenamento e a reutilização de dados obtidos anteriormente para melhorar a eficiência do sistema e reduzir os tempos de carga. Esse mecanismo acelera significativamente a entrega de conteúdo, aumenta o desempenho do site e otimiza a experiência do usuário.

O AEM as a Cloud Service tem várias camadas de armazenamento em cache e estratégias diferentes entre os serviços de criação e publicação.

![Visão geral do armazenamento em cache do AEM as a Cloud Service](./assets/overview/all.png){align="center"}

## Armazenamento em cache do AEM

O AEM as a Cloud Service tem uma estratégia de armazenamento em cache multicamadas robusta e configurável, incluindo uma CDN, AEM Dispatcher e, opcionalmente, uma CDN gerenciada pelo cliente. O armazenamento em cache entre camadas pode ser ajustado para otimizar o desempenho, garantindo que o AEM proporcione apenas as melhores experiências. O AEM tem um foco diferente em relação ao armazenamento em cache para os serviços de criação e publicação. Familiarize-se com as estratégias de armazenamento em cache de cada serviço abaixo.


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="Serviço do AEM Publish" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="Armazenamento em cache do serviço de publicação do AEM">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="Armazenamento em cache do serviço de publicação do AEM">Armazenamento em cache do serviço de publicação do AEM</a></p>
            <p class="is-size-6">O serviço de publicação do AEM usa uma CDN gerenciada e o AEM Dispatcher para otimizar as experiências da web do usuário final.</p>
            <a href="./publish.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
              <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
            </a>
          </div>
        </div>
      </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Author service caching">
        <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
            <div class="card-image">
            <figure class="image is-16by9">
                <a href="./author.md" title="Armazenamento em cache do serviço de criação do AEM" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="Armazenamento em cache do serviço de criação do AEM">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="Armazenamento em cache do serviço de criação do AEM">Armazenamento em cache do serviço de criação do AEM</a></p>
                <p class="is-size-6">O serviço de criação do AEM usa uma CDN gerenciada para fornecer experiências de criação otimizadas.</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
            </div>
        </div>
    </div>
</div>
