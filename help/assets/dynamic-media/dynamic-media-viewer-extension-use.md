---
title: Uso de visualizadores do Dynamic Media com Adobe Analytics e Adobe Launch
description: A extensão do Dynamic Media Viewers para Adobe Launch, juntamente com o lançamento do Dynamic Media Viewers 5.13, permite que os clientes do Dynamic Media, do Adobe Analytics e do Adobe Launch usem eventos e dados específicos dos Dynamic Media Viewers na configuração do Adobe Launch.
sub-product: Dynamic Media
feature: Asset Insights
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 15%

---


# Uso de visualizadores do Dynamic Media com Adobe Analytics e Adobe Launch{#using-dynamic-media-viewers-adobe-analytics-launch}

Para clientes com Dynamic Media e Adobe Analytics, agora você pode rastrear o uso de Visualizadores do Dynamic Media no seu site usando a Extensão do Visualizador do Dynamic Media.

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> Execute o Adobe Experience Manager no modo Dynamic Media Scene7 para essa funcionalidade. Você também precisa [integrar o Adobe Experience Platform Launch à instância do AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html).

Com a introdução da extensão Visualizador do Dynamic Media, a Adobe Experience Manager agora oferece suporte avançado a análises para ativos fornecidos com visualizadores do Dynamic Media (5.13), fornecendo controle mais granular sobre o rastreamento de eventos quando um Visualizador do Dynamic Media é usado em uma página do Sites.

Se já tiver o AEM Assets e o Sites, é possível integrar a propriedade do Launch com a instância do autor de AEM. Depois que a integração do launch estiver associada ao site, você poderá adicionar componentes de mídia dinâmica à página com o rastreamento de eventos para visualizadores habilitados.

Para clientes somente AEM Assets ou clientes do Dynamic Media Classic, o usuário pode obter o código incorporado de um visualizador e adicioná-lo à página. As bibliotecas de script do Launch podem ser adicionadas manualmente à página para o rastreamento de eventos do visualizador.

A tabela a seguir lista os eventos do Visualizador do Dynamic Media e seus argumentos compatíveis:

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
         <td> %event.detail.dm.BANER.asset% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANER.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANER.label% </td>
      </tr>
      <tr>
         <td> HREF </td>
         <td> %event.detail.dm.HREF.rolover% </td>
      </tr>
      <tr>
         <td> ITEM </td>
         <td> %event.detail.dm.ITEM.rolover% </td>
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
         <td> %event.detail.dm.LOAD.company% </td>
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
         <td> REPRODUZIR </td>
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

* [Integração do Adobe Experience Manager com o Adobe Launch](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)
* [Execução do Adobe Experience Manager no modo Dynamic Media Scene7](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dms7.html?lang=en)
* [Integração dos visualizadores do Dynamic Media ao Adobe Analytics e ao Adobe Launch](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/dynamic-media-viewer-extension-use.html)
