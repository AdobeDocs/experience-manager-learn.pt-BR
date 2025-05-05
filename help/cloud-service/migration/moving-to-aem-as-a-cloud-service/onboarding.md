---
title: Integração ao AEM as a Cloud Service
description: Saiba mais sobre a integração com o AEM as a Cloud Service, desde a fase de contrato até a configuração de ambientes usando o Cloud Manager.
version: Experience Manager as a Cloud Service
feature: Onboarding
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8631
thumbnail: 336959.jpeg
exl-id: 9d2004e5-e928-4190-8298-695635c8e92c
duration: 504
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 7%

---

# Integração ao AEM as a Cloud Service

Saiba mais sobre a integração com o AEM as a Cloud Service, desde a fase de contrato até a configuração de ambientes, usando o Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3431501?quality=12&learn=on&captions=por_br)

## Cloud Manager e Admin Console

![Diagrama de alto nível de integração](assets/onboarding-diagram.png)

Uma parte crítica da integração é criar programas do AEM as a Cloud Service e provisionar vários ambientes usando o Adobe Cloud Manager. O [Admin Console](https://adminconsole.adobe.com/) é usado para atribuir funções e fornecer acesso aos ambientes do AEM aos usuários em sua organização.

## Principais atividades

+ Um administrador do sistema usa o [Admin Console](https://adminconsole.adobe.com/) para atribuir um ou mais usuários ao perfil de produto [Cloud Manager - Proprietário da empresa](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/setting-up-users-and-roles.html?lang=pt-BR).
+ Os usuários atribuídos ao Perfil de Produto Proprietário da Empresa usam os recursos de autoatendimento do [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html?lang=pt-BR) para [criar programas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/production-programs/creating-production-program.html?lang=pt-BR) e [adicionar ambientes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html?lang=pt-BR)
+ Use o [Admin Console](https://adminconsole.adobe.com/) para atribuir desenvolvedores e usuários a diferentes [funções do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/setting-up-users-and-roles.html?lang=pt-BR) e conceder permissão a vários ambientes do AEM.

## Exercício prático

Aplique seu conhecimento experimentando o que você aprendeu com este exercício prático.

Antes de experimentar o exercício prático, verifique se você assistiu e compreendeu o vídeo acima e os seguintes materiais:

+ [Pensar de forma diferente sobre o AEM as a Cloud Service](./introduction.md)
+ [Cloud Manager](./cloud-manager.md)

Além disso, verifique se você concluiu o exercício prático anterior:

+ [Exercício prático de Ferramentas de modernização do AEM](./aem-modernization-tools.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session3-onboarding#bootcamp---session-3-on-boarding"><img alt="Repositório GitHub de exercícios práticos" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Práticas com integração</div>
            <p style="margin:1rem 0">
                Explore o processo de integração do AEM as a Cloud Service e como implantar um aplicativo do AEM no AEM SDK.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session3-onboarding#bootcamp---session-3-on-boarding" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Experimente a integração</span>
            </a>
        </td>
    </tr>
</table>
