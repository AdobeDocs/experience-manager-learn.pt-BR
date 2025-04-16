---
title: Vídeos e tutoriais do AEM Sites
description: O Adobe Experience Manager (AEM) Sites é uma plataforma de gerenciamento de experiência líder. Este guia do usuário contém vídeos e tutoriais sobre os vários recursos e funcionalidades do AEM Sites.
solution: Experience Manager, Experience Manager Sites
sub-product: Experience Manager Sites
topic: Content Management
doc-type: Catalog
exl-id: cde4ce7f-0afe-4632-8c1c-354586f296d5
source-git-commit: 5bd752ec257763e6d5da99c3da4b44fa9a43fd25
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 5%

---

# Vídeos e tutoriais do AEM Sites {#overview}

{{edge-delivery-services}}

O Adobe Experience Manager (AEM) Sites é uma plataforma de gerenciamento de experiência líder. Este guia do usuário contém vídeos e tutoriais sobre os vários recursos e funcionalidades do AEM Sites.

## Três maneiras de criar com o AEM Sites

O AEM Sites fornece três maneiras de criar, criar e entregar experiências. Quer você esteja criando páginas inteiras, otimizando para desempenho de borda ou acionando aplicativos headless, o AEM Sites oferece opções flexíveis para atender às suas necessidades de projeto:

1. **Sites tradicionais** usam o Editor de páginas do AEM Author para criar conteúdo, que é ativado e entregue aos usuários finais pela Publicação do AEM como páginas da Web do HTML.
1. Os sites do **Edge Delivery Services** usam a criação baseada em documento ou o Adobe Universal Editor para criar conteúdo, que é ativado e entregue aos usuários finais pelo Edge Delivery Services como páginas da Web do HTML.
1. As experiências da Web **Headless/API-first** usam o Editor de fragmento de conteúdo ou o Editor universal para criar conteúdo, que é então ativado e entregue pela AEM Publish como JSON.

Essas opções são projetadas para atender às diversas necessidades das organizações de marketing, para fornecer experiências personalizadas e envolventes em alta velocidade e escala em qualquer canal ou dispositivo.

O diagrama a seguir ilustra os diferentes caminhos:

![AEM-Sites-Content-Authoring-and-Experience-Delivery-Paths.png](./assets/aem-sites-authoring-and-experience-delivery-paths.png){width="700" zoomable="yes"}

### Comparar maneiras de criar com o AEM Sites

A tabela a seguir fornece uma comparação de alto nível entre os três caminhos. Ele se concentra na criação de conteúdo e nas nuances de entrega de experiência de cada caminho.

|            | Sites tradicionais | Edge Delivery Services | Headless/API-First |
|---------------------|------------------------------|---------------------------------|---------------------------------------------|
| **Ferramentas de criação** | Editor de página | Criação baseada em documento, Editor universal | Fragmentos de conteúdo, Editor universal |
| **Repositório de Conteúdo Criado** | AEM Author (JCR) | Documentos ou autor do AEM (JCR) | AEM Author (JCR) |
| **Entrega** | Publicação do AEM (com Adobe CDN + Dispatcher) | Edge Delivery Services | Publicação do AEM (com Adobe CDN + Dispatcher) |
| **Armazenamento de Conteúdo de Entrega** | AEM Publish (JCR) | Edge Delivery Services | AEM Publish (JCR) |
| **Formato de Entrega** | HTML | HTML | JSON |
| **Tecnologia de desenvolvimento** | Java™, JavaScript, CSS | JavaScript, CSS | Qualquer item (por exemplo, Swift, React etc.) |

## Tutoriais

Saiba mais sobre cada um dos três caminhos a serem criados com o AEM Sites por meio dos seguintes tutoriais:

<!-- CARDS

* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview
  {title = Traditional Sites - WKND Tutorial}
  {description = Learn how to build a sample AEM Sites project using the WKND tutorial. This guide walks you through project setup, Core Components, Editable Templates, client-side libraries, and component development.}
  {image = ./assets/aem-wknd-spa-editor-tutorial.png}
  {target = _self}
* https://www.aem.live/docs/
  {title = Edge Delivery Services - Guides}
  {description = Explore Edge Delivery Services with comprehensive guides. The Build, Publish, and Launch guides cover everything you need to get started with EDS.}
  {image = ./assets/edge-delivery-services.png}
  {target = _blank}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/overview
  {title = Headless/API-First - Tutorials}
  {description = Learn how to build headless applications powered by AEM content. Tutorials cover frameworks like iOS, Android, and React—choose what fits your stack.}
  {image = ./assets/headless.png}
  {target = _self}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Traditional Sites - WKND Tutorial">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" title="Sites tradicionais - Tutorial do WKND" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/aem-wknd-spa-editor-tutorial.png" alt="Sites tradicionais - Tutorial do WKND"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" title="Sites tradicionais - Tutorial do WKND">Sites tradicionais - Tutorial WKND</a>
                    </p>
                    <p class="is-size-6">Saiba como criar um projeto de amostra do AEM Sites usando o tutorial WKND. Este guia aborda a configuração de projetos, os Componentes principais, os Modelos editáveis, as bibliotecas do lado do cliente e o desenvolvimento de componentes.</p>
                </div>
                <a href="https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Edge Delivery Services - Guides">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://www.aem.live/docs/" title="Edge Delivery Services - Guias" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/edge-delivery-services.png" alt="Edge Delivery Services - Guias"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" title="Edge Delivery Services - Guias">Edge Delivery Services - Guias</a>
                    </p>
                    <p class="is-size-6">Explore o Edge Delivery Services com guias abrangentes. Os guias Criar, Publicar e Iniciar abordam tudo o que é necessário para começar a usar o EDS.</p>
                </div>
                <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Headless/API-First - Tutorials">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/overview" title="Headless/API-First - tutoriais" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/headless.png" alt="Headless/API-First - tutoriais"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" title="Headless/API-First - tutoriais">Headless/API-First - tutoriais</a>
                    </p>
                    <p class="is-size-6">Saiba como criar aplicativos headless alimentados por conteúdo do AEM. Os tutoriais abrangem estruturas como iOS, Android™ e React — escolha o que se encaixa na sua pilha.</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Recursos adicionais

* [Documentação de criação do AEM Sites](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/essentials/first-steps)
* [Documentação de desenvolvimento do AEM Sites](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/introduction/getting-started)
* [Documentação de administração do AEM Sites](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/administering/home)
* [Documentação de implantação do AEM Sites](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/deploying/introduction/platform)
* [Tutoriais do AEM as a Cloud Service](/help/cloud-service/overview.md)
* [Tutoriais do AEM Assets](/help/assets/overview.md)
* [Tutoriais do AEM Forms](/help/forms/overview.md)
* [Tutoriais do AEM Foundation](/help/foundation/overview.md)
