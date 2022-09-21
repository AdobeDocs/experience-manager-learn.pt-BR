---
title: Habilitar pipeline front-end para Arquétipo de projeto de AEM padrão
description: Saiba como habilitar um pipeline front-end para projetos de AEM padrão para implantação mais rápida de recursos estáticos, como CSS, JavaScript, Fontes, Ícones. Também separação do desenvolvimento front-end do desenvolvimento back-end completo de pilha em AEM.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 2e3615e9e9305165ca9c3c93b38ac7e9bdcc51fb
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 3%

---


# Habilitar pipeline front-end para Arquétipo de projeto de AEM padrão{#enable-front-end-pipeline-standard-aem-project}

Saiba como habilitar o [AEM Projeto de Sites WKND](https://github.com/adobe/aem-guides-wknd) (também conhecido como Projeto de AEM Padrão) criado usando [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype) para implantar recursos front-end como CSS, JavaScript, Fontes e Icons usando um pipeline front-end para um ciclo de desenvolvimento para implantação mais rápido. Separação do desenvolvimento front-end do desenvolvimento back-end completo em AEM. Você também aprenderá como esses recursos de front-end são __not__ Servido do repositório de AEM, mas da CDN, uma mudança no paradigma de delivery.


Um novo pipeline front-end é criado no Adobe Cloud Manager que só cria e implanta `ui.frontend` artefatos do CDN integrado e informa AEM sobre sua localização. No AEM durante a geração de HTML da página da Web, a variável `<link>` e `<script>` tags , consulte este local de artefato no `href` valor do atributo.

No entanto, após a conversão do projeto AEM Sites da WKND, os desenvolvedores de front-end podem trabalhar separadamente e em paralelo a qualquer desenvolvimento de back-end de pilha completo no AEM, que tem seus próprios pipelines de implantação.

>[!IMPORTANT]
>
>Em geral, o pipeline de front-end geralmente é usado com o [Criação rápida de AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en), há um tutorial relacionado [Introdução ao AEM Sites - Criação rápida do site](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) para saber mais sobre isso. Assim, neste tutorial e vídeos associados você encontra referências a ele, isso garante que diferenças sutis sejam chamadas e que haja alguma comparação direta ou indireta para explicar conceitos cruciais.


Um relacionado [tutorial em várias etapas](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) O orienta a implementação de um site de AEM para uma marca fictícia de estilo de vida na WKND usando o recurso Criação rápida de site . Revisão da [Fluxo de trabalho de temas](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) para entender o funcionamento do pipeline front-end também é útil.

## Visão geral, benefícios e considerações para pipeline front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409343/)


>[!NOTE]
>
>Isso se aplica somente a AEM as a Cloud Service e não a implantações do Adobe Cloud Manager baseadas em AMS.

## Pré-requisitos

A etapa de implantação deste tutorial ocorre em um Adobe Cloud Manager, e você tem uma __Gerenciador de implantação__ consulte Cloud Manage [Definições de função](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

Certifique-se de usar o [Programa Sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) e [Ambiente de desenvolvimento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) ao concluir este tutorial.

## Próximas etapas {#next-steps}

Um tutorial passo a passo percorre o [AEM Projeto de Sites WKND](https://github.com/adobe/aem-guides-wknd) conversão para habilitá-la para o pipeline de front-end.

O que você está esperando? Inicie o tutorial navegando até o [Revisar projeto de pilha completa](review-uifrontend-module.md) e recapitular o ciclo de vida de desenvolvimento front-end no contexto do AEM Sites Project padrão.

