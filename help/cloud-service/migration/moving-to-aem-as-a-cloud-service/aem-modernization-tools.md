---
title: Utilização de ferramentas de modernização do AEM para migrar para o AEM as a Cloud Service
description: Saiba como as Ferramentas de modernização do AEM são usadas para atualizar um projeto e conteúdo existente do AEM para serem compatíveis com o AEM as a Cloud Service.
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 6%

---


# Ferramentas de Modernização do AEM

Saiba como as Ferramentas de modernização do AEM são usadas para atualizar um conteúdo existente do AEM Sites para serem compatíveis com o AEM as a Cloud Service e se alinharem às práticas recomendadas.

## Conversor tudo-em-um

>[!VIDEO](https://video.tv.adobe.com/v/338802?quality=12&learn=on)

## Conversão de página

>[!VIDEO](https://video.tv.adobe.com/v/338799?quality=12&learn=on)

## Conversão de componente

>[!VIDEO](https://video.tv.adobe.com/v/338788?quality=12&learn=on)

## Importação de política

>[!VIDEO](https://video.tv.adobe.com/v/338797?quality=12&learn=on)

## Uso de ferramentas de modernização do AEM

![Ciclo de vida das ferramentas de modernização do AEM](./assets/aem-modernization-tools.png)

As ferramentas de Modernização do AEM convertem automaticamente as páginas AEM existentes compostas por modelos estáticos herdados, componentes de base e os parsys, para usar abordagens modernas, como modelos editáveis, componentes WCM principais do AEM e contêineres de layout.

## Atividades principais

+ Clonar a produção do AEM 6.x para executar as ferramentas de Modernização do AEM
+ Baixe e instale o [ferramentas mais recentes de modernização do AEM](https://github.com/adobe/aem-modernize-tools/releases/latest) no clone de produção do AEM 6.x via Gerenciador de pacotes

+ [Conversor de estrutura de página](https://opensource.adobe.com/aem-modernize-tools/pages/structure/about.html) atualiza o conteúdo de página existente do modelo estático para um modelo editável mapeado usando contêineres de layout
   + Definir regras de conversão usando a configuração do OSGi
   + Executar o Conversor de estrutura de página em relação às páginas existentes

+ [Conversor de componentes](https://opensource.adobe.com/aem-modernize-tools/pages/component/about.html) atualiza o conteúdo de página existente do modelo estático para um modelo editável mapeado usando contêineres de layout
   + Definir regras de conversão por meio de definições de nó JCR/XML
   + Executar a ferramenta Conversor de componentes em relação às páginas existentes

+ [Importador de políticas](https://opensource.adobe.com/aem-modernize-tools/pages/policy/about.html) cria políticas a partir da Configuração de design
   + Definir regras de conversão usando definições de nó JCR/XML
   + Executar o Importador de políticas em relação às definições de projeto existentes
   + Aplicação de políticas importadas a componentes e contêineres AEM

## Exercício prático

Aplique seu conhecimento experimentando o que você aprendeu com este exercício prático.

Antes de experimentar o exercício prático, verifique se você assistiu e compreendeu o vídeo acima e os seguintes materiais:

+ [Pensando diferente sobre AEM as a Cloud Service](./introduction.md)
+ [Modernização do repositório](./repository-modernization.md)
+ [Conteúdo mutável e imutável](../../developing/basics/mutable-immutable.md)
+ [Estrutura de projeto do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=pt-BR)

Além disso, verifique se você concluiu o exercício prático anterior:

+ [Exercício prático de BPA e CAM](./bpa-and-cam.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology"><img alt="Repositório GitHub de exercícios práticos" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Práticas com a modernização do AEM</div>
            <p style="margin:1rem 0">
                Explore o uso das Ferramentas de modernização do AEM para atualizar um site WKND herdado para estar em conformidade com as práticas recomendadas as a Cloud Service do AEM.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Experimente as ferramentas de modernização do AEM</span>
            </a>
        </td>
    </tr>
</table>

## Outros recursos

+ [Baixar ferramentas de modernização do AEM](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [Documentação das ferramentas de modernização do AEM](https://opensource.adobe.com/aem-modernize-tools/)
+ [Gems AEM - Introdução ao AEM Modernization Suite](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)

1. Implante o site wknd-legacy recém-modernizado no SDK AEM local. AEM ASK disponível para download aqui:
   + [Portal de distribuição de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html).
