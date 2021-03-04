---
title: Introdução ao AEM Sites - Tutorial do WKND
description: Introdução ao AEM Sites - Tutorial do WKND. O tutorial WKND é um tutorial em várias partes projetado para desenvolvedores novos no Adobe Experience Manager. O tutorial aborda a implementação de um site AEM para uma marca fictícia de estilo de vida, a WKND. O tutorial aborda tópicos fundamentais como configuração de projeto, arquétipos de maven, Componentes principais, Modelos editáveis, bibliotecas de clientes e desenvolvimento de componentes.
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
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '893'
ht-degree: 14%

---


# Introdução ao AEM Sites - Tutorial do WKND {#introduction}

Bem-vindo a um tutorial de várias partes projetado para desenvolvedores novos no Adobe Experience Manager (AEM). Este tutorial aborda a implementação de um site AEM para uma marca fictícia de estilo de vida na WKND. O tutorial aborda tópicos fundamentais como configuração do projeto, Componentes principais, Modelos editáveis, bibliotecas do lado do cliente e desenvolvimento de componentes com o Adobe Experience Manager Sites.

## Visão geral {#wknd-tutorial-overview}

O objetivo deste tutorial em várias partes é ensinar um desenvolvedor a implementar um site usando os padrões e as tecnologias mais recentes no Adobe Experience Manager (AEM). Após concluir este tutorial, um desenvolvedor deve entender a base básica da plataforma e com conhecimento de padrões de design comuns no AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

O tutorial foi projetado para funcionar com o **AEM as a Cloud Service** e é compatível com versões anteriores do **AEM 6.5.5.0+** e **AEM 6.4.8.1+**. O site é implementado usando:

* [Arquétipo de projeto do AEM Maven](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/developing/archetype/overview.html)
* [Componentes principais](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Modelos sling
* [Modelos editáveis](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Sistema de estilos](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Estime de 1 a 2 horas para visualizar cada parte do tutorial.*

## Ambiente de desenvolvimento local {#local-dev-environment}

Um ambiente de desenvolvimento local é necessário para concluir este tutorial. Capturas de tela e vídeo são capturados usando o SDK do AEM as a Cloud Service em execução em um ambiente Mac OS com o [Visual Studio Code](https://code.visualstudio.com/) como IDE. Os comandos e o código devem ser independentes do sistema operacional local, salvo indicação em contrário.

### Software necessário

Devem ser instalados:

* Instância local do AEM **Author** (SDK do Cloud Service, 6.5.5+ ou 6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou mais recente)
* [Node.js](https://nodejs.org/en/)  (LTS - Suporte a longo prazo)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio ](https://code.visualstudio.com/) Code ou IDE equivalente
   * [Sincronização AEM VSCode](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  - Ferramenta usada em todo o tutorial

>[!NOTE]
>
> **Novo no AEM as a Cloud Service?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Novo no AEM 6.5?** Consulte o guia a  [seguir para configurar um ambiente](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) de desenvolvimento local.

## Sobre o tutorial {#about-tutorial}

A WKND é uma revista e um blog fictício online que se concentra na vida noturna, nas atividades e nos eventos em várias cidades internacionais.

### Kit da interface do usuário do Adobe XD

Para tornar este tutorial mais próximo de um cenário real, os talentosos designers UX da Adobe criaram os modelos para o site usando [Adobe XD](https://www.adobe.com/products/xd.html). Ao longo do tutorial, várias partes dos designs são implementadas em um site AEM totalmente criável. Agradecimentos especiais a **Lorenzo Buosi** e **Kilian Emenda** que criaram o belo design para o site WKND.

Baixe os kits da interface do usuário XD:

* [Kit da interface do usuário do componente principal do AEM](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit da interface do usuário do WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

O nome WKND está configurado, pois esperamos que um desenvolvedor faça a melhor parte de um ***fim de semana*** para concluir o tutorial.

### Github {#github}

Todo o código para o projeto pode ser encontrado no Github no repositório do AEM Guide:

**[GitHub: Projeto de Sites WKND](https://github.com/adobe/aem-guides-wknd)**

Além disso, cada parte do tutorial tem sua própria ramificação no GitHub. Um usuário pode iniciar o tutorial a qualquer momento, fazendo o check-out da ramificação que corresponde à parte anterior.

>[!NOTE]
>
> Se estava trabalhando com a versão anterior deste tutorial, ainda é possível encontrar os [pacotes de solução](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1) e [código](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1) no GitHub.

## Site de referência {#reference-site}

Uma versão concluída do Site WKND também está disponível como referência: [https://wknd.site/](https://wknd.site/)

O tutorial aborda as principais habilidades de desenvolvimento necessárias para um desenvolvedor do AEM, mas *não* criará todo o site de ponta a ponta. O site de referência finalizado é outro excelente recurso para explorar e ver mais recursos prontos para uso do AEM.

Para testar o código mais recente antes de passar para o tutorial, baixe e instale a versão mais recente de **[do GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Alimentado pelo Adobe Stock

Muitas das imagens no site de Referência da WKND são do [Adobe Stock](https://stock.adobe.com/) e são Materiais de terceiros, conforme definido nos Termos adicionais do ativo de demonstração em [https://www.adobe.com/legal/terms.html](https://www.adobe.com/br/legal/terms.html). Se você quiser usar uma imagem do Adobe Stock para outros propósitos além da exibição deste site de demonstração, como exibi-la em um site ou em materiais de marketing, poderá comprar uma licença no Adobe Stock.

Com o Adobe Stock, você tem acesso a mais de 140 milhões de imagens de alta qualidade e isentas de royalties, incluindo fotos, gráficos, vídeos e modelos para começar seus projetos criativos.

## Próximas etapas {#next-steps}

O que você está esperando?! Inicie o tutorial navegando até o capítulo [Configuração do projeto](project-setup.md) e saiba como gerar um novo projeto do Adobe Experience Manager usando o Arquétipo de projeto AEM.
