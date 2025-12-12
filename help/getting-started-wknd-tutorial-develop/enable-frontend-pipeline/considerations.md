---
title: Considerações sobre desenvolvimento
description: Considere o impacto no processo de desenvolvimento de front-end e back-end depois de habilitar o pipeline de front-end.
version: Experience Manager as a Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: a3b27d5b-b167-4c60-af49-8f2e8d814c86
duration: 79
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 100%

---

# Considerações sobre desenvolvimento

Depois de permitir que o pipeline de front-end implante apenas os recursos de front-end no ambiente do AEM as a Cloud Service, há algum impacto no desenvolvimento local do AEM, sendo necessário ajustar o modelo de ramificação do Git.

## Objetivo

* Como ter um fluxo de desenvolvimento de front-end e back-end sem atritos
* Analisar as dependências entre o pipeline de pilha completa e de front-end


## Considerações sobre desenvolvimento local

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## Abordagem de desenvolvimento ajustada

* Para o desenvolvimento local com o SDK do AEM, a equipe de desenvolvimento de back-end ainda precisa da geração de clientlibs por meio do módulo `ui.frontend`, mas, durante a implantação do Cloud Manager no ambiente do AEM as a Cloud Service, é necessário ignorá-la. Isso revela um desafio referente a como isolar as alterações de configuração do projeto descritas no capítulo [Atualizar projeto](update-project.md).

Uma __solução__ pode ser ajustar o seu modelo de ramificação do Git e garantir que as alterações nas configurações do projeto do AEM nunca retornem à ramificação __de desenvolvimento local__ que os desenvolvedores de back-end do AEM usam.


* Como parte de um aprimoramento contínuo do seu projeto do AEM, se você introduzir novos componentes ou atualizar um componente existente que tenha alterações nos módulos `ui.app` e `ui.frontend`, será necessário executar pipelines de pilha completa e de front-end.
