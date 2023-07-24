---
title: Tutorial Integrar o Analytics e o Real-time Customer Data Platform ao conector de origem do Experience Platform
description: Saiba como integrar o Adobe Analytics ao Real-time Customer Data Platform.
solution: Real-Time Customer Data Platform, Analytics
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
index: true
hidefromtoc: true
kt: null
thumbnail: null
last-substantial-update: 2023-04-11T00:00:00Z
badgeIntegration: label="Integração" type="positive"
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 3%

---


# Integrar o Adobe Analytics e o Real-time Customer Data Platform com o conector de origem do Experience Platform

<ol>
    <li><a href="https://experienceleague.adobe.com/?lang=en#dashboard/learning" _target="_blank" rel="noopener noreferrer">Criar esquemas</a> para que os dados sejam assimilados.</li>
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html" _target="_blank" rel="noopener noreferrer">Criar conjuntos de dados</a> para que os dados sejam assimilados.</a></li>
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/identities/label-ingest-and-verify-identity-data.html?lang=en" _target="_blank" rel="noopener noreferrer">Configure as identidades e os namespaces de identidade corretos no esquema</a> para garantir que os dados assimilados possam se unir a um perfil unificado.</li> 
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/profiles/bring-data-into-the-real-time-customer-profile.html" _target="_blank" rel="noopener noreferrer">Ativar os esquemas e conjuntos de dados para o perfil</a>.</li>
    <li>Assimile dados no Experience Platform usando um destes métodos:</li>
        <ul>
            <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/sources/ingest-data-from-adobe-analytics.html" _target="_blank" rel="noopener noreferrer">Conector de origem do Adobe Analytics</a></li>
            <li>SDK da Web do Experience Platform:</li>
                <ul>
                    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=pt-BR" _target="_blank" rel="noopener noreferrer">Tutorial</a></li>
                    <li><a href="https://experienceleague.adobe.com/docs/analytics/implementation/aep-edge/web-sdk/overview.html" _target="_blank" rel="noopener noreferrer">Lista de verificação</a></li>
                </ul>
            <li>SDK móvel do Experience Platform:</li>
                <ul>
                    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/data-collection/mobile-sdk/create-mobile-properties.html" _target="_blank" rel="noopener noreferrer">Tutorial</a></li>
                    <li><a href="https://experienceleague.adobe.com/docs/analytics/implementation/aep-edge/mobile-sdk/overview.html" _target="_blank" rel="noopener noreferrer">Lista de verificação</a></li>
                </ul></li>
            <li>API do servidor de rede de borda:</li>
                <ul>
                    <li><a href="https://experienceleague.adobe.com/docs/experience-platform/edge-network-server-api/interacting-other-adobe-solutions/interacting-adobe-analytics.html" _target="_blank" rel="noopener noreferrer">Tutorial</a></li>
                </ul>
       </ul>
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html" _target="_blank" rel="noopener noreferrer">Criar segmentos no Experience Platform.</a> O sistema determina automaticamente se o segmento é avaliado como batch (conector de dados) ou streaming (rede de borda).</li>
    <li><a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/destinations/create-destinations-and-activate-data.html" _target="_blank" rel="noopener noreferrer">Configure destinos para o compartilhamento de atributos de perfil e associações de público-alvo com os destinos desejados.</a></li>   
</ol>

>[!NOTE]
>
>As etapas de fluxo de trabalho padrão do conector de origem do Adobe Analytics criam o esquema e o conjunto de dados usados para assimilar os dados do Analytics &quot;como estão&quot;. Portanto, as duas primeiras etapas são tratadas pelo sistema. O fluxo de trabalho de mapeamento requer a criação de atributos personalizados; portanto, siga a sequência de etapas completamente.