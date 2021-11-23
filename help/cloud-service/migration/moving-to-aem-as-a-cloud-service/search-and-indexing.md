---
title: Pesquisa e indexação em AEM as a Cloud Service
description: Saiba mais sobre AEM índices de pesquisa do as a Cloud Service, como converter AEM 6 definições de índice e como implantar índices.
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 2%

---

# Pesquisa e indexação

Saiba mais sobre AEM índices de pesquisa do as a Cloud Service, como converter AEM 6 definições de índice para serem AEM compatíveis e como implantar índices para AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/336963/?quality=12&learn=on)

## Ferramenta Conversor de índice

![Ferramenta Conversor de índice](./assets/index-converter.png)

Como parte da refatoração da base de código, use o [Ferramenta conversor de índice](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) para converter definições de índice Oak personalizadas em AEM definições de índice compatíveis as a Cloud Service.

## Atividades principais

+ Use o [Migrador de fluxo de trabalho do Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) ferramenta para migrar fluxos de trabalho de processamento de ativos para usar os microsserviços do Asset compute.
+ Configure um [ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR) e implante os índices personalizados. Verifique se os índices atualizados estão atualizados.
+ Implante a base de código atualizada em um ambiente de desenvolvimento as a Cloud Service AEM e continue a validar.
+ Se modificar um índice predefinido **SEMPRE** copie a definição de índice mais recente de um ambiente AEM as a Cloud Service em execução na versão mais recente. Modifique a definição do índice copiado para atender às suas necessidades.

## Exercício prático

Aplique o seu conhecimento experimentando o que aprendeu com este exercício prático.

Antes de experimentar o exercício prático, certifique-se de ter assistido e compreendido o vídeo acima e os seguintes materiais:

+ [Pensando de forma diferente sobre AEM as a Cloud Service](./introduction.md)
+ [Modernização do Repositório](./repository-modernization.md)

Além disso, certifique-se de que concluiu o exercício prático anterior:

+ [Exercício prático da ferramenta Transferência de conteúdo](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="Repositório GitHub de exercício manual" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Mão a mão com índices</div>
            <p style="margin:1rem 0">
                Explore a definição e implantação de índices Oak para AEM as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Experimente indexação</span>
            </a>
        </td>
    </tr>
</table>
