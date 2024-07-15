---
title: Considerações sobre desenvolvimento
description: Considere o impacto no processo de desenvolvimento de front-end e back-end depois de habilitar o pipeline de front-end.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: a3b27d5b-b167-4c60-af49-8f2e8d814c86
duration: 79
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Considerações sobre desenvolvimento

Depois de permitir que o pipeline de front-end implante apenas os recursos de front-end no ambiente do AEM as a Cloud Service, há algum impacto no desenvolvimento local do AEM e é necessário ajustar o modelo de ramificação Git.

## Objetivo

* Como ter um fluxo de desenvolvimento front-end e back-end sem atritos
* Analisar as dependências entre o pipeline de pilha completa e de front-end


## Considerações sobre desenvolvimento local

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## Abordagem de desenvolvimento ajustada

* Para o desenvolvimento local usando o SDK do AEM, a equipe de desenvolvimento de back-end ainda precisa da geração de clientlib por meio do módulo `ui.frontend`, mas durante a implantação do Cloud Manager no ambiente do AEM as a Cloud Service, é necessário ignorá-la. Isso revela um desafio sobre como isolar as alterações de configuração do projeto descritas no capítulo [Atualizar projeto](update-project.md).

Uma __solução__ pode ser ajustar seu modelo de ramificação Git e garantir que as alterações de configuração do projeto AEM nunca retornem à ramificação __de desenvolvimento local__ que os desenvolvedores de back-end do AEM usam.


* Como parte de um aprimoramento contínuo do seu projeto AEM, se você introduzir novos componentes ou atualizar um componente existente que tenha alterações nos módulos `ui.app` e `ui.frontend`, será necessário executar pipelines de pilha completa e de front-end.
