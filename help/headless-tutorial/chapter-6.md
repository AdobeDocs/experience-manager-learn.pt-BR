---
title: Introdução ao AEM sem cabeçalho - Capítulo 6 - Exposição do conteúdo no AEM Publish como JSON
description: O Capítulo 6 do tutorial AEM sem cabeçalho cobre como garantir que todos os pacotes, configurações e conteúdo necessários estejam no AEM Publish para permitir o consumo do aplicativo móvel.
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 0%

---


# Capítulo 6 - Exposição do conteúdo no AEM Publish for Delivery

O Capítulo 6 do tutorial AEM sem cabeçalho cobre como garantir que todos os pacotes, configurações e conteúdo necessários estejam no AEM Publish para permitir o consumo pelo aplicativo móvel.

## Publicar o conteúdo para AEM Content Services

A configuração e o conteúdo criados para direcionar os Eventos por meio AEM Content Services devem ser publicados no AEM Publish para que o aplicativo móvel possa acessá-los.

Como AEM Content Services é criado a partir de Configuração (Modelos de fragmento de conteúdo, Modelos editáveis), Ativos (Fragmentos de conteúdo, Imagens) e Páginas, todas essas partes desfrutam automaticamente dos recursos de gestão de conteúdo AEM, incluindo:

* Fluxo de trabalho para revisão e processamento
* e ativação/desativação para enviar e extrair conteúdo dos pontos finais do AEM Content Services do AEM Publish

1. Verifique se os **[!DNL WKND Mobile]Pacotes** de aplicativos, listados no [Capítulo 1](./chapter-1.md#wknd-mobile-application-packages), estão instalados no **AEM Publish** usando o [!UICONTROL Package Manager].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publicar o modelo **[!DNL WKND Mobile Events API]editável**
   1. Navegue até **[!UICONTROL AEM]>[!UICONTROL Ferramentas]>[!UICONTROL Geral]>[!UICONTROL Modelos]>[!DNL WKND Mobile]**
   1. Select the **[!DNL Event API]** template
   1. Toque em **[!UICONTROL Publicar]** na barra de ação superior
   1. Publicar o **modelo** e **todas as referências** (políticas de conteúdo, mapeamentos de política de conteúdo e modelos)

1. Publique os fragmentos **[!DNL WKND Mobile Events]do** conteúdo.

Observe que isso é necessário, pois a API Eventos usa o componente de Lista Fragmento de conteúdo, que não faz referência específica aos Fragmentos de conteúdo.
1. Navegue até **[!UICONTROL AEM]>[!UICONTROL Ativos]>[!UICONTROL Arquivos]>[!DNL WKND Mobile]>[!DNL English]>[!DNL Events]** 1. Selecione todos os fragmentos de **[!DNL Event]** conteúdo1. Toque em **[!UICONTROL Gerenciar publicação]** na barra de ação superior1. Deixando a ação padrão **Publicar** como está, toque em **[!UICONTROL Próximo]** na barra de ação superior1. Selecione **todos** os fragmentos de conteúdo1. Toque em **[!UICONTROL Publicar]** na barra de ação superior* *O Modelo de fragmento de[!DNL Events]conteúdo e as imagens de Evento de referência serão publicados automaticamente juntamente com os fragmentos de conteúdo.*

1. Publique a **[!DNL Events API]página**.
   1. Navegue até **[!UICONTROL AEM]>[!UICONTROL Sites]>[!DNL WKND Mobile]>[!DNL English]>[!DNL API]**
   1. Selecionar a **[!DNL Events]** página
   1. Toque em **[!UICONTROL Gerenciar publicação]** na barra de ação superior
   1. Deixando a ação padrão **Publicar** como está, toque em **[!UICONTROL Próximo]** na barra de ação superior
   1. Selecionar a **[!DNL Events]** página
   1. Toque **[!DNL Publish]** na barra de ação superior

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## Verificar a publicação do AEM

1. Em um novo navegador da Web, verifique se você está logado fora do AEM Publish e solicite os seguintes URLs (substituindo `http://localhost:4503` por qualquer host:porta em que o AEM Publish está sendo executado).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Essas solicitações devem retornar a mesma resposta JSON de quando os pontos finais correspondentes do autor de AEM forem revisados. Caso contrário, verifique se todas as publicações foram bem-sucedidas (verifique as filas de Replicação), se o pacote [!DNL WKND Mobile]`ui.apps` está instalado no AEM Publish e reveja o `error.log` para AEM Publish.

## Próxima etapa

Não há pacotes adicionais para instalar. Certifique-se de que o conteúdo e a configuração descritos nesta seção estejam publicados no AEM Publish, caso contrário os capítulos subsequentes não funcionarão.

* [Capítulo 7 - Consumir AEM Content Services de um aplicativo móvel](./chapter-7.md)
