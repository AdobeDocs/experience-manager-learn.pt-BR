---
title: Microserviços AEM Assets e mudança para AEM as a Cloud Service
description: Saiba como os microsserviços de asset compute do AEM Assets as a Cloud Service permitem que você gere de forma automática e eficiente qualquer representação dos seus ativos, substituindo essa função do fluxo de trabalho AEM tradicional.
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 3%

---

# Microserviços AEM Assets - Migração para AEM as a Cloud Service

Saiba como os microsserviços de asset compute do AEM Assets as a Cloud Service permitem que você gere de forma automática e eficiente qualquer representação dos seus ativos, substituindo essa função do fluxo de trabalho AEM tradicional.

>[!VIDEO](https://video.tv.adobe.com/v/336990/?quality=12&learn=on)

## Ferramenta Migração de fluxo de trabalho

![Ferramenta Migração de fluxo de trabalho de ativos](./assets/asset-workflow-migration.png)

Como parte da refatoração da base de código, use o [Ferramenta Migração de fluxo de trabalho de ativos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html) para migrar workflows existentes para usar os microsserviços do Asset compute em AEM as a Cloud Service.

## Atividades principais

+ Use o [Migrador de fluxo de trabalho do Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) ferramenta para migrar fluxos de trabalho de processamento de ativos para usar os microsserviços do Asset compute.
+ Configure um [ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR) e implante os workflows atualizados. Pode ser necessário um ajuste manual para fluxos de trabalho complexos.
+ Continue a iterar em um ambiente de desenvolvimento local usando o SDK do AEM até que o fluxo de trabalho atualizado corresponda à paridade de recursos.
+ Implante a base de código atualizada em um ambiente de desenvolvimento as a Cloud Service AEM e continue a validar.

## Exercício prático

Aplique o seu conhecimento experimentando o que aprendeu com este exercício prático.

Antes de experimentar o exercício prático, certifique-se de ter assistido e compreendido o vídeo acima e os seguintes materiais:

+ [Pensando de forma diferente sobre AEM as a Cloud Service](./introduction.md)
+ [Integração](./onboarding.md)

Além disso, certifique-se de que concluiu o exercício prático anterior:

+ [Buscar e indexar exercício prático](./search-and-indexing.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices"><img alt="Repositório GitHub de exercício manual" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Mão com o upload de ativos</div>
            <p style="margin:1rem 0">
                Explore como definir e atribuir Perfis de processamento do AEM Assets a pastas e fazer upload de ativos para AEM usando o módulo CLI "aem-upload" npm.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Experimente o gerenciamento de ativos</span>
            </a>
        </td>
    </tr>
</table>
