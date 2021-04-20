---
title: Uso de ofertas de fragmento de experiência do AEM no Adobe Target
seo-title: Uso de ofertas de fragmento de experiência do AEM no Adobe Target
description: O Adobe Experience Manager 6.4 recria o fluxo de trabalho de personalização entre o AEM e o Target. As experiências criadas no AEM agora podem ser entregues diretamente para o Adobe Target como ofertas HTML. Ele permite que os profissionais de marketing testem e personalizem com facilidade o conteúdo em diferentes canais.
seo-description: O Adobe Experience Manager 6.4 recria o fluxo de trabalho de personalização entre o AEM e o Target. As experiências criadas no AEM agora podem ser entregues diretamente para o Adobe Target como ofertas HTML. Ele permite que os profissionais de marketing testem e personalizem com facilidade o conteúdo em diferentes canais.
sub-product: serviços de conteúdo
feature: Experience Fragments
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
topic: Personalization
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 11%

---


# Uso de ofertas de fragmento de experiência no Adobe Target{#using-experience-fragment-offers-within-adobe-target}

O Adobe Experience Manager 6.4 recria o fluxo de trabalho de personalização entre o AEM e o Target. As experiências criadas no AEM agora podem ser entregues diretamente para o Adobe Target como ofertas HTML. Ele permite que os profissionais de marketing testem e personalizem com facilidade o conteúdo em diferentes canais.

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>Recomendado para usar a biblioteca de clientes da at.js e a prática recomendada é usar soluções de gerenciamento de tags como o Launch da Adobe, o Adobe DTM ou qualquer solução de gerenciamento de tags de terceiros para adicionar bibliotecas de direcionamento às páginas do site

>[!NOTE]
>
>As Ofertas de fragmento de experiência do AEM no Adobe Target também estão disponíveis como um Feature Pack para usuários do AEM 6.3. Consulte a seção abaixo para pacotes de recursos e dependências.


* O mecanismo de criação de conteúdo fácil de usar e eficiente do Adobe Experience Manager, juntamente com a Inteligência artificial (AI) e o Aprendizado de máquina do Adobe Target, ajudam os autores de conteúdo a criar e gerenciar o conteúdo para todos os canais em um local centralizado. Com a capacidade de exportar Fragmentos de experiência para o Adobe Target como ofertas HTML, os profissionais de marketing agora têm mais flexibilidade para criar uma experiência mais personalizada usando essas ofertas e agora podem testar e dimensionar cada experiência criada.
* A principal diferença entre as ofertas HTML e as ofertas de Fragmento de experiência é que a edição para mais tarde só pode ser feita no AEM e, em seguida, sincronizada com o Adobe Target
* A configuração do serviço da Target Cloud aplicada à pasta Fragmento de experiência herda todos os Fragmentos de experiência criados diretamente na pasta principal. A pasta secundária não herda a configuração dos serviços de nuvem principais.
* Para criar uma oferta personalizada, agora podemos aproveitar facilmente o conteúdo armazenado no AEM.
* Você pode criar tipos de atividades do Target, incluindo as atividades alimentadas pelo Sensei, como Alocação automática, Direcionamento automático e Personalização automatizada

## Pacotes de recursos e dependências do AEM 6.3 {#aem-feature-packs-and-dependencies}

| Feature Pack do AEM 6.3 | Dependências |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumulativefixpack:aem-6.3.2-cfp:2.0 |

## Recursos adicionais {#additional-resources}

* [Documentação dos fragmentos de experiência](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Uso de Fragmentos de experiência](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
