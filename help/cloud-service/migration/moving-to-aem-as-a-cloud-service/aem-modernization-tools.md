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
source-git-commit: 3657e7798774f9cc673ff6ccd8af1a555b1d4013
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 1%

---


# Ferramentas de Modernização do AEM

Saiba como AEM Ferramentas de Modernização são usadas para atualizar um conteúdo AEM Sites existente para ser AEM compatível e estar alinhado às práticas recomendadas.

>[!VIDEO](https://video.tv.adobe.com/v/336965/?quality=12&learn=on)

## Usar ferramentas de Modernização AEM

![Ciclo de vida das Ferramentas de Modernização do AEM](./assets/aem-modernization-tools.png)

AEM Ferramentas de Modernização convertem automaticamente páginas AEM existentes compostas por modelos estáticos herdados, componentes de base e parsys - para usar abordagens modernas, como modelos editáveis, AEM Componentes WCM principais e Contêineres de layout.

### Atividades principais

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

### Outros recursos

+ [Baixar ferramentas de Modernização AEM](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [Documentação das Ferramentas de Modernização do AEM](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems - Introdução ao AEM Modernization Suite](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)
