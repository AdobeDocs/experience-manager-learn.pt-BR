---
title: Configurar o BPA e o projeto CAM
description: Saiba como o Analisador de práticas recomendadas e o Cloud Acceleration Manager fornecem um guia personalizado para a migração para AEM as a Cloud Service.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 3%

---

# Gerenciador de aceleração de nuvem e do Analisador de práticas recomendadas

Saiba como o BPA (Best Practices Analyzer) e o CAM (Cloud Acceleration Manager) fornecem um guia personalizado para migrar para AEM as a Cloud Service. 

>[!VIDEO](https://video.tv.adobe.com/v/336957/?quality=12&learn=on)

## Uso de BPA e CAM

![Diagrama de alto nível de BPA e CAM](assets/bpa-cam-diagram.png)

O pacote BPA deve ser instalado em um clone do ambiente de produção AEM 6.x. O BPA gerará um relatório que poderá ser carregado para o CAM, o que fornecerá orientação sobre as atividades principais que precisam ocorrer para se mudarem para o AEM as a Cloud Service.

## Atividades principais

+ Faça um clone do ambiente 6.x de produção. À medida que você migra o conteúdo e o código de refatoração, ter um clone de um ambiente de produção será valioso para testar várias ferramentas e alterações.
+ Baixe a ferramenta BPA mais recente da [Portal de distribuição de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) e instale no ambiente clonado do AEM 6.x.
+ Use a ferramenta BPA para gerar um relatório que pode ser carregado para o Cloud Acceleration Manager (CAM). O CAM é acessado por meio de [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
+ Use CAM para fornecer orientação sobre quais atualizações precisam ser feitas na base de código e no ambiente atuais, a fim de mudar para AEM as a Cloud Service.

## Exercício prático

Aplique o seu conhecimento experimentando o que aprendeu com este exercício prático.

Antes de experimentar o exercício prático, certifique-se de ter assistido e compreendido o vídeo acima e os seguintes materiais:

+ [Pensando de forma diferente sobre AEM as a Cloud Service](./introduction.md)
+ [O que é AEM as a Cloud Service?](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/what-is-aem-as-a-cloud-service.html?lang=en)
+ [Arquitetura do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/architecture.html?lang=en)
+ [Conteúdo variável e imutável](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/mutable-immutable.html?lang=en)
+ [Diferenças no desenvolvimento para AEM as a Cloud Service e AEM 6.x](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html#developing)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently"><img alt="Repositório GitHub de exercício manual" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Manual com o Analisador de práticas recomendadas</div>
            <p style="margin:1rem 0">
                Explore o BPA (Best Practices Analyzer) e revise os resultados executando-o em uma base de código WKND herdada que contém exemplos de violações.
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

+ [Baixe o Analisador de práticas recomendadas](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Best*+Practices*+Analyzer*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)