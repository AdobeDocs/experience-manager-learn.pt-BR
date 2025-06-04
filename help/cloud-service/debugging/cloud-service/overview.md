---
title: Depurar o AEM as a Cloud Service
description: em infraestruturas em nuvem dimensionáveis e de autoatendimento exige que os(as) desenvolvedores(as) do AEM compreendam e saibam como depurar vários aspectos do AEM as a Cloud Service, desde a criação e implantação até a obtenção de detalhes sobre a execução de aplicativos do AEM.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
duration: 60
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '314'
ht-degree: 100%

---

# Depurar o AEM as a Cloud Service

O AEM as a Cloud Service permite utilizar os aplicativos do AEM diretamente da nuvem. O AEM as a Cloud Service é executado em uma infraestrutura em nuvem dimensionável e de autoatendimento que exige que os desenvolvedores do AEM saibam como entender e depurar vários aspectos do AEM as a Cloud Service, desde a criação e implantação até a obtenção de detalhes sobre a execução de aplicativos do AEM.

## Logs

Os logs fornecem detalhes sobre como o aplicativo está funcionando no AEM as a Cloud Service, bem como insights sobre problemas com implantações.

[Depurar o AEM as a Cloud Service com logs](./logs.md)

## Build e implantação

Os pipelines do Adobe Cloud Manager implantam o aplicativo do AEM por meio de uma série de etapas, a fim de determinar a qualidade e a viabilidade do código quando implantado no AEM as a Cloud Service. Cada uma das etapas pode resultar em falhas, o que torna importante entender como depurar compilações para determinar a causa das falhas e como resolvê-las.

[Depurar a compilação e a implantação do AEM as a Cloud Service](./build-and-deployment.md)

## Console do desenvolvedor

O console do desenvolvedor fornece diversas informações e insights sobre ambientes do AEM as a Cloud Service que são úteis para entender como o seu aplicativo é reconhecido e funciona no AEM as a Cloud Service.

[Depurar o AEM as a Cloud Service com o console do desenvolvedor](./developer-console.md)

## Navegador de repositório

O navegador de repositório é uma ferramenta potente que oferece visibilidade do armazenamento interno de dados do AEM, permitindo uma depuração fácil do ambiente do AEM as a Cloud Service. O navegador de repositório permite uma exibição somente leitura dos recursos e propriedades do AEM nos ambientes de produção, preparo e desenvolvimento, bem como nos serviços de autor, publicação e visualização.

[Depurar o AEM as a Cloud Service com o navegador de repositório](./repository-browser.md)
