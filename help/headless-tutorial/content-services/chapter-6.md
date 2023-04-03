---
title: Capítulo 6 - Exposição do conteúdo na publicação do AEM como JSON - Serviços de conteúdo
description: O Capítulo 6 do tutorial AEM Headless cobre a garantia de que todos os pacotes, configurações e conteúdo necessários estão no AEM Publish para permitir o consumo do aplicativo móvel.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# Capítulo 6 - Exposição do conteúdo na publicação do AEM para entrega

O Capítulo 6 do tutorial AEM Headless cobre a garantia de que todos os pacotes, configurações e conteúdo necessários estão no AEM Publish para permitir o consumo pelo aplicativo móvel.

## Publicar o conteúdo dos serviços de conteúdo AEM

A configuração e o conteúdo criados para orientar os Eventos por meio AEM Content Services devem ser publicados na AEM Publish para que o Aplicativo móvel possa acessá-los.

Como os AEM Content Services são criados a partir de Configuração (Modelos de fragmentos de conteúdo, Modelos editáveis), Ativos (Fragmentos de conteúdo, Imagens) e Páginas, todas essas partes desfrutam automaticamente dos recursos de gerenciamento de conteúdo AEM, incluindo:

* Fluxo de trabalho para análise e processamento
* e ativação/desativação para envio e extração de conteúdo dos pontos finais dos serviços de conteúdo AEM do AEM Publish

1. Verifique se a **[!DNL WKND Mobile]Pacotes de aplicativos** listado em [Capítulo 1](./chapter-1.md#wknd-mobile-application-packages)são instalados em **Publicação do AEM** usar [!UICONTROL Gerenciador de pacotes].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publique o **[!DNL WKND Mobile Events API]Modelo editável**
   1. Navegar para **[!UICONTROL AEM] > [!UICONTROL Ferramentas] > [!UICONTROL Geral] > [!UICONTROL Modelos] >[!DNL WKND Mobile]**
   1. Selecione o **[!DNL Event API]** modelo
   1. Toque **[!UICONTROL Publicar]** na barra de ação superior
   1. Publique o **modelo** e **todas as referências** (políticas de conteúdo, mapeamentos de política de conteúdo e modelos)

1. Publique o **[!DNL WKND Mobile Events]fragmentos de conteúdo**.

   Observe que isso é necessário, pois a API de eventos usa o componente Lista de fragmentos do conteúdo , que não faz referência específica aos Fragmentos do conteúdo.

   1. Navegar para **[!UICONTROL AEM] > [!UICONTROL Ativos] > [!UICONTROL Arquivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. Selecione todas as **[!DNL Event]** fragmentos de conteúdo
   1. Toque no **[!UICONTROL Gerenciar publicação]** na barra de ação superior
   1. Deixando o padrão **Publicar** ação como está, toque **[!UICONTROL Próximo]** na barra de ação superior
   1. Selecionar **all** fragmentos de conteúdo
   1. Toque **[!UICONTROL Publicar]** na barra de ação superior
      * *O [!DNL Events] O Modelo de fragmento de conteúdo e as referências às Imagens do evento serão publicados automaticamente junto com os fragmentos de conteúdo.*

1. Publique o **[!DNL Events API]página**.
   1. Navegar para **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Selecione o **[!DNL Events]** página
   1. Toque no **[!UICONTROL Gerenciar publicação]** na barra de ação superior
   1. Deixando o padrão **Publicar** ação como está, toque **[!UICONTROL Próximo]** na barra de ação superior
   1. Selecione o **[!DNL Events]** página
   1. Toque **[!DNL Publish]** na barra de ação superior

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## Verificar a publicação do AEM

1. Em um novo navegador da Web, verifique se você está desconectado da publicação do AEM e solicite os seguintes URLs (substituindo `http://localhost:4503` para qualquer host:porta em que o AEM Publish está sendo executado).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Essas solicitações devem retornar a mesma resposta JSON de quando os pontos finais correspondentes do Autor do AEM foram revisados. Caso contrário, verifique se todas as publicações tiveram êxito (verifique as filas de Replicação), e [!DNL WKND Mobile] `ui.apps` O pacote é instalado no AEM Publish e revise o `error.log` para publicação do AEM.

## Próxima etapa

Não há pacotes extras para instalar. Certifique-se de que o conteúdo e a configuração descritos nesta seção sejam publicados na Publicação do AEM; caso contrário, os capítulos subsequentes não funcionarão.

* [Capítulo 7 - Consumo AEM Content Services a partir de um aplicativo móvel](./chapter-7.md)
