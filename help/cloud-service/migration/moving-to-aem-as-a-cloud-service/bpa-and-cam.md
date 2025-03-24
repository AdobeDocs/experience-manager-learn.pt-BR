---
title: Configurar seu projeto BPA e CAM
description: Saiba como o Analisador de práticas recomendadas e o Cloud Acceleration Manager fornecem um guia personalizado para migrar para o AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
duration: 680
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 2%

---

# Analisador de práticas recomendadas e Cloud Acceleration Manager

Saiba como o Analisador de práticas recomendadas (BPA) e o Cloud Acceleration Manager (CAM) fornecem um guia personalizado sobre migração para o AEM as a Cloud Service. 

>[!VIDEO](https://video.tv.adobe.com/v/336957?quality=12&learn=on)

## Uso de BPA e CAM

![Diagrama de alto nível de BPA e CAM](assets/bpa-cam-diagram.png)

O pacote de BPA deve ser instalado em um clone do ambiente de produção do AEM 6.x. O BPA gerará um relatório que poderá ser carregado no CAM, o que fornecerá orientação sobre as principais atividades que devem ocorrer para migrar para o AEM as a Cloud Service.

## Atividades principais

+ Faça um clone de seu ambiente de produção 6.x. À medida que você migra o conteúdo e refatora o código, ter um clone de um ambiente de produção é algo valioso para testar várias ferramentas e alterações.
+ Baixe a ferramenta BPA mais recente do [Portal de distribuição de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) e instale no ambiente clonado do AEM 6.x.
+ Use a ferramenta BPA para gerar um relatório que pode ser carregado no Cloud Acceleration Manager (CAM). O CAM é acessado pelo [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
+ Use o CAM para fornecer orientação sobre quais atualizações precisam ser feitas na base de código e no ambiente atuais para migrar para o AEM as a Cloud Service.

## Exercício prático

Aplique seu conhecimento experimentando o que você aprendeu com este exercício prático.

Antes de experimentar o exercício prático, verifique se você assistiu e compreendeu o vídeo acima e os seguintes materiais:

+ [Pensar de forma diferente sobre o AEM as a Cloud Service](./introduction.md)
+ [O que é o AEM as a Cloud Service?](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/what-is-aem-as-a-cloud-service.html?lang=en)
+ [Arquitetura do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/architecture.html?lang=en)
+ [Conteúdo mutável e imutável](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/mutable-immutable.html?lang=en)
+ [Diferenças no desenvolvimento para o AEM as a Cloud Service e o AEM 6.x](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html#developing)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently"><img alt="Repositório GitHub de exercícios práticos" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Práticas com o Analisador de práticas recomendadas</div>
            <p style="margin:1rem 0">
                Explore o Analisador de práticas recomendadas (BPA) e revise os resultados ao executá-lo em uma base de código WKND herdada que contém violações de exemplo.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Experimente o Analisador de práticas recomendadas</span>
            </a>
        </td>
    </tr>
</table>


## Outros recursos

+ [Baixar o Analisador de Práticas Recomendadas](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Best*+Practices*+Analyzer*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)