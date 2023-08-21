---
title: Pesquisa e indexação no AEM as a Cloud Service
description: Saiba mais sobre os índices de pesquisa do AEM as a Cloud Service, como converter as definições de índice do AEM 6 e como implantar índices.
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: 77b960315c07ba194642a412a0cc6049edcf7bd2
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 2%

---

# Pesquisa e indexação

Saiba mais sobre os índices de pesquisa do AEM as a Cloud Service, como converter as definições de índice do AEM AEM as a Cloud Service AEM 6 para serem compatíveis com o e como implantar índices para o as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/336963?quality=12&learn=on)

## Ferramenta Conversor de índice

![Ferramenta Conversor de índice](./assets/index-converter.png)

Como parte da refatoração de sua base de código, use o [Ferramenta Conversor de índice](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) para converter definições de índice Oak personalizadas em definições de índice compatíveis com AEM as a Cloud Service.

Revise o [documentação do conversor de índice](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/refactoring-tools/index-converter.html) para o conjunto completo e atual de recursos do Conversor de índice.

## Atividades principais

+ Use o [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) ferramenta para migrar fluxos de trabalho de processamento de ativos para usar os microsserviços do Asset compute.
+ Configurar um [ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR) e implantar os índices personalizados. Verifique se os índices atualizados estão atualizados.
+ Implante a base de código atualizada em um ambiente de desenvolvimento as a Cloud Service para AEM e continue a validar.
+ Se estiver modificando um índice pronto para uso **SEMPRE** copie a definição de índice mais recente de um ambiente AEM as a Cloud Service em execução na versão mais recente. Modifique a definição do índice copiado para atender às suas necessidades.

## Exercício prático

Aplique seu conhecimento experimentando o que você aprendeu com este exercício prático.

Antes de experimentar o exercício prático, verifique se você assistiu e compreendeu o vídeo acima e os seguintes materiais:

+ [Pensando diferente sobre AEM as a Cloud Service](./introduction.md)
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
                Explore a definição e a implantação de índices Oak no AEM as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Experimentar indexação</span>
            </a>
        </td>
    </tr>
</table>
