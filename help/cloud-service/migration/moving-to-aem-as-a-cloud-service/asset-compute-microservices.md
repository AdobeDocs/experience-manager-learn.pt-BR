---
title: Microsserviços da AEM Assets e migração para o AEM as a Cloud Service
description: Saiba como os microsserviços de Asset Compute do AEM Assets as a Cloud Service permitem gerar qualquer representação dos seus ativos de forma automática e eficiente, substituindo essa função do fluxo de trabalho tradicional do AEM.
version: Experience Manager as a Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
duration: 973
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 1%

---

# Microsserviços da AEM Assets - Migração para o AEM as a Cloud Service

Saiba como os microsserviços de Asset Compute do AEM Assets as a Cloud Service permitem gerar qualquer representação dos seus ativos de forma automática e eficiente, substituindo essa função do fluxo de trabalho tradicional do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/336990?quality=12&learn=on)

## Ferramenta Migração de fluxo de trabalho

![Ferramenta de migração de fluxo de trabalho de ativos](./assets/asset-workflow-migration.png)

Como parte da refatoração de sua base de código, use a [ferramenta de Migração de Fluxo de Trabalho de Ativos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html?lang=pt-BR) para migrar fluxos de trabalho existentes e usar os microsserviços da Asset Compute no AEM as a Cloud Service.

## Atividades principais

+ Use a ferramenta [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) para migrar fluxos de trabalho de processamento de ativos para usar os microsserviços da Asset Compute.
+ Configure um [ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR) e implante os fluxos de trabalho atualizados. O ajuste manual pode ser necessário para workflows complexos.
+ Continue a iterar em um ambiente de desenvolvimento local usando o AEM SDK até que o fluxo de trabalho atualizado corresponda à paridade de recursos.
+ Implante a base de código atualizada em um ambiente de desenvolvimento do AEM as a Cloud Service e continue a validar.

## Exercício prático

Aplique seu conhecimento experimentando o que você aprendeu com este exercício prático.

Antes de experimentar o exercício prático, verifique se você assistiu e compreendeu o vídeo acima e os seguintes materiais:

+ [Pensar de forma diferente sobre o AEM as a Cloud Service](./introduction.md)
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
                Explore como definir e atribuir perfis de processamento do AEM Assets a pastas e fazer upload de ativos para o AEM usando o módulo CLI npm "aem-upload".
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Experimente o gerenciamento de ativos</span>
            </a>
        </td>
    </tr>
</table>
