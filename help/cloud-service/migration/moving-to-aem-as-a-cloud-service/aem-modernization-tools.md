---
title: Usar ferramentas de Modernização AEM para AEM as a Cloud Service
description: Saiba como AEM Ferramentas de Modernização são usadas para atualizar um projeto de AEM existente e conteúdo para ser AEM compatível.
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 2%

---


# Ferramentas de Modernização do AEM

Saiba como AEM Ferramentas de Modernização são usadas para atualizar um conteúdo AEM Sites existente para ser AEM compatível e estar alinhado às práticas recomendadas.

>[!VIDEO](https://video.tv.adobe.com/v/336965/?quality=12&learn=on)

## Usar ferramentas de Modernização AEM

![Ciclo de vida das Ferramentas de Modernização do AEM](./assets/aem-modernization-tools.png)

AEM Ferramentas de Modernização convertem automaticamente páginas AEM existentes compostas por modelos estáticos herdados, componentes de base e parsys - para usar abordagens modernas, como modelos editáveis, AEM Componentes WCM principais e Contêineres de layout.

## Atividades principais

+ Clonar AEM produção 6.x para executar as ferramentas de Modernização AEM
+ Baixe e instale o [ferramentas de Modernização do AEM mais recentes](https://github.com/adobe/aem-modernize-tools/releases/latest) sobre o clone de produção do AEM 6.x por meio do Gerenciador de pacotes

+ [Conversor de estrutura de página](https://opensource.adobe.com/aem-modernize-tools/pages/tools/page-structure.html) atualiza o conteúdo de página existente do modelo estático para um modelo editável mapeado usando contêineres de layout
   + Definir regras de conversão usando a configuração OSGi
   + Executar o Conversor de Estrutura de Página em relação às páginas existentes

+ [Conversor de componentes](https://opensource.adobe.com/aem-modernize-tools/pages/tools/component.html) atualiza o conteúdo de página existente do modelo estático para um modelo editável mapeado usando contêineres de layout
   + Definir regras de conversão por meio de definições de nó JCR/XML
   + Execute a ferramenta Conversor de componentes em páginas existentes

+ [Importador de políticas](https://opensource.adobe.com/aem-modernize-tools/pages/tools/policy-importer.html) cria políticas a partir da configuração Design
   + Definir regras de conversão usando definições de nó JCR/XML
   + Executar Importador de Política em relação às Definições de Design existentes
   + Aplicar políticas importadas a componentes e contêineres de AEM

+ [Conversor de Diálogo](https://opensource.adobe.com/aem-modernize-tools/pages/tools/dialog.html) converte caixas de diálogo do componente baseado em CoralUI 3 Classic(ExtJS) e CoralUI 2 para caixas de diálogo baseadas em CoralUI 3 TouchUI.
   + Execute a ferramenta Conversor de caixa de diálogo em caixas de diálogo existentes com base em ExtJS ou Coral2 UI
   + Sincronizar caixas de diálogo convertidas de volta no repositório Git

## Exercício prático

Aplique o seu conhecimento experimentando o que aprendeu com este exercício prático.

Antes de experimentar o exercício prático, certifique-se de ter assistido e compreendido o vídeo acima e os seguintes materiais:

+ [Pensando de forma diferente sobre AEM as a Cloud Service](./introduction.md)
+ [Modernização do repositório](./repository-modernization.md)
+ [Conteúdo variável e imutável](../../developing/basics/mutable-immutable.md)
+ [Estrutura do projeto AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=pt-BR)

Além disso, certifique-se de que concluiu o exercício prático anterior:

+ [BPA e CAM exercício prático](./bpa-and-cam.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology"><img alt="Repositório GitHub de exercício manual" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Mão com a modernização AEM</div>
            <p style="margin:1rem 0">
                Explore o uso das Ferramentas de Modernização AEM para atualizar um site WKND herdado para estar em conformidade com AEM práticas recomendadas as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Experimente as Ferramentas de Modernização do AEM</span>
            </a>
        </td>
    </tr>
</table>

## Outros recursos

+ [Baixar ferramentas de Modernização AEM](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [Documentação das Ferramentas de Modernização do AEM](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems - Introdução ao AEM Modernization Suite](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)



1. Implante o site do wknd legacy recém-modernizado no SDK do Cloud Service local. Disponível para download aqui:
+ [Portal de distribuição de software](https://experienceleague.adobe.com/docs/experience-cloud/software-distribution/home.htm).
