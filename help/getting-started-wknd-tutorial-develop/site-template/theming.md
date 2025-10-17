---
title: Fluxo de trabalho de temas | Criação rápida de sites do AEM
description: Saiba como atualizar as fontes do tema de um site do Adobe Experience Manager para aplicar estilos específicos da marca. Saiba como usar um servidor de proxy para ver uma prévia em tempo real das atualizações do CSS e do Javascript. Este tutorial também abordará como implantar atualizações de temas em um site do AEM por meio do pipeline de front-end do Adobe Cloud Manager.
version: Experience Manager as a Cloud Service
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
recommendations: noDisplay, noCatalog
duration: 1275
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '458'
ht-degree: 100%

---

# Fluxo de trabalho de temas {#theming}

Neste capítulo, vamos atualizar as fontes do tema de um site do Adobe Experience Manager para aplicar estilos específicos da marca. Vamos aprender a usar um servidor de proxy para visualizar as atualizações do CSS e do JavaScript ao codificarmos para o site ativo. Este tutorial também abordará como implantar atualizações de temas em um site do AEM por meio do pipeline de front-end do Adobe Cloud Manager.

No fim, o nosso site será atualizado para incluir estilos que correspondam à marca da WKND.

## Pré-requisitos {#prerequisites}

Este é um tutorial em várias partes, e presume-se que as etapas descritas no capítulo [Modelos de página](./page-templates.md) tenham sido concluídas.

## Objetivos

1. Aprender como as fontes do tema de um site podem ser baixadas e modificadas.
1. Aprender a codificar para o site ativo e exibir uma visualização em tempo real.
1. Entender o fluxo de trabalho de ponta a ponta de entrega de atualizações do CSS e do JavaScript compiladas como parte de um tema por meio do pipeline de front-end do Adobe Cloud Manager.

## Atualizar um tema {#theme-update}

Em seguida, faça alterações nas fontes do tema, de modo que o site corresponda à aparência da marca da WKND.

>[!VIDEO](https://video.tv.adobe.com/v/332918?quality=12&learn=on)

Etapas de alto nível do vídeo:

1. Crie um usuário local no AEM para usar com um servidor de desenvolvimento de proxy.
1. Baixe as fontes do tema do AEM e abra-as com um IDE local, como o VSCode.
1. Modifique as fontes do tema e use um servidor de desenvolvimento de proxy para visualizar as alterações do CSS e do JavaScript em tempo real.
1. Atualize as fontes do tema, de modo que o artigo de revista corresponda aos estilos e modelos da marca da WKND.

### Arquivos de solução

Baixe os estilos completos do [Tema de amostra da WKND](assets/theming/WKND-THEME-src-1.1.zip)

## Implantar um tema com um pipeline de front-end {#deploy-theme}

Implante as atualizações de um tema em um ambiente do AEM com o pipeline de front-end do Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

Etapas de alto nível do vídeo:

1. Criar um novo [repositório do Git no Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)
1. Adicione o projeto de fontes do tema ao repositório do Git do Cloud Manager:

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. Configure um [pipeline de front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html) no Cloud Manager para implantar o código de front-end.
1. Execute o pipeline de front-end para implantar atualizações no ambiente do AEM de destino.

### Exemplos de repositório

Há alguns exemplos de repositório do GitHub que podem ser usados como referência:

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e): usado como exemplo para projetos do “mundo real”.

## Parabéns! {#congratulations}

Parabéns, você acabou de atualizar e implantar um tema no AEM!

### Próximas etapas {#next-steps}

Saiba mais sobre o desenvolvimento no AEM e entenda mais sobre a tecnologia subjacente, criando um site com o [Arquétipo de projeto do AEM](../project-archetype/overview.md).
