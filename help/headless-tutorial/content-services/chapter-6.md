---
title: Capítulo 6 - Exposição do conteúdo na publicação do AEM como JSON - Serviços de conteúdo
description: O Capítulo 6 do tutorial AEM Headless cobre a garantia de que todos os pacotes, configurações e conteúdo necessários estão no AEM Publish para permitir o consumo do aplicativo móvel.
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---


# Capítulo 6 - Exposição do conteúdo na publicação do AEM para entrega

O Capítulo 6 do tutorial AEM Headless cobre a garantia de que todos os pacotes, configurações e conteúdo necessários estão no AEM Publish para permitir o consumo pelo aplicativo móvel.

## Publicar o conteúdo dos serviços de conteúdo do AEM

A configuração e o conteúdo criados para direcionar os Eventos por meio do AEM Content Services devem ser publicados na Publicação do AEM para que o aplicativo móvel possa acessá-los.

Como os AEM Content Services são criados a partir de Configuração (Modelos de fragmento de conteúdo, Modelos editáveis), Ativos (Fragmentos de conteúdo, Imagens) e Páginas, todas essas partes desfrutam automaticamente dos recursos de gerenciamento de conteúdo do AEM, incluindo:

* Fluxo de trabalho para análise e processamento
* e ativação/desativação para envio e extração de conteúdo dos pontos finais do AEM Publish

1. Verifique se os **[!DNL WKND Mobile]Pacotes de Aplicativo**, listados em [Capítulo 1](./chapter-1.md#wknd-mobile-application-packages), estão instalados em **AEM Publish** usando [!UICONTROL Gerenciador de Pacotes].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publicar o **[!DNL WKND Mobile Events API]Modelo editável**
   1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Ferramentas] > [!UICONTROL Geral] > [!UICONTROL Modelos] >[!DNL WKND Mobile]**
   1. Selecione o modelo **[!DNL Event API]**
   1. Toque em **[!UICONTROL Publicar]** na barra de ações superior
   1. Publique o **template** e **todas as referências** (políticas de conteúdo, mapeamentos de política de conteúdo e modelos)

1. Publique os **[!DNL WKND Mobile Events]fragmentos de conteúdo**.

Observe que isso é necessário, pois a API de eventos usa o componente Lista de fragmentos do conteúdo , que não faz referência específica aos Fragmentos do conteúdo.
1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Arquivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
1. Selecione todos os **[!DNL Event]** fragmentos de conteúdo
1. Toque em **[!UICONTROL Gerenciar publicação]** na barra de ação superior
1. Deixando a ação padrão **Publicar** como está, toque em **[!UICONTROL Próximo]** na barra de ação superior
1. Selecione **todos** fragmentos de conteúdo
1. Toque em **[!UICONTROL Publicar]** na barra de ação superior
* *O [!DNL Events] Modelo de Fragmento de Conteúdo e as referências a Imagens de Evento serão publicadas automaticamente junto com os fragmentos de conteúdo.*

1. Publique a página **[!DNL Events API]**.
   1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Selecione a página **[!DNL Events]**
   1. Toque em **[!UICONTROL Gerenciar publicação]** na barra de ação superior
   1. Deixando a ação padrão **Publicar** como está, toque em **[!UICONTROL Próximo]** na barra de ação superior
   1. Selecione a página **[!DNL Events]**
   1. Toque em **[!DNL Publish]** na barra de ação superior

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## Verificar a publicação do AEM

1. Em um novo navegador da Web, verifique se você está desconectado da Publicação AEM e solicite os seguintes URLs (substituindo `http://localhost:4503` por qualquer host:port em que o AEM Publish está em execução).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Essas solicitações devem retornar a mesma resposta JSON de quando os pontos finais correspondentes do Autor do AEM foram revisados. Caso contrário, verifique se todas as publicações foram bem-sucedidas (verifique as filas de Replicação), o pacote [!DNL WKND Mobile] `ui.apps` é instalado no AEM Publish e revise `error.log` para o AEM Publish.

## Próxima etapa

Não há pacotes extras para instalar. Certifique-se de que o conteúdo e a configuração descritos nesta seção sejam publicados na Publicação do AEM; caso contrário, os capítulos subsequentes não funcionarão.

* [Capítulo 7 - Consumo dos serviços de conteúdo do AEM a partir de um aplicativo móvel](./chapter-7.md)
