---
title: Uso de visualizadores de mídia dinâmica com Adobe Analytics e lançamento de Adobe
seo-title: Uso de visualizadores de mídia dinâmica com Adobe Analytics e lançamento de Adobe
description: A extensão do Dynamic Media Viewers para Adobe Launch, juntamente com o lançamento do Dynamic Media Viewers 5.13, permite que os clientes do Dynamic Media, do Adobe Analytics e do Adobe Launch usem eventos e dados específicos dos Dynamic Media Viewers na configuração do Adobe Launch.
seo-description: 'A extensão do Dynamic Media Viewers para Adobe Launch, juntamente com o lançamento do Dynamic Media Viewers 5.13, permite que os clientes do Dynamic Media, do Adobe Analytics e do Adobe Launch usem eventos e dados específicos dos Dynamic Media Viewers na configuração do Adobe Launch. '
sub-product: mídia dinâmica, análise
feature: asset-insights, media-player
topics: images, videos, renditions, authoring, integrations, publishing
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 23%

---


# Using Dynamic Media Viewers with Adobe Analytics and Adobe Launch{#using-dynamic-media-viewers-adobe-analytics-launch}

Para clientes com o Dynamic Media e o Adobe Analytics, agora é possível rastrear o uso de Visualizadores de Mídia Dinâmica em seu site usando o Dynamic Media Viewer Extension.

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> Execute o Adobe Experience Manager no modo Scene7 do Dynamic Media para essa funcionalidade. Você também precisa [integrar o Adobe Experience Platform Launch à sua instância](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html)AEM.

Com a introdução da extensão do Visualizador de Mídia Dinâmica, a Adobe Experience Manager agora oferta oferece suporte avançado a análises para ativos fornecidos com visualizadores de Mídia Dinâmica (5.13), fornecendo controle mais granular sobre o rastreamento de eventos quando um Visualizador de Mídia Dinâmica é usado em uma página Sites.

Se você já tiver o AEM Assets e o Sites, poderá integrar sua propriedade Launch à sua instância AEM do autor. Depois que a integração de inicialização for associada ao seu site, você poderá adicionar componentes de mídia dinâmica à sua página com o rastreamento de eventos para visualizadores habilitados.

Para clientes somente AEM Assets ou clientes do Dynamic Media Classic, o usuário pode obter o código incorporado para um visualizador e adicioná-lo à página. As bibliotecas de Script de inicialização podem ser adicionadas manualmente à página para o rastreamento de eventos do visualizador.

A tabela a seguir lista eventos do Visualizador de Mídia Dinâmica do e seus argumentos suportados:

<table>
   <tbody>
      <tr>
         <td>Nome do evento do visualizador</td>
         <td>Referência do argumento</td>
      </tr>
      <tr>
         <td> FREQUENTES </td>
         <td> %event.detail.dm.objID% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.compClass% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.instName% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.timeStamp% </td>
      </tr>
      <tr>
         <td> BANNER <br></td>
         <td> %event.detail.dm.BANNER.asset% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.label% </td>
      </tr>
      <tr>
         <td> HREF </td>
         <td> %event.detail.dm.HREF.rollover% </td>
      </tr>
      <tr>
         <td> ITEM </td>
         <td> %event.detail.dm.ITEM.rollover% </td>
      </tr>
      <tr>
         <td> CARREGAR </td>
         <td> %event.detail.dm.LOAD.applicationname% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.asset% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.empresa% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.sdkversion% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewertype% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewerversion% </td>
      </tr>
      <tr>
         <td> METADADOS </td>
         <td> %event.detail.dm.METADATA.length% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.METADATA.type% </td>
      </tr>
      <tr>
         <td> MARCO </td>
         <td> %event.detail.dm.MILESTONE.milestone% </td>
      </tr>
      <tr>
         <td> PÁGINA </td>
         <td> %event.detail.dm.PAGE.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.PAGE.label% </td>
      </tr>
      <tr>
         <td> PAUSA </td>
         <td> %event.detail.dm.PAUSE.timestamp% </td>
      </tr>
      <tr>
         <td> JOGAR </td>
         <td> %event.detail.dm.PLAY.timestamp% </td>
      </tr>
      <tr>
         <td> SPIN </td>
         <td> %event.detail.dm.SPIN.framenumber% </td>
      </tr>
      <tr>
         <td> PARAR </td>
         <td> %event.detail.dm.STOP.timestamp% </td>
      </tr>
      <tr>
         <td> SWAP </td>
         <td> %event.detail.dm.SWAP.asset% </td>
      </tr>
      <tr>
         <td> SWATCH </td>
         <td> %event.detail.dm.SWATCH.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.SWATCH.label% </td>
      </tr>
      <tr>
         <td> TARG </td>
         <td> %event.detail.dm.TARG.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.TARG.label% </td>
      </tr>
      <tr>
         <td> ZOOM </td>
         <td> %event.detail.dm.ZOOM.scale% </td>
      </tr>
   </tbody>
</table>

## Recursos adicionais{#additional-resources}

* [Integração da Adobe Experience Manager com o Adobe Launch](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html)
* [Execução do Adobe Experience Manager no modo Scene7 do Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
* [Integração dos visualizadores do Dynamic Media ao Adobe Analytics e ao Adobe Launch](https://helpx.adobe.com/experience-manager/6-5/assets/using/launch.html)
