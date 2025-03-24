---
title: Introdução ao AEM Sites - Arquétipo de projeto
description: Introdução ao AEM Sites - Arquétipo de projeto. O tutorial do WKND é um tutorial em várias partes projetado para desenvolvedores novos no Adobe Experience Manager. O tutorial aborda a implementação de um site do AEM para uma marca fictícia de estilo de vida, a WKND. O tutorial aborda tópicos fundamentais como configuração de projetos, arquétipos maven, Componentes principais, Modelos editáveis, bibliotecas de clientes e desenvolvimento de componentes.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
doc-type: Tutorial
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
duration: 74
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 18%

---

# Introdução ao AEM Sites - Arquétipo de projeto {#project-archetype}

{{edge-delivery-services-and-page-editor}}

Bem-vindo a um tutorial em várias partes projetado para desenvolvedores novos no Adobe Experience Manager (AEM). Este tutorial aborda a implementação de um site do AEM para uma marca fictícia de estilo de vida, a WKND.

Este tutorial começa usando o [Arquétipo de projeto do AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=pt-BR) para gerar um novo projeto.

O tutorial foi projetado para funcionar com o **AEM as a Cloud Service** e tem compatibilidade retroativa com o **AEM 6.5.14+**. O site é implementado usando:

* [Arquétipo de projeto do AEM Maven](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=pt-BR)
* [Componentes principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)
* [Modelos sling](https://sling.apache.org/documentation/bundles/models.html)
* [Modelos editáveis](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=pt-BR)
* [Sistema de estilos](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=pt-BR)

*Estimativa de 1 a 2 horas para passar por cada parte do tutorial.*

## Ambiente de desenvolvimento local {#local-dev-environment}

Um ambiente de desenvolvimento local é necessário para concluir este tutorial. Capturas de tela e vídeos são capturados usando o AEM as a Cloud Service SDK em execução em um ambiente macOS com o [Visual Studio Code](https://code.visualstudio.com/) como IDE. Os comandos e o código devem ser independentes do sistema operacional local, a menos que indicado de outra forma.

### Software necessário

Devem ser instalados:

* [Instância de **Autor** do AEM local](https://experience.adobe.com/#/downloads) (Cloud Service SDK ou 6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou mais recente)
* [Node.js](https://nodejs.org/en/) (LTS - Suporte a Longo Prazo)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio Code](https://code.visualstudio.com/) ou IDE equivalente
   * [Sincronização do AEM VSCode](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - Ferramenta usada em todo o tutorial

>[!NOTE]
>
> **Novo no AEM as a Cloud Service?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR).
>
> **Novo no AEM 6.5?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=pt-BR).

## GitHub {#github}

O código deste tutorial pode ser encontrado no GitHub, no repositório do Guia do AEM:

**[GitHub: Projeto de Sites WKND](https://github.com/adobe/aem-guides-wknd)**

Além disso, cada parte do tutorial tem sua própria ramificação no GitHub. Um usuário pode começar o tutorial a qualquer momento simplesmente verificando a ramificação que corresponde à parte anterior.

## Próximas etapas {#next-steps}

O que você está esperando? Inicie o tutorial navegando até o capítulo [Configuração do projeto](project-setup.md) e saiba como gerar um novo projeto do Adobe Experience Manager usando o Arquétipo de projeto do AEM.
