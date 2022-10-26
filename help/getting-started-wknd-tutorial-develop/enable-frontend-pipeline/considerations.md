---
title: Considerações de desenvolvimento
description: Considere o impacto no processo de desenvolvimento front-end e back-end após habilitar o pipeline front-end.
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
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---


# Considerações de desenvolvimento

Depois de habilitar o pipeline front-end para implantar apenas os recursos front-end AEM ambiente as a Cloud Service, há algum impacto no desenvolvimento de AEM local e você precisa ajustar o modelo de ramificação git.

## Objetivo

* Como ter um fluxo de desenvolvimento front-end e back-end sem atrito
* Revise as dependências entre a pilha completa e o pipeline front-end


## Considerações sobre o desenvolvimento local

>[!VIDEO](https://video.tv.adobe.com/v/3409421/)


## Abordagem de desenvolvimento adaptada

* Para o desenvolvimento local usando AEM SDK, a equipe de desenvolvimento de back-end ainda precisa de geração de clientlib por meio de `ui.frontend` mas durante a implantação do Cloud Manager para AEM ambiente as a Cloud Service é necessário ignorá-lo. Isso mostra um desafio sobre como isolar as alterações de configuração do projeto descritas na [Atualizar projeto](update-project.md) capítulo.

A __solução__ pode ser ajustar o modelo de ramificação git e garantir que as alterações na configuração do projeto AEM nunca retornem ao __desenvolvimento local__ ramificar o AEM que os desenvolvedores de back-end usam.


* Como parte de um aprimoramento contínuo do seu projeto de AEM, se você apresentar novos componentes ou atualizar um componente existente que tenha alterações em ambos `ui.app` e `ui.frontend` módulo, você precisa executar pipelines full-stack e front-end.



