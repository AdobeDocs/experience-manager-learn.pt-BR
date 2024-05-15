---
title: Armazenamento em cache as a Cloud Service do AEM
description: Visão geral do armazenamento em cache as a Cloud Service do AEM.
version: Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: e76ed4c5-3220-4274-a315-a75e549f8b40
duration: 36
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 0%

---

# Armazenamento em cache as a Cloud Service do AEM

No AEM as a Cloud Service, entender o armazenamento em cache é fundamental. O armazenamento em cache envolve o armazenamento e a reutilização de dados obtidos anteriormente para melhorar a eficiência do sistema e reduzir os tempos de carga. Esse mecanismo acelera significativamente a entrega de conteúdo, aumenta o desempenho do site e otimiza a experiência do usuário.

O AEM as a Cloud Service tem várias camadas de armazenamento em cache e estratégias diferentes entre os serviços de Autor e Publicação.

![Visão geral do armazenamento em cache as a Cloud Service do AEM](./assets/overview/all.png){align="center"}

## Armazenamento em cache do AEM

O AEM as a Cloud Service tem uma estratégia robusta e configurável de armazenamento em cache de várias camadas, incluindo um CDN, AEM Dispatcher e, opcionalmente, um CDN gerenciado pelo cliente. O armazenamento em cache entre camadas pode ser ajustado para otimizar o desempenho, garantindo que o AEM forneça apenas as melhores experiências. O AEM tem diferentes preocupações com o armazenamento em cache para os serviços de Autor e Publicação. Explore as estratégias de armazenamento em cache para cada serviço abaixo.


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="Serviço de publicação do AEM" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="Armazenamento em cache do serviço de publicação do AEM">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="Armazenamento em cache do serviço de publicação do AEM">Armazenamento em cache do serviço de publicação do AEM</a></p>
            <p class="is-size-6">O serviço de Publicação do AEM AEM usa um CDN e um Dispatcher gerenciados para otimizar as experiências da Web do usuário final.</p>
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
                <a href="./author.md" title="Armazenamento em cache do serviço do autor no AEM" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="Armazenamento em cache do serviço do autor no AEM">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="Armazenamento em cache do serviço do autor no AEM">Armazenamento em cache do serviço do autor no AEM</a></p>
                <p class="is-size-6">O serviço de Autor do AEM usa uma CDN gerenciada para fornecer experiências de criação otimizadas.</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
            </div>
        </div>
    </div>
</div>
