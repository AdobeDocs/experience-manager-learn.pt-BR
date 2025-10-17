---
title: Habilitar pipeline de front-end para o arquétipo de projeto padrão do AEM
description: Saiba como habilitar um pipeline de front-end para um projeto padrão do AEM a fim de obter uma implantação mais rápida de recursos estáticos, como CSS, JavaScript, fontes e ícones. Além disso, temos a separação do desenvolvimento de front-end do desenvolvimento de back-end de pilha completa no AEM.
version: Experience Manager as a Cloud Service
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
source-git-commit: dbf63f30ccfd06e4f4d7883c2f7bc4ac78245364
workflow-type: ht
source-wordcount: '428'
ht-degree: 100%

---

# Habilitar pipeline de front-end para o arquétipo de projeto padrão do AEM{#enable-front-end-pipeline-standard-aem-project}

{{traditional-aem}}

Saiba como habilitar o [Projeto de site da WKND no AEM](https://github.com/adobe/aem-guides-wknd) (também conhecido como projeto padrão do AEM) criado com o [Arquétipo de projeto do AEM](https://github.com/adobe/aem-project-archetype) para implantar recursos de front-end, como CSS, JavaScript, fontes e ícones, usando um pipeline de front-end para obter um ciclo mais rápido do desenvolvimento à implantação. A separação do desenvolvimento de front-end do desenvolvimento de back-end de pilha completa no AEM. Você também aprenderá como esses recursos de front-end __não__ são oferecidos pelo repositório do AEM, mas pela CDN, uma alteração no paradigma de entrega.


Um novo pipeline de front-end é criado no Adobe Cloud Manager para criar e implantar apenas `ui.frontend` artefatos na CDN integrada, e informar o AEM sobre sua localização. No AEM, durante a geração do HTML da página da web, as tags `<link>` e `<script>` referem-se a essa localização do artefato no valor do atributo `href`.

No entanto, após a conversão do projeto do site da WKND no AEM, os desenvolvedores de front-end podem trabalhar separadamente e paralelamente a qualquer desenvolvimento de back-end de pilha completa no AEM, que tem seus próprios pipelines de implantação.

>[!IMPORTANT]
>
>De modo geral, o pipeline de front-end é normalmente usado com a [Criação rápida de sites do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=pt-br). Há um tutorial relacionado, [Introdução ao AEM Sites: criação rápida de sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=pt-BR), para saber mais sobre isso. Neste tutorial e nos vídeos associados, você verá referências a ele, a fim de garantir que diferenças sutis sejam destacadas e haja uma comparação direta ou indireta para explicar conceitos cruciais.


Um [tutorial em várias etapas](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=pt-BR) relacionado aborda a implementação de um site do AEM para uma marca fictícia de estilo de vida, a WKND, usando o recurso de criação rápida de sites. Também é útil revisar o [Fluxo de trabalho de temas](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=pt-BR) para entender o funcionamento do pipeline de front-end.

## Visão geral, benefícios e considerações sobre o pipeline de front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>Aplica-se somente ao AEM as a Cloud Service, não a implantações do Adobe Cloud Manager baseadas no AMS.

## Pré-requisitos

A etapa de implantação deste tutorial ocorre em um Adobe Cloud Manager. Verifique se você tem uma função de __Gerente de implantação__. Consulte [Definições de função](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=pt-BR#role-definitions) do Cloud Manager.

Use o [Programa de sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=pt-BR) e o [Ambiente de desenvolvimento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=pt-BR) para realizar este tutorial.

## Próximas etapas {#next-steps}

Um tutorial passo a passo percorre a conversão do [Projeto do site da WKND no AEM](https://github.com/adobe/aem-guides-wknd) para habilitá-lo para o pipeline de front-end.

O que você está esperando? Para iniciar o tutorial, navegue até o capítulo [Revisar projeto de pilha completa](review-uifrontend-module.md) e recapitule o ciclo de vida de desenvolvimento de front-end no contexto do projeto padrão do AEM Sites.
