---
title: Criação de fragmentos de conteúdo em AEM
description: 'Fragmentos de conteúdo são uma abstração de conteúdo em AEM que permite que o conteúdo baseado em texto seja criado e gerenciado independentemente dos canais suportados. '
sub-product: serviços de conteúdo
feature: content-fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: cloud-service
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 6%

---


# Criação de fragmentos de conteúdo {#authoring-content-fragments}

Fragmentos de conteúdo são uma abstração de conteúdo em AEM que permite que o conteúdo baseado em texto seja criado e gerenciado independentemente dos canais suportados.

AEM Fragmentos de conteúdo são conteúdos editoriais baseados em texto que podem incluir alguns elementos de dados estruturados associados, mas considerados conteúdo puro sem informações de design ou layout. Os Fragmentos de conteúdo normalmente são criados como conteúdo agnóstico do canal, que deve ser usado e reutilizado em canais, o que, por sua vez, envolve o conteúdo em uma experiência específica do contexto.

Esta série de vídeos cobre o ciclo de vida de criação de Fragmentos de conteúdo em AEM. Detalhes sobre [como fornecer Fragmentos de conteúdo podem ser encontrados aqui](content-fragments-delivery-feature-video-use.md).

1. Ativação e definição de modelos de fragmento de conteúdo
2. Criação de fragmentos de conteúdo
3. Download de fragmentos de conteúdo
4. Recursos editoriais

## Definindo Modelos de Fragmento de Conteúdo {#defining-content-fragment-models}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

AEM Modelos de fragmentos de conteúdo, os schemas de dados de Fragmentos de conteúdo, devem ser ativados por meio AEM [[!UICONTROL Navegador de configuração]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html), que permite que os Modelos de fragmento de conteúdo sejam definidos com base em cada configuração.

## Criação de fragmentos de conteúdo {#creating-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

AEM configurações são aplicadas às hierarquias de pastas do AEM Assets para permitir que seus Modelos de fragmento de conteúdo sejam criados como Fragmentos de conteúdo. Fragmentos de conteúdo oferecem suporte a uma experiência de criação baseada em formulários avançada que permite que o conteúdo seja modelado como uma coleção de elementos.

Fragmentos de conteúdo podem ter várias variantes, cada variante tratando de um caso de uso diferente (pensamento, não necessariamente canal) para o conteúdo.

*Exemplo de biografia de atleta para importação:*\
**[sandra-sprient-bio.txt](assets/sandra-sprient-bio.txt)**

## Download de fragmentos de conteúdo {#downloading-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22450/?quality=12&learn=on)

AEM Fragmentos de conteúdo podem ser baixados do autor de AEM como um arquivo Zip contendo variáveis, elementos e metadados.

*Exemplo de arquivo Zip de download do fragmento de conteúdo:*\
**[daniel_schreder.zip](assets/daniel_schreder.zip)**

## Recursos editoriais do Fragmento de conteúdo {#editorial-capabilities}

>[!VIDEO](https://video.tv.adobe.com/v/25891/?quality=12&learn=on)

>[!NOTE]
>
> A comparação de anotação e versão para Fragmentos de conteúdo foi introduzida em [AEM 6.4 Service Pack 2](https://helpx.adobe.com/br/experience-manager/aem-releases-updates.html) e [AEM 6.3 Service Pack 3](https://helpx.adobe.com/experience-manager/6-3/release-notes/sp3-release-notes.html).

## Próximas etapas

Saiba mais sobre [como fornecer Fragmentos de conteúdo](content-fragments-delivery-feature-video-use.md).

## Recursos adicionais {#additional-resources}

* [Fornecer fragmentos de conteúdo](content-fragments-delivery-feature-video-use.md)
* [Componentes principais do AEM WCM](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/introduction.html)
* [Componente de fragmento de conteúdo principal do AEM WCM](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/components/content-fragment-component.html)

Para baixar e instalar o pacote abaixo em uma instância AEM 6.4+ para o estado final da série de vídeo:

**[aem_demo_fluidescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
