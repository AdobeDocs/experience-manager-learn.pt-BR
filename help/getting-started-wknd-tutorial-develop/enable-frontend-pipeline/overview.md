---
title: Habilitar pipeline front-end para o arquétipo padrão de projeto AEM
description: Saiba como habilitar um pipeline de front-end para projeto AEM padrão para uma implantação mais rápida de recursos estáticos, como CSS, JavaScript, Fontes, Ícones. Separação do desenvolvimento de front-end do desenvolvimento de back-end de pilha completa no AEM.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
doc-type: Tutorial
exl-id: b795e7e8-f611-4fc3-9846-1d3f1a28ccbc
duration: 206
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---

# Habilitar pipeline front-end para o arquétipo padrão de projeto AEM{#enable-front-end-pipeline-standard-aem-project}

Saiba como habilitar o [Projeto de Sites WKND do AEM](https://github.com/adobe/aem-guides-wknd) (também conhecido como Projeto AEM Padrão) criado com o [Arquétipo de Projeto do AEM](https://github.com/adobe/aem-project-archetype) para implantar recursos de front-end, como CSS, JavaScript, Fontes e Ícones, usando um pipeline de front-end para obter um ciclo mais rápido do desenvolvimento até a implantação. A separação do desenvolvimento front-end do desenvolvimento back-end de pilha completa no AEM. Você também aprenderá como esses recursos de front-end são __não__ oferecidos pelo repositório AEM, mas pela CDN, uma mudança no paradigma de entrega.


Um novo pipeline de front-end é criado no Adobe Cloud Manager que cria e implanta apenas `ui.frontend` artefatos no CDN incorporado e informa o AEM sobre sua localização. No AEM, durante a geração de HTML da página da Web, as tags `<link>` e `<script>` se referem a esse local do artefato no valor do atributo `href`.

No entanto, após a conversão do projeto AEM do WKND Sites, os desenvolvedores de front-end podem trabalhar separadamente e em paralelo a qualquer desenvolvimento de back-end de pilha completa no AEM, que tem seus próprios pipelines de implantação.

>[!IMPORTANT]
>
>De modo geral, o pipeline de front-end geralmente é usado com a [Criação rápida de site de AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en). Há um tutorial relacionado [Introdução ao AEM Sites - Criação rápida de site](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) para saber mais sobre isso. Neste tutorial e vídeos associados você encontra referências a ele, isso é para garantir que diferenças sutis sejam destacadas e haja alguma comparação direta ou indireta para explicar conceitos cruciais.


Um [tutorial em várias etapas](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) relacionado aborda a implementação de um site AEM para uma marca fictícia de estilo de vida, a WKND, usando o recurso de Criação rápida de sites. Também é útil revisar o [Fluxo de trabalho de tema](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) para entender o funcionamento do pipeline de front-end.

## Visão geral, benefícios e considerações para pipeline de front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>Isso se aplica somente à AEM as a Cloud Service e não a implantações de Adobe Cloud Manager baseadas em AMS.

## Pré-requisitos

A etapa de implantação deste tutorial ocorre em um Adobe Cloud Manager. Verifique se você tem uma função de __Gerente de implantação__. Consulte as [Definições de função](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions) do Cloud Manager.

Use o [Programa de sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) e o [Ambiente de desenvolvimento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) ao concluir este tutorial.

## Próximas etapas {#next-steps}

Um tutorial passo a passo percorre a conversão do [Projeto de sites WKND do AEM](https://github.com/adobe/aem-guides-wknd) para habilitá-lo para o pipeline de front-end.

O que você está esperando? Inicie o tutorial navegando até o capítulo [Revisar projeto de pilha completa](review-uifrontend-module.md) e recapitule o ciclo de vida de desenvolvimento front-end no contexto do projeto padrão do AEM Sites.
