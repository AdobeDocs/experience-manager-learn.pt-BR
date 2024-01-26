---
title: Configuração do Dispatcher ao migrar para o AEM as a Cloud Service
description: Saiba mais sobre alterações notáveis no AEM Dispatcher para AEM as a Cloud Service, a ferramenta de conversão do Dispatcher e como usar o SDK de ferramentas do Dispatcher.
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
duration: 1642
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 4%

---


# Dispatcher

Saiba mais sobre o AEM Dispatcher para AEM as a Cloud Service, com foco em alterações notáveis do Dispatcher para AEM 6, a ferramenta de conversão do Dispatcher e como usar o SDK de ferramentas do Dispatcher.

>[!VIDEO](https://video.tv.adobe.com/v/336962?quality=12&learn=on)

## Conversor do Dispatcher Converter

![Conversor do Dispatcher Converter](./assets/dispatcher-converter-diagram.png)

Como parte da refatoração de sua base de código, use o [Conversor do Dispatcher do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) para refatorar configurações existentes no local ou no Adobe Managed Services Dispatcher para configurações compatíveis com o AEM as a Cloud Service no Dispatcher.

## Atividades principais

+ Use o [Ferramenta Conversor do Dispatcher do Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) para migrar uma configuração existente do Dispatcher.
+ Faça referência ao módulo Dispatcher no [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) como prática recomendada.
+ [Configurar ferramentas locais do Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html?lang=pt-BR) para validar o dispatcher, antes de testar em um ambiente Cloud Service.

## Exercício prático

Aplique seu conhecimento experimentando o que você aprendeu com este exercício prático.

Antes de experimentar o exercício prático, verifique se você assistiu e compreendeu o vídeo acima e os seguintes materiais:

+ [Ferramentas de Modernização do AEM](./aem-modernization-tools.md)
+ [Integração](./onboarding.md)
+ [Cloud Manager](./cloud-manager.md)

Além disso, verifique se você concluiu o exercício prático anterior:

+ [Exercício prático do Cloud Manager](./cloud-manager.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher"><img alt="Repositório GitHub de exercícios práticos" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Práticas com ferramentas do Dispatcher</div>
            <p style="margin:1rem 0">
                Explore o usando as Ferramentas do Dispatcher do SDK do AEM para validar as configurações do Dispatcher, bem como executar o AEM Dispatcher localmente usando o Docker.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Experimente as Ferramentas do Dispatcher</span>
            </a>
        </td>
    </tr>
</table>
