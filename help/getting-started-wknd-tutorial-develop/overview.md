---
title: Introdução ao AEM Sites - Tutorial do WKND
description: Introdução ao AEM Sites - Tutorial do WKND. O tutorial da WKND é um tutorial de várias partes projetado para desenvolvedores novos à Adobe Experience Manager. O tutorial percorre a implementação de um site AEM para uma marca fictícia de estilo de vida, a WKND. O tutorial aborda tópicos fundamentais como configuração de projeto, arquétipos de modelos maven, Componentes principais, Modelos editáveis, bibliotecas de clientes e desenvolvimento de componentes.
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
translation-type: tm+mt
source-git-commit: 76462bb75ceda1921db2fa37606ed7c5a1eadb81
workflow-type: tm+mt
source-wordcount: '893'
ht-degree: 13%

---


# Introdução ao AEM Sites - Tutorial do WKND {#introduction}

Bem-vindo a um tutorial de várias partes projetado para desenvolvedores novos no Adobe Experience Manager (AEM). Este tutorial aborda a implementação de um site AEM para uma marca de estilo de vida fictício na WKND. O tutorial aborda tópicos fundamentais como configuração de projeto, Componentes principais, Modelos editáveis, bibliotecas do lado do cliente e desenvolvimento de componentes com a Adobe Experience Manager Sites.

## Visão geral {#wknd-tutorial-overview}

A meta para este tutorial de várias partes é ensinar um desenvolvedor a implementar um site usando os mais recentes padrões e tecnologias da Adobe Experience Manager (AEM). Após concluir este tutorial, um desenvolvedor deve entender a base básica da plataforma e com conhecimento de padrões de design comuns em AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

O tutorial foi projetado para funcionar com **AEM como um Cloud Service** e é compatível com versões anteriores com **AEM 6.5.5.0+** e **AEM 6.4.8.1+**. O site é implementado usando:

* [Arquétipo de Projeto Maven AEM](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/developing/archetype/overview.html)
* [Componentes principais](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Modelos Sling
* [Modelos editáveis](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Sistema de estilos](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Estime de 1 a 2 horas para percorrer cada parte do tutorial.*

## Ambiente de desenvolvimento local {#local-dev-environment}

É necessário um ambiente de desenvolvimento local para concluir este tutorial. Capturas de tela e vídeo são capturados usando o AEM como um SDK Cloud Service em execução em um ambiente Mac OS com [Código do Visual Studio](https://code.visualstudio.com/) como o IDE. Os comandos e o código devem ser independentes do sistema operacional local, a menos que seja observado o contrário.

### Software necessário

Devem ser instalados:

* Instância Local AEM **Author** (Cloud Service SDK, 6.5.5+ ou 6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou mais recente)
* [Node.js](https://nodejs.org/en/) (LTS - Suporte a longo prazo)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio ](https://code.visualstudio.com/) Codeor equivalente IDE
   * [Sincronização](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  AEM VSCode - Ferramenta usada em todo o tutorial

>[!NOTE]
>
> **Novo no AEM as a Cloud Service?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Novo no AEM 6.5?** Consulte o guia a  [seguir para configurar um ambiente](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) de desenvolvimento local.

## Sobre o tutorial {#about-tutorial}

A WKND é uma revista e um blog fictícios online que foca na vida noturna, atividades e eventos em várias cidades internacionais.

### Kit de interface do usuário Adobe XD

Para tornar este tutorial mais próximo de um cenário real, os designers UX talentosos criaram modelos para o site usando [Adobe XD](https://www.adobe.com/products/xd.html). Durante o curso do tutorial, várias partes dos designs são implementadas em um site de AEM totalmente criável. Agradecimentos especiais a **Lorenzo Buosi** e **Kilian Emenda** que criaram o belo design para o site da WKND.

Baixe os kits da interface do usuário XD:

* [Kit da interface do usuário do componente principal do AEM](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit de interface do usuário WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

O nome WKND é adequado, pois esperamos que um desenvolvedor aproveite melhor a parte de um ***fim de semana*** para concluir o tutorial.

### Github {#github}

Todo o código do projeto está disponível no Github no AEM Guide repo:

**[GitHub: Projeto de Sites WKND](https://github.com/adobe/aem-guides-wknd)**

Além disso, cada parte do tutorial tem seu próprio ramo no GitHub. Um usuário pode começar o tutorial a qualquer momento, simplesmente verificando a ramificação que corresponde à parte anterior.

>[!NOTE]
>
> Se você estava trabalhando com a versão anterior deste tutorial, ainda é possível encontrar os [pacotes de solução](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1) e [código](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1) no GitHub.

## Site de referência {#reference-site}

Uma versão concluída do Site WKND também está disponível como referência: [https://wknd.site/](https://wknd.site/)

O tutorial aborda as principais habilidades de desenvolvimento necessárias para um desenvolvedor de AEM, mas *não* criará o site inteiro de ponta a ponta. O site de referência finalizado é outro grande recurso para explorar e ver mais recursos AEM prontos para uso.

Para testar o código mais recente antes de ir para o tutorial, baixe e instale a versão mais recente do **[GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Alimentado pela Adobe Stock

Muitas das imagens no site de referência WKND são de [Adobe Stock](https://stock.adobe.com/) e são de Materiais de terceiros, conforme definido nos Termos adicionais de ativos de demonstração em [https://www.adobe.com/legal/terms.html](https://www.adobe.com/legal/terms.html). Se você quiser usar uma imagem da Adobe Stock para outros fins além da exibição deste site de demonstração, como apresentá-la em um site ou em materiais de marketing, você pode adquirir uma licença na Adobe Stock.

Com o Adobe Stock, você tem acesso a mais de 140 milhões de imagens de alta qualidade e isentas de royalties, incluindo fotos, gráficos, vídeos e modelos para começar seus projetos criativos.

## Próximas etapas {#next-steps}

O que você está esperando?! Start o tutorial navegando até o capítulo [Configuração do projeto](project-setup.md) e aprenda a gerar um novo projeto Adobe Experience Manager usando o AEM Project Archetype.
