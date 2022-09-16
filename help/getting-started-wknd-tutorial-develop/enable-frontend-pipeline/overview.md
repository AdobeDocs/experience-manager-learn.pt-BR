---
title: Habilitar pipeline front-end para o arquétipo de projeto AEM padrão
description: Converta um projeto de AEM padrão para implantação mais rápida de recursos estáticos, como CSS, JavaScript, Fontes, Icones usando o pipeline de Front-End. E a separação do desenvolvimento do Front-End do desenvolvimento de back-end de pilha completa em AEM.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 96e1c95b7cd672aa5d4f79707735abc86dae7b8a
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Habilitar pipeline front-end para o arquétipo de projeto AEM padrão{#enable-front-end-pipeline-standard-aem-project}

Saiba como habilitar o projeto de AEM padrão criado usando [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype) para implantar recursos estáticos, como CSS, JavaScript, Fontes, Ícones usando o pipeline de front-end para um ciclo de desenvolvimento para implantação mais rápido. E a separação do desenvolvimento do Front-End do desenvolvimento de back-end de pilha completa em AEM. Você também aprenderá como esses recursos estáticos são __not__ Servido de AEM repositório, mas da CDN, uma mudança no paradigma de delivery.

Um novo pipeline front-end é criado no Adobe Cloud Manager que só cria e implanta `ui.frontend` artefatos para CDN e informa AEM sobre sua localização. Durante a geração do HTML da página da Web, a variável `<link>` e `<script>` as tags se referem a esse local na `href` valor do atributo.

Após a conversão padrão do projeto de AEM, os desenvolvedores front-end podem trabalhar separadamente e em paralelo a qualquer desenvolvimento back-end de pilha completo no AEM, que tem seus próprios pipelines de implantação.

>[!VIDEO](https://video.tv.adobe.com/v/3409268)

>[!NOTE]
>
>Isso só é aplicável a AEM as a Cloud Service e não a implantações do Adobe Cloud Manager baseadas em AMS.

