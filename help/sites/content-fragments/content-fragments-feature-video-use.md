---
title: Criação de fragmentos de conteúdo no AEM
description: Fragmentos de conteúdo são uma abstração de conteúdo no AEM que permite que o conteúdo baseado em texto seja criado e gerenciado independentemente dos canais compatíveis.
sub-product: content-services
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: Cloud Service
topic: Content Management
role: User
level: Beginner
exl-id: d33c033a-9577-4d4e-99be-f3c7e2a4ce73
source-git-commit: 9aae58c3301a7067baca374d6499f1afc3c95b06
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 9%

---

# Criação de fragmentos de conteúdo {#authoring-content-fragments}

Fragmentos de conteúdo são uma abstração de conteúdo no AEM que permite que o conteúdo baseado em texto seja criado e gerenciado independentemente dos canais compatíveis.

AEM Fragmentos de conteúdo são conteúdos editoriais baseados em texto que podem incluir alguns elementos de dados estruturados associados, mas considerados conteúdo puro sem informações de design ou layout. Os Fragmentos de conteúdo geralmente são criados como conteúdo independente de canal, que deve ser usado e reutilizado em canais, o que, por sua vez, envolve o conteúdo em uma experiência específica de contexto.

Esta série de vídeo cobre o ciclo de vida de criação dos Fragmentos de conteúdo no AEM. Detalhes sobre [O delivery de Fragmentos de conteúdo pode ser encontrado aqui](content-fragments-delivery-feature-video-use.md).

1. Ativar e definir modelos de fragmento de conteúdo
2. Criação de fragmentos de conteúdo
3. Download de fragmentos de conteúdo
4. Recursos editoriais

>[!CONTEXTUALHELP]
>id="aemcloud_sites_admin_content_fragments"
>title="Gerenciar fragmentos"
>abstract="Saiba como os Fragmentos de conteúdo permitem projetar, criar, preparar e usar conteúdo independente da página."

## Definição dos modelos de fragmento do conteúdo {#defining-content-fragment-models}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

AEM Modelos de fragmentos de conteúdo, os esquemas de dados dos Fragmentos de conteúdo, devem ser ativados por meio de AEM [[!UICONTROL Navegador de configuração]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=pt-BR), que permite que os Modelos de fragmento de conteúdo sejam definidos com base na configuração.

## Criação de fragmentos de conteúdo {#creating-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

AEM configurações são aplicadas às hierarquias da pasta AEM Assets para permitir que seus Modelos de fragmento de conteúdo sejam criados como Fragmentos de conteúdo. Os Fragmentos de conteúdo são compatíveis com uma experiência de criação baseada em formulário avançada, permitindo que o conteúdo seja modelado como uma coleção de elementos.

Os Fragmentos de conteúdo podem ter várias variantes, cada variante abordando um caso de uso diferente (pensamento, não necessariamente canal) para o conteúdo.

*Exemplo de biografia de atleta para importação:*\
**[sandra-sprient-bio.txt](assets/sandra-sprient-bio.txt)**

## Download de fragmentos de conteúdo {#downloading-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22450/?quality=12&learn=on)

AEM Fragmentos de conteúdo podem ser baixados do autor do AEM como um arquivo Zip contendo Variantes, Elementos e Metadados.

*Exemplo de arquivo Zip de download do Fragmento de conteúdo:*\
**[daniel_schreder.zip](assets/daniel_schreder.zip)**

## Recursos editoriais do Fragmento de conteúdo {#editorial-capabilities}

>[!VIDEO](https://video.tv.adobe.com/v/25891/?quality=12&learn=on)

>[!NOTE]
>
> A anotação e a comparação de versão para Fragmentos de conteúdo foram introduzidas em [AEM 6.4 Service Pack 2](https://helpx.adobe.com/br/experience-manager/aem-releases-updates.html) e [AEM 6.3 Service Pack 3](https://helpx.adobe.com/br/experience-manager/6-3/release-notes/sp3-release-notes.html).

## Próximas etapas

Saiba mais sobre [Fornecimento de fragmentos de conteúdo](content-fragments-delivery-feature-video-use.md).

## Recursos adicionais {#additional-resources}

* [Entrega de fragmentos de conteúdo](content-fragments-delivery-feature-video-use.md)
* [Componentes principais do WCM AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR)
* [Componente do Fragmento de conteúdo principal do WCM AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=pt-BR)

Para baixar e instalar o pacote abaixo em uma instância do AEM 6.4+ para o estado final da série de vídeos:

**[aem_demo_fluidoexperiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
