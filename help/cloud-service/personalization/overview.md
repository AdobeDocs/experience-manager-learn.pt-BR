---
title: Visão geral da personalização
description: Saiba como personalizar sites do AEM as a Cloud Service usando aplicativos Adobe Target e Adobe Experience Platform.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18717
thumbnail: null
source-git-commit: 70665c019f63df1e736292ad24c47624a3a80d49
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 3%

---

# Visão geral da personalização

Saiba como o AEM as a Cloud Service (AEMCS) se integra ao Adobe Target e ao Adobe Experience Platform (AEP). Descubra como fornecer experiências personalizadas usando testes A/B, direcionando usuários com base em seu comportamento ou personalizando conteúdo usando perfis de clientes.

## Pré-requisitos

Para demonstrar vários cenários de personalização, este tutorial usa a amostra de projeto [WKND](https://github.com/adobe/aem-guides-wknd/) do AEM. Em seguida, é necessário:

- Uma organização da Adobe com acesso a:
   - **Ambiente do AEM as a Cloud Service** - para criar e gerenciar conteúdo
   - **Adobe Target** - para compor e entregar experiências personalizadas
   - **aplicativos do Adobe Experience Platform** - para gerenciar perfis e públicos-alvo de clientes
   - **Marcas (antigo Launch) na AEP** - para implantar a Web SDK e o JavaScript personalizado para coleta e personalização de dados

- Uma compreensão básica dos componentes do AEM e Fragmentos de experiência

- O projeto [WKND](https://github.com/adobe/aem-guides-wknd/) do AEM foi implantado em seu ambiente AEM as a Cloud Service.

## Introdução

Antes de explorar casos de uso específicos, primeiro configure o AEM as a Cloud Service para personalização. Comece integrando o Adobe Target e as tags para permitir a personalização no lado do cliente usando o AEP Web SDK. Essas etapas fundamentais permitem que suas páginas do AEM sejam compatíveis com experimentação, direcionamento de público-alvo e personalização em tempo real.

<!-- CARDS
{target = _self}

* ./setup/integrate-adobe-target.md
  {title = Integrate Adobe Target}
  {description = Integrate AEMCS with Adobe Target to activate personalized content as Adobe Target offers.}
  {image = ./assets/setup/integrate-target.png}
  {cta = Integrate Target}

* ./setup/integrate-adobe-tags.md
  {title = Integrate Tags}
  {description = Integrate AEMCS with Tags to inject the Web SDK and custom JavaScript for data collection and personalization.}
  {image = ./assets/setup/integrate-tags.png}
  {cta = Integrate Tags}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Adobe Target">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-target.md" title="Integrar o Adobe Target" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-target.png" alt="Integrar o Adobe Target"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" title="Integrar o Adobe Target">Integrar o Adobe Target</a>
                    </p>
                    <p class="is-size-6">Integre o AEM CS com o Adobe Target para ativar o conteúdo personalizado como ofertas do Adobe Target.</p>
                </div>
                <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Integrar o Target</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-tags.md" title="Tags de integração" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-tags.png" alt="Tags de integração"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" title="Tags de integração">Integrar marcas</a>
                    </p>
                    <p class="is-size-6">Integre o AEMCS com tags para inserir o Web SDK e o JavaScript personalizado para coleta e personalização de dados.</p>
                </div>
                <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Integrar Marcas</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



## Casos de uso

Explore os seguintes casos de uso comuns de personalização compatíveis com o AEM, o Adobe Target e o Adobe Experience Platform.

<!-- CARDS
{target = _self}

* ./use-cases/experimentation.md
  {title = Experimentation (A/B Testing)}
  {description = Learn how to test different content variations in AEMCS using Adobe Target for A/B testing.}
  {image = ./assets/use-cases/experiment/experimentation.png}
  {cta = Learn Experimentation}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Experimentation (A/B Testing)">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/experimentation.md" title="Experimentação (teste A/B)" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/experiment/experimentation.png" alt="Experimentação (teste A/B)"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/experimentation.md" target="_self" rel="referrer" title="Experimentação (teste A/B)">Experimentação (Teste A/B)</a>
                    </p>
                    <p class="is-size-6">Saiba como testar diferentes variações de conteúdo no AEM CS usando o Adobe Target para testes A/B.</p>
                </div>
                <a href="./use-cases/experimentation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aprender experimentação</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



















