---
title: Microsserviços da AEM Assets e migração para o AEM as a Cloud Service
description: Saiba como os microsserviços do asset compute AEM do AEM Assets as a Cloud Service permitem gerar qualquer representação dos seus ativos de forma automática e eficiente, substituindo essa função do fluxo de trabalho tradicional do.
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
duration: 989
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 1%

---

# Microsserviços da AEM Assets - Migração para o AEM as a Cloud Service

Saiba como os microsserviços do asset compute AEM do AEM Assets as a Cloud Service permitem gerar qualquer representação dos seus ativos de forma automática e eficiente, substituindo essa função do fluxo de trabalho tradicional do.

>[!VIDEO](https://video.tv.adobe.com/v/336990?quality=12&learn=on)

## Ferramenta Migração de fluxo de trabalho

![Ferramenta Migração de fluxo de trabalho de ativos](./assets/asset-workflow-migration.png)

Como parte da refatoração de sua base de código, use o [Ferramenta Migração de fluxo de trabalho de ativos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html?lang=pt-BR) para migrar fluxos de trabalho existentes para usar os microsserviços do Asset compute no AEM as a Cloud Service.

## Atividades principais

+ Use o [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) ferramenta para migrar fluxos de trabalho de processamento de ativos para usar os microsserviços do Asset compute.
+ Configurar um [ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR) e implante os workflows atualizados. O ajuste manual pode ser necessário para workflows complexos.
+ Continue a iterar em um ambiente de desenvolvimento local usando o SDK do AEM até que o fluxo de trabalho atualizado corresponda à paridade de recursos.
+ Implante a base de código atualizada em um ambiente de desenvolvimento as a Cloud Service para AEM e continue a validar.

## Exercício prático

Aplique seu conhecimento experimentando o que você aprendeu com este exercício prático.

Antes de experimentar o exercício prático, verifique se você assistiu e compreendeu o vídeo acima e os seguintes materiais:

+ [Pensando diferente sobre AEM as a Cloud Service](./introduction.md)
+ [Integração](./onboarding.md)

Além disso, verifique se você concluiu o exercício prático anterior:

+ [Exercício prático de pesquisa e indexação](./search-and-indexing.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices"><img alt="Repositório GitHub de exercícios práticos" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Práticas com o upload de ativos</div>
            <p style="margin:1rem 0">
                Explore como definir e atribuir perfis de processamento do AEM Assets a pastas e fazer upload de ativos para AEM usando o módulo CLI npm "aem-upload".
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Experimente o gerenciamento de ativos</span>
            </a>
        </td>
    </tr>
</table>
