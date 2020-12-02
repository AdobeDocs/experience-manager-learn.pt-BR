---
title: Capítulo 6 - Exposição do conteúdo na publicação do AEM como JSON - Serviços de conteúdo
description: O Capítulo 6 do tutorial AEM sem cabeçalho cobre como garantir que todos os pacotes, configurações e conteúdo necessários estejam no AEM Publish para permitir o consumo do aplicativo móvel.
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---


# Capítulo 6 - Exposição do conteúdo no AEM Publish for Delivery

O Capítulo 6 do tutorial AEM sem cabeçalho cobre como garantir que todos os pacotes, configurações e conteúdo necessários estejam no AEM Publish para permitir o consumo pelo aplicativo móvel.

## Publicar o conteúdo para AEM Content Services

A configuração e o conteúdo criados para direcionar os Eventos por meio AEM Content Services devem ser publicados no AEM Publish para que o aplicativo móvel possa acessá-los.

Como AEM Content Services é criado a partir de Configuração (Modelos de fragmento de conteúdo, Modelos editáveis), Ativos (Fragmentos de conteúdo, Imagens) e Páginas, todas essas partes desfrutam automaticamente dos recursos de gestão de conteúdo AEM, incluindo:

* Fluxo de trabalho para revisão e processamento
* e ativação/desativação para enviar e extrair conteúdo dos pontos finais do AEM Content Services do AEM Publish

1. Verifique se os **[!DNL WKND Mobile]Pacotes de aplicativos**, listados em [Capítulo 1](./chapter-1.md#wknd-mobile-application-packages), estão instalados em **AEM Publish** usando [!UICONTROL Gerenciador de pacotes].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publicar o **[!DNL WKND Mobile Events API]Modelo editável**
   1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Ferramentas] > [!UICONTROL Geral] > [!UICONTROL Modelos] >[!DNL WKND Mobile]**
   1. Selecione o modelo **[!DNL Event API]**
   1. Toque em **[!UICONTROL Publicar]** na barra de ação superior
   1. Publique **template** e **todas as referências** (políticas de conteúdo, mapeamentos de política de conteúdo e modelos)

1. Publique os **[!DNL WKND Mobile Events]fragmentos de conteúdo**.

Observe que isso é necessário, pois a API Eventos usa o componente de Lista Fragmento de conteúdo, que não faz referência específica aos Fragmentos de conteúdo.
1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Ativos] > [!UICONTROL Arquivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
1. Selecionar todos os fragmentos de conteúdo **[!DNL Event]**
1. Toque em **[!UICONTROL Gerenciar publicação]** na barra de ação superior
1. Deixando a ação padrão **Publicar** como está, toque **[!UICONTROL Próximo]** na barra de ação superior
1. Selecionar **todos** fragmentos de conteúdo
1. Toque em **[!UICONTROL Publicar]** na barra de ação superior
* *O [!DNL Events] Modelo de fragmento de conteúdo e as imagens de Evento de referência serão publicadas automaticamente juntamente com os fragmentos de conteúdo.*

1. Publique a página **[!DNL Events API]**.
   1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Selecione a página **[!DNL Events]**
   1. Toque em **[!UICONTROL Gerenciar publicação]** na barra de ação superior
   1. Deixando a ação padrão **Publicar** como está, toque em **[!UICONTROL Próximo]** na barra de ação superior
   1. Selecione a página **[!DNL Events]**
   1. Toque em **[!DNL Publish]** na barra de ação superior

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## Verificar a publicação do AEM

1. Em um novo navegador da Web, certifique-se de estar desconectado do AEM Publish e solicitar os seguintes URLs (substituindo `http://localhost:4503` por qualquer host:porta em que o AEM Publish esteja sendo executado).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Essas solicitações devem retornar a mesma resposta JSON de quando os pontos finais correspondentes do autor de AEM forem revisados. Caso contrário, verifique se todas as publicações foram bem-sucedidas (verifique as filas de replicação), o pacote [!DNL WKND Mobile] `ui.apps` será instalado no AEM Publish e reveja `error.log` para o AEM Publish.

## Próxima etapa

Não há pacotes adicionais para instalar. Certifique-se de que o conteúdo e a configuração descritos nesta seção estejam publicados no AEM Publish, caso contrário os capítulos subsequentes não funcionarão.

* [Capítulo 7 - Consumir AEM Content Services de um aplicativo móvel](./chapter-7.md)
