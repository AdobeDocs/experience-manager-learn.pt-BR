---
title: Introdução ao AEM Sites - Arquétipo de projeto
description: Introdução ao AEM Sites - Arquétipo de projeto. O tutorial do WKND é um tutorial em várias partes projetado para desenvolvedores novos no Adobe Experience Manager. O tutorial aborda a implementação de um site AEM para uma marca fictícia de estilo de vida, a WKND. O tutorial aborda tópicos fundamentais como configuração de projetos, arquétipos maven, Componentes principais, Modelos editáveis, bibliotecas de clientes e desenvolvimento de componentes.
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
source-git-commit: bbdb045edf5f2c68eec5094e55c1688e725378dc
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 31%

---

# Introdução ao AEM Sites - Arquétipo de projeto {#project-archetype}

Bem-vindo a um tutorial em várias partes projetado para desenvolvedores novos no Adobe Experience Manager (AEM). Esse tutorial aborda a implementação de um site do AEM para uma marca fictícia de estilo de vida, a WKND.

Este tutorial começa usando o [Arquétipo de projeto AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=pt-BR) para gerar um novo projeto.

O tutorial foi projetado para funcionar com **AEM as a Cloud Service** e é compatível com versões anteriores com **AEM 6.5.14+**. O site é implementado usando:

* [Arquétipo de projeto do AEM Maven](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=pt-BR)
* [Componentes principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)
* [Modelos sling](https://sling.apache.org/documentation/bundles/models.html)
* [Modelos editáveis](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=pt-BR)
* [Sistema de estilos](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=pt-BR)

*Estime de 1 a 2 horas para passar por cada parte do tutorial.*

## Ambiente de desenvolvimento local {#local-dev-environment}

Um ambiente de desenvolvimento local é necessário para concluir este tutorial. Capturas de tela e vídeos são capturados usando o SDK as a Cloud Service do AEM em execução em um ambiente macOS com [Código do Visual Studio](https://code.visualstudio.com/) como o IDE. Os comandos e o código devem ser independentes do sistema operacional local, a menos que indicado de outra forma.

### Software necessário

Devem ser instalados:

* [AEM local **Autor** instância](https://experience.adobe.com/#/downloads) (Cloud Service SDK ou 6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou mais recente)
* [Node.js](https://nodejs.org/en/) (LTS - Suporte a longo prazo)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Código do Visual Studio](https://code.visualstudio.com/) ou IDE equivalente
   * [Sincronização VSCode com AEM](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - Ferramenta usada no tutorial

>[!NOTE]
>
> **Novo no AEM as a Cloud Service?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR).
>
> **Novo no AEM 6.5?** Confira o [guia a seguir para configurar um ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=pt-BR).

## GitHub {#github}

O código deste tutorial pode ser encontrado no GitHub no repositório do Guia AEM:

**[GitHub: projeto do WKND Sites](https://github.com/adobe/aem-guides-wknd)**

Além disso, cada parte do tutorial tem sua própria ramificação no GitHub. Um usuário pode começar o tutorial a qualquer momento simplesmente verificando a ramificação que corresponde à parte anterior.

## Próximas etapas {#next-steps}

O que você está esperando? Inicie o tutorial navegando até o [Configuração do projeto](project-setup.md) e saiba como gerar um novo projeto do Adobe Experience Manager usando o Arquétipo de projeto AEM.
