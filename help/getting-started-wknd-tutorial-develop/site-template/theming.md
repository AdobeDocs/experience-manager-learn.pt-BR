---
title: Fluxo de trabalho de tema | Criação rápida de sites no AEM
description: Saiba como atualizar as fontes de tema de um site do Adobe Experience Manager para aplicar estilos específicos da marca. Saiba como usar um servidor proxy para exibir uma visualização ao vivo das atualizações de CSS e Javascript. Este tutorial também abordará como implantar atualizações de temas em um site AEM usando o pipeline de front-end do Adobe Cloud Manager.
version: Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '458'
ht-degree: 1%

---

# Fluxo de trabalho de tema {#theming}

Neste capítulo, atualizamos as fontes de tema de um site do Adobe Experience Manager para aplicar estilos específicos da marca. Aprendemos a usar um servidor proxy para visualizar as atualizações de CSS e JavaScript ao codificarmos para o site ativo. Este tutorial também abordará como implantar atualizações de temas em um site AEM usando o pipeline de front-end do Adobe Cloud Manager.

No final, nosso site é atualizado para incluir estilos que correspondam à marca WKND.

## Pré-requisitos {#prerequisites}

Este é um tutorial em várias partes e presume-se que as etapas descritas no [Modelos de página](./page-templates.md) capítulo foram concluídas.

## Objetivos

1. Saiba como as fontes de tema de um site podem ser baixadas e modificadas.
1. Saiba como codificar no site ativo para uma visualização em tempo real.
1. Entenda o fluxo de trabalho completo de entrega de atualizações de CSS e JavaScript compiladas como parte de um tema usando o pipeline de front-end do Adobe Cloud Manager.

## Atualizar um tema {#theme-update}

Em seguida, faça alterações nas fontes de tema para que o site corresponda à aparência da Marca WKND.

>[!VIDEO](https://video.tv.adobe.com/v/332918?quality=12&learn=on)

Etapas de alto nível para o vídeo:

1. Criar um usuário local no AEM para uso com um servidor de desenvolvimento proxy.
1. Baixe as fontes de tema do AEM e abra o usando um IDE local, como o VSCode.
1. Modifique as fontes de tema e use um servidor proxy dev para visualizar as alterações de CSS e JavaScript em tempo real.
1. Atualize as fontes de tema para que o artigo da revista corresponda aos estilos e modelos da marca WKND.

### Arquivos de solução

Baixe os estilos concluídos para o [Tema de amostra do WKND](assets/theming/WKND-THEME-src-1.1.zip)

## Implantar um tema usando um pipeline de front-end {#deploy-theme}

Implante atualizações em um tema para um ambiente AEM usando o pipeline de front-end do Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

Etapas de alto nível para o vídeo:

1. Criar um novo Git [repositório no Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)
1. Adicione o projeto de fontes de tema ao repositório Git do Cloud Manager:

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. Configurar um [Pipeline de front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html) no Cloud Manager para implantar o código de front-end.
1. Execute o pipeline de front-end para implantar atualizações no ambiente AEM de destino.

### Exemplo de acordos de recompra

Há alguns exemplos de repositórios GitHub que podem ser usados como referência:

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) - Usado como exemplo para projetos &quot;reais&quot;.

## Parabéns. {#congratulations}

Parabéns, você acabou de atualizar e implantar um tema para AEM!

### Próximas etapas {#next-steps}

Aprofunde-se no desenvolvimento do AEM e entenda mais sobre a tecnologia subjacente criando um site usando o [Arquétipo de projeto AEM](../project-archetype/overview.md).
