---
title: Depuração do AEM as a Cloud Service
description: em infraestruturas em nuvem dimensionáveis e de autoatendimento, o que faz com que os desenvolvedores de AEM entendam como entender e depurar vários aspectos do AEM as a Cloud Service, desde a criação e a implantação até a obtenção de detalhes sobre a execução de aplicativos do AEM.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
duration: 60
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 1%

---

# Depuração do AEM as a Cloud Service

O AEM as a Cloud Service é a maneira nativa em nuvem de aproveitar os aplicativos AEM. O AEM as a Cloud Service é executado em uma infraestrutura de nuvem dimensionável e de autoatendimento, o que requer que os desenvolvedores AEM entendam como entender e depurar várias facetas do AEM as a Cloud Service AEM, desde a criação e implantação até a obtenção de detalhes sobre a execução de aplicativos do.

## Logs

Os registros fornecem detalhes sobre como o aplicativo está funcionando no AEM as a Cloud Service, bem como insights sobre problemas com implantações.

[Depuração do AEM as a Cloud Service usando logs](./logs.md)

## Criar e implantar

Os pipelines Adobe Cloud Manager implantam o aplicativo AEM por meio de uma série de etapas para determinar a qualidade e a viabilidade do código quando implantado no AEM as a Cloud Service. Cada uma das etapas pode resultar em falha, o que torna importante entender como depurar builds para determinar a causa raiz da falha e como resolvê-las.

[Depuração da build e da implantação do AEM as a Cloud Service](./build-and-deployment.md)

## Console do desenvolvedor

O console do desenvolvedor fornece diversas informações e introspecções em ambientes do AEM as a Cloud Service que são úteis para entender como seu aplicativo é reconhecido pelo e funciona no AEM as a Cloud Service.

[Depurando o AEM as a Cloud Service com o Developer Console](./developer-console.md)

## Navegador de repositório

O Navegador do repositório é uma ferramenta poderosa que oferece visibilidade do armazenamento de dados subjacente do AEM, permitindo uma depuração fácil do ambiente do AEM as a Cloud Service. O Navegador do repositório é compatível com uma exibição somente leitura dos recursos e propriedades do AEM nos ambientes de Produção, Preparo e Desenvolvimento, bem como com os serviços Author, Publish e Preview.

[Depuração do AEM as a Cloud Service com o Navegador do repositório](./repository-browser.md)
