---
title: Capítulo 6 - Exposição do conteúdo na publicação do AEM como JSON - Content Services
description: O capítulo 6 do tutorial do AEM headless abrange garantir que todos os pacotes, a configuração e o conteúdo necessários estejam no AEM Publish para permitir o consumo do aplicativo móvel.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# Capítulo 6 - Expor o conteúdo na publicação do AEM para entrega

O capítulo 6 do tutorial do AEM headless abrange garantir que todos os pacotes, a configuração e o conteúdo necessários estejam no AEM Publish para permitir o consumo pelo aplicativo móvel.

## Publicação de conteúdo para o AEM Content Services

A configuração e o conteúdo criados para orientar os eventos por meio do AEM Content Services devem ser publicados para AEM Publish, para que o aplicativo móvel possa acessá-los.

Como os Serviços de conteúdo AEM são criados a partir de Configuração (modelos de fragmento de conteúdo, modelos editáveis), Ativos (fragmentos de conteúdo, imagens) e Páginas, todas essas partes desfrutam automaticamente dos recursos de gerenciamento de conteúdo do AEM, incluindo:

* Fluxo de trabalho para revisão e processamento
* e ativação/desativação para enviar e receber conteúdo dos pontos de acesso do AEM Content Services do AEM Publish do

1. Assegure a **[!DNL WKND Mobile]Pacotes de aplicativos**, listados em [Capítulo 1](./chapter-1.md#wknd-mobile-application-packages), são instalados em **Publicação no AEM** usar [!UICONTROL Gerenciador de pacotes].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publicar o **[!DNL WKND Mobile Events API]Modelo editável**
   1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Ferramentas] > [!UICONTROL Geral] > [!UICONTROL Modelos] >[!DNL WKND Mobile]**
   1. Selecione o **[!DNL Event API]** modelo
   1. Toque **[!UICONTROL Publish]** na barra de ação superior
   1. Publicar o **modelo** e **todas as referências** (políticas de conteúdo, mapeamentos de políticas de conteúdo e modelos)

1. Publicar o **[!DNL WKND Mobile Events]fragmentos de conteúdo**.

   Observe que isso é necessário, pois a API de eventos usa o componente Lista de fragmentos de conteúdo, que não faz referência específica aos fragmentos de conteúdo.

   1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Arquivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. Selecione todas as **[!DNL Event]** fragmentos de conteúdo
   1. Toque no **[!UICONTROL Gerenciar publicação]** na barra de ação superior
   1. Deixando o padrão **Publish** ação como está, toque em **[!UICONTROL Próxima]** na barra de ação superior
   1. Selecionar **all** fragmentos de conteúdo
   1. Toque **[!UICONTROL Publish]** na barra de ação superior
      * *A variável [!DNL Events] O modelo de fragmento de conteúdo e as imagens de evento de referência serão publicados automaticamente junto com os fragmentos de conteúdo.*

1. Publicar o **[!DNL Events API]página**.
   1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Selecione o **[!DNL Events]** página
   1. Toque no **[!UICONTROL Gerenciar publicação]** na barra de ação superior
   1. Deixando o padrão **Publish** ação como está, toque em **[!UICONTROL Próxima]** na barra de ação superior
   1. Selecione o **[!DNL Events]** página
   1. Toque **[!DNL Publish]** na barra de ação superior

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## Verificar publicação do AEM

1. Em um novo navegador da Web, verifique se você está desconectado do AEM Publish e solicite os seguintes URLs (substituindo `http://localhost:4503` para qualquer host:porta em que o AEM Publish estiver sendo executado).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)

   Essas solicitações devem retornar a mesma resposta JSON de quando os pontos de extremidade do autor do AEM correspondentes foram revisados. Caso contrário, verifique se todas as publicações tiveram êxito (verifique as filas de Replicação), a variável [!DNL WKND Mobile] `ui.apps` for instalado no AEM Publicar e revise as `error.log` para AEM Publish.

## Próxima etapa

Não há pacotes extras a serem instalados. Certifique-se de que o conteúdo e a configuração descritos nesta seção sejam publicados no AEM Publish, caso contrário, os capítulos subsequentes não funcionarão.

* [Capítulo 7 - Consumir serviços de conteúdo de AEM de um aplicativo móvel](./chapter-7.md)
