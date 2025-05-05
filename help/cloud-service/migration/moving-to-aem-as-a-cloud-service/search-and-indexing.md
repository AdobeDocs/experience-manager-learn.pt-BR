---
title: Pesquisa e indexação no AEM as a Cloud Service
description: Saiba mais sobre os índices de pesquisa do AEM as a Cloud Service, como converter definições de índice do AEM 6 e como implantar índices.
version: Experience Manager as a Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
duration: 1231
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---

# Pesquisa e indexação

Saiba mais sobre os índices de pesquisa do AEM as a Cloud Service, como converter as definições de índice do AEM 6 para que sejam compatíveis com o AEM as a Cloud Service e como implantar índices para o AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3454719?quality=12&learn=on&captions=por_br)

## Ferramenta Conversor de índice

![Ferramenta Conversor de Índice](./assets/index-converter.png)

Como parte da refatoração de sua base de código, use a [ferramenta de conversão de índice](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) para converter definições de índice Oak personalizadas em definições de índice compatíveis com AEM as a Cloud Service.

Revise a [documentação do conversor de índice](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/refactoring-tools/index-converter.html?lang=pt-BR) para obter o conjunto completo e atual de recursos do Conversor de Índice.

## Atividades principais

+ Use a ferramenta [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) para migrar fluxos de trabalho de processamento de ativos para usar os microsserviços da Asset Compute.
+ Configure um [ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR) e implante os índices personalizados. Verifique se os índices atualizados estão atualizados.
+ Implante a base de código atualizada em um ambiente de desenvolvimento do AEM as a Cloud Service e continue a validar.
+ Se você modificar um índice pronto para uso **SEMPRE**, copie a definição de índice mais recente de um ambiente do AEM as a Cloud Service em execução na versão mais recente. Modifique a definição do índice copiado para atender às suas necessidades.

## Exercício prático

Aplique seu conhecimento experimentando o que você aprendeu com este exercício prático.

Antes de experimentar o exercício prático, verifique se você assistiu e compreendeu o vídeo acima e os seguintes materiais:

+ [Pensar de forma diferente sobre o AEM as a Cloud Service](./introduction.md)
+ [Modernização do repositório](./repository-modernization.md)

Além disso, verifique se você concluiu o exercício prático anterior:

+ [Exercício prático da Ferramenta de transferência de conteúdo](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="Repositório GitHub de exercícios práticos" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Práticas com índices</div>
            <p style="margin:1rem 0">
                Explore a definição e a implantação de índices do Oak no AEM as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Experimentar indexação</span>
            </a>
        </td>
    </tr>
</table>
