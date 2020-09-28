---
title: Uso AEM Ofertas de fragmento de experiência no Adobe Target
seo-title: Uso AEM Ofertas de fragmento de experiência no Adobe Target
description: O Adobe Experience Manager 6.4 reinventa o fluxo de trabalho de personalização entre AEM e Público alvo. As experiências criadas no AEM agora podem ser entregues diretamente para a Adobe Target como Ofertas HTML. Isso permite que os profissionais de marketing testem e personalizem o conteúdo de forma contínua em diferentes canais.
seo-description: O Adobe Experience Manager 6.4 reinventa o fluxo de trabalho de personalização entre AEM e Público alvo. As experiências criadas no AEM agora podem ser entregues diretamente para a Adobe Target como Ofertas HTML. Isso permite que os profissionais de marketing testem e personalizem o conteúdo de forma contínua em diferentes canais.
sub-product: serviços de conteúdo
feature: experience-fragments
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 10%

---


# Uso de Ofertas de fragmento de experiência no Adobe Target{#using-experience-fragment-offers-within-adobe-target}

O Adobe Experience Manager 6.4 reinventa o fluxo de trabalho de personalização entre AEM e Público alvo. As experiências criadas no AEM agora podem ser entregues diretamente para a Adobe Target como Ofertas HTML. Isso permite que os profissionais de marketing testem e personalizem o conteúdo de forma contínua em diferentes canais.

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>Recomendado para usar a biblioteca do cliente at.js e a prática recomendada é usar soluções de gerenciamento de tags como Launch by Adobe, Adobe DTM ou qualquer solução de gerenciamento de tags de terceiros para adicionar bibliotecas de públicos alvos às páginas do site

>[!NOTE]
>
>AEM Ofertas de fragmento de experiência no Adobe Target também estão disponíveis como um Pacote de recursos para AEM usuários 6.3. Consulte a seção abaixo para ver os Pacotes de recursos e as dependências.


* A Adobe Experience Manager, que  o mecanismo de criação de conteúdo fácil de usar e poderoso junto com a Inteligência Artificial (AI) da Adobe Target e o Aprendizado de Máquina, ajuda os autores de conteúdo a criar e gerenciar conteúdo para todos os canais em um local centralizado. Com a capacidade de exportar Fragmentos de experiência para o Adobe Target como ofertas HTML, os profissionais de marketing agora têm mais flexibilidade para criar uma experiência mais personalizada usando essas ofertas e agora podem testar e dimensionar cada experiência criada.
* A principal diferença entre as ofertas HTML e as ofertas de fragmento de experiência é que a edição para mais tarde só pode ser feita em AEM e sincronizada com a Adobe Target
* A configuração do serviço da Público alvo Cloud aplicada à pasta Experience Fragment herda todos os Fragmentos de experiência criados diretamente na pasta pai. A pasta filho não herda a configuração dos serviços de nuvem pai.
* Para criar uma oferta personalizada, agora é possível aproveitar facilmente o conteúdo armazenado em AEM.
* Você pode criar tipos de atividades de Públicos alvos, incluindo atividades com capacidade para Sensei, como Autoalocação, Público alvo automático e Automated Personalization

## AEM 6.3 Pacotes de recursos e dependências {#aem-feature-packs-and-dependencies}

| Pacote de recursos AEM 6.3 | Dependências |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:2.0 |

## Recursos adicionais {#additional-resources}

* [Personalização fluida - AEM fragmentos de experiência no Adobe Target](https://www.youtube.com/watch?v=ohvKDjCb1yM)
* [Documentação dos fragmentos de experiência](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Uso de Fragmentos de experiência](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
