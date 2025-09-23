---
title: Vídeos e tutoriais do AEM Sites
description: Navegue por vídeos e tutoriais sobre os recursos e as funcionalidades do Adobe Experience Manager Sites. O AEM Sites é uma plataforma líder em gerenciamento de experiência.
solution: Experience Manager, Experience Manager Sites
sub-product: Experience Manager Sites
topic: Content Management
doc-type: Catalog
exl-id: cde4ce7f-0afe-4632-8c1c-354586f296d5
source-git-commit: 2b3ff1957f9da313b71a73492777700ddbd79854
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 38%

---

# Vídeos e tutoriais do AEM Sites {#overview}

{{edge-delivery-services}}

O Adobe Experience Manager (AEM) Sites é a plataforma de gerenciamento de experiências da Adobe que permite a criação, o gerenciamento e a entrega de experiências digitais, seja por meio de um site, de um aplicativo móvel ou de qualquer outro canal digital.

## Três maneiras de fornecer experiências com o AEM Sites

O AEM Sites fornece três maneiras de criar e entregar experiências. Quer você esteja criando sites, otimizando para desempenho de borda ou acionando aplicativos headless, o AEM Sites oferece opções flexíveis para atender às suas necessidades de projeto:

1. As experiências do **Edge Delivery Services** usam o Edge Network da Adobe para fornecer conteúdo com alta velocidade e baixa latência. O serviço otimiza automaticamente o conteúdo para o dispositivo de consumo, os mecanismos de pesquisa e os agentes GenAI. Os autores criam conteúdo usando o Adobe Universal Editor ou a criação baseada em documento.
1. As experiências **Headless/API-first** usam o AEM Publish para fornecer conteúdo como JSON sobre APIs HTTP para aplicativos móveis, aplicativos de página única (SPAs) ou outros clientes headless. Os autores criam conteúdo usando o Editor de fragmento de conteúdo ou o Editor universal.
1. As experiências **AEM** tradicionais usam o AEM Publish para fornecer conteúdo como páginas da Web do HTML. Os autores criam conteúdo usando o Editor de páginas do AEM Author. Essa opção é mais adequada para projetos existentes ou já migrados.

Todas as três opções são abordagens fortes e a melhor opção depende do seu caso de uso e das necessidades organizacionais. Cada abordagem permite que as equipes forneçam experiências personalizadas e envolventes em velocidade e escala em qualquer canal ou dispositivo.

>[!IMPORTANT]
>
> O **Edge Delivery Services** é a maneira mais recente e avançada de fornecer sites com o AEM. Ele combina a velocidade e a escalabilidade do Edge Network do Adobe com opções de criação modernas. Embora a Edge Delivery Services seja recomendada para novos projetos, a AEM Sites continua a oferecer suporte a abordagens headless e tradicionais, para que você possa escolher o caminho que melhor se adapta às suas necessidades.

O diagrama a seguir descreve as diferentes opções para criar experiências com o AEM Sites:

![AEM-Sites-Content-Authoring-and-Experience-Delivery-Paths.png](./assets/aem-sites-authoring-and-experience-delivery-paths.png){width="700" zoomable="yes"}

### Comparar maneiras de criar com o AEM Sites

A tabela a seguir fornece uma comparação de alto nível entre os três caminhos. Ela se concentra na criação de conteúdo e nas nuances de entrega de experiência de cada caminho.

|            | Edge Delivery Services | Headless/API-First | AEM tradicional |
|---------------------|------------------------------|---------------------------------|---------------------------------------------|
| **Recomendado para** | Sites com altas necessidades de tráfego, desempenho e escalabilidade | Aplicativos móveis, SPAs e outros aplicativos headless | Projetos existentes ou migrados |
| **Ferramentas de criação** | Criação baseada em documento, Editor universal, Editor de páginas | Fragmentos de conteúdo, editor universal | Editor de páginas, Editor universal |
| **Repositório de conteúdo criado** | Documentos ou AEM Author (JCR) | AEM Author (JCR) | AEM Author (JCR) |
| **Entrega** | Edge Delivery Services | AEM Publish (com Adobe CDN + Dispatcher) | AEM Publish (com Adobe CDN + Dispatcher) |
| **Repositório de conteúdo de entrega** | Edge Delivery Services | AEM Publish (JCR) | AEM Publish (JCR) |
| **Formato de entrega** | HTML | JSON | HTML |
| **Tecnologia de desenvolvimento** | JavaScript, CSS | Qualquer (por exemplo, Swift, React etc.) | Java™, HTL, JavaScript, CSS |
| **Pesquisar suporte de bot e agente GenAI** | Otimizado para bots, mecanismos de pesquisa e agentes GenAI | Funciona para bots e agentes, mas pode exigir SSR ou configuração adicional | Adequado para bots, mas o desempenho pode ser mais lento em comparação ao Edge Delivery Services |

## Migração do AMS ou no local

Se você estiver migrando do AMS ou no local (OTP) para o AEM as a Cloud Service, a Adobe incentiva avaliar a mudança diretamente para o Edge Delivery Services. Normalmente, o esforço não é maior do que migrar para o AEM as a Cloud Service Publish e, ao mesmo tempo, fornecer desempenho mais rápido e maior escalabilidade. Se você decidir que o Edge Delivery Services não é a escolha certa para você no momento ou se as outras abordagens atenderem melhor às suas necessidades, elas permanecerão totalmente compatíveis e válidas para o seu projeto.

## Tutoriais

Explore as três abordagens para criar com o AEM Sites em mais detalhes. Os tutoriais abaixo orientam você sobre como cada opção funciona, as ferramentas envolvidas e quando usá-las.

<!-- CARDS

* https://www.aem.live/docs/
  {title = Edge Delivery Services - Guides}
  {description = Explore Edge Delivery Services with comprehensive guides. The Build, Publish, and Launch guides cover everything you need to get started with Edge Delivery Services.}
  {image = ./assets/edge-delivery-services.png}
  {target = _blank}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/overview
  {title = Headless/API-First - Tutorials}
  {description = Learn how to build headless applications powered by AEM content. Tutorials cover frameworks like iOS, Android, and React—choose what fits your stack.}
  {image = ./assets/headless.png}
  {target = _self}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview
  {title = Traditional AEM - WKND Tutorial}
  {description = Learn how to build a sample AEM Sites project using the WKND tutorial. This guide walks you through project setup, Core Components, Editable Templates, client-side libraries, and component development.}
  {image = ./assets/aem-wknd-spa-editor-tutorial.png}
  {target = _self}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Edge Delivery Services - Guides">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://www.aem.live/docs/" title="Edge Delivery Services — Guias" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/edge-delivery-services.png" alt="Edge Delivery Services — Guias"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" title="Edge Delivery Services — Guias">Edge Delivery Services — Guias</a>
                    </p>
                    <p class="is-size-6">Explore o Edge Delivery Services com guias abrangentes. Os guias Criar, Publicar e Launch abordam tudo o que você precisa para começar a usar o Edge Delivery Services.</p>
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
                    <a href="https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/getting-started-with-aem-headless/overview" title="Headless/API-First — Tutoriais" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/headless.png" alt="Headless/API-First — Tutoriais"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" title="Headless/API-First — Tutoriais">Headless/API-First — Tutoriais</a>
                    </p>
                    <p class="is-size-6">Saiba como criar aplicativos headless alimentados por conteúdo do AEM. Os tutoriais abrangem estruturas como iOS, Android e React: escolha o que corresponde à sua pilha.</p>
                </div>
                <a href="https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Traditional AEM - WKND Tutorial">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" title="AEM tradicional: tutorial da WKND" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/aem-wknd-spa-editor-tutorial.png" alt="AEM tradicional: tutorial da WKND"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" title="AEM tradicional: tutorial da WKND">AEM tradicional: tutorial da WKND</a>
                    </p>
                    <p class="is-size-6">Saiba como criar um projeto de amostra do AEM Sites usando o tutorial WKND. Este guia aborda a configuração de projetos, os Componentes principais, os Modelos editáveis, as bibliotecas do lado do cliente e o desenvolvimento de componentes.</p>
                </div>
                <a href="https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Saiba mais</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Recursos adicionais

* [Documentação de criação do AEM Sites](https://experienceleague.adobe.com/pt-br/docs/experience-manager-65/content/sites/authoring/essentials/first-steps)
* [Documentação de desenvolvimento do AEM Sites](https://experienceleague.adobe.com/pt-br/docs/experience-manager-65/content/implementing/developing/introduction/getting-started)
* [Documentação de administração do AEM Sites](https://experienceleague.adobe.com/pt-br/docs/experience-manager-65/content/sites/administering/home)
* [Documentação de implantação do AEM Sites](https://experienceleague.adobe.com/pt-br/docs/experience-manager-65/content/implementing/deploying/introduction/platform)
* [Tutoriais do AEM as a Cloud Service](/help/cloud-service/overview.md)
* [Tutoriais do AEM Assets](/help/assets/overview.md)
* [Tutoriais do AEM Forms](/help/forms/overview.md)
* [Tutoriais do AEM Foundation](/help/foundation/overview.md)
