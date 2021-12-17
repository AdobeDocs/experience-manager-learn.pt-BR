---
title: Fluxo de trabalho de temas | Criação rápida de AEM
description: Saiba como atualizar as fontes de tema de um site do Adobe Experience Manager para aplicar estilos específicos de marca. Saiba como usar um servidor proxy para exibir uma pré-visualização ao vivo das atualizações de CSS e Javascript. Este tutorial também abordará como implantar atualizações de temas em um site AEM usando o pipeline front-end do Adobe Cloud Manager.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
source-git-commit: 0225b7f2e495d5c020ea5192302691e3466808ed
workflow-type: tm+mt
source-wordcount: '484'
ht-degree: 0%

---

# Fluxo de trabalho de temas {#theming}

Neste capítulo, atualizaremos as fontes de tema de um site do Adobe Experience Manager para aplicar estilos específicos da marca. Nós aprenderemos a usar um servidor proxy para exibir uma pré-visualização das atualizações de CSS e Javascript enquanto codificamos no site ativo. Este tutorial também abordará como implantar atualizações de temas em um site AEM usando o pipeline front-end do Adobe Cloud Manager.

No final, nosso site será atualizado para incluir estilos para corresponder à marca WKND.

## Pré-requisitos {#prerequisites}

Este é um tutorial de várias partes e presume-se que as etapas descritas na seção [Modelos de página](./page-templates.md) capítulo foi concluído.

## Objetivos

1. Saiba como as fontes de tema de um site podem ser baixadas e modificadas.
1. Saiba mais sobre o código do site ativo para uma visualização em tempo real.
1. Entenda o fluxo de trabalho completo do fornecimento de atualizações de CSS e JavaScript compiladas como parte de um tema usando o pipeline front-end do Adobe Cloud Manager.

## Atualizar um tema {#theme-update}

Em seguida, faça alterações nas fontes de tema para que o site corresponda à aparência da Marca WKND.

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

Etapas de alto nível para o vídeo:

1. Crie um usuário local no AEM para uso com um servidor de desenvolvimento de proxy.
1. Baixe as fontes de tema do AEM e abra usando um IDE local, como VSCode.
1. Modifique as fontes de temas e use um servidor dev de proxy para visualizar as alterações de CSS e JavaScript em tempo real.
1. Atualize as fontes de temas para que o artigo da revista corresponda aos estilos e modelos da marca WKND.

### Arquivos de solução

Baixe os estilos concluídos para a [Tema de amostra WKND](assets/theming/WKND-THEME-src-1.1.zip)

## Implantar um tema usando um pipeline de front-end {#deploy-theme}

Implante atualizações em um tema em um ambiente de AEM usando o pipeline front-end do Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/338722/?quality=12&learn=on)

Etapas de alto nível para o vídeo:

1. Criar um novo git [repositório no Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)
1. Adicione seu projeto de fontes de temas ao repositório Git do Cloud Manager:

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. Configure um [Pipeline de front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html) no Cloud Manager para implantar o código de front-end.
1. Execute o pipeline de front-end para implantar atualizações no ambiente de AEM de destino.

### Exemplo de acordos de recompra

Há alguns exemplos de acordos de recompra do GitHub que podem ser usados como referência:

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) - Utilizado como exemplo para projetos do &quot;mundo real&quot;.

## Parabéns! {#congratulations}

Parabéns, você acabou de criar e atualizou um tema para AEM!

### Próximas etapas {#next-steps}

Faça um mergulho mais profundo no desenvolvimento AEM e entenda mais a tecnologia subjacente criando um site usando o [Arquétipo de projeto AEM](../project-archetype/overview.md).
