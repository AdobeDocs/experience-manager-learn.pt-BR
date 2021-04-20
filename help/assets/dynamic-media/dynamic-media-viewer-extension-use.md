---
title: Uso de visualizadores do Dynamic Media com o Adobe Analytics e o Adobe Launch
description: A extensão do Dynamic Media Viewers para Adobe Launch, juntamente com o lançamento do Dynamic Media Viewers 5.13, permite que os clientes do Dynamic Media, do Adobe Analytics e do Adobe Launch usem eventos e dados específicos dos Dynamic Media Viewers na configuração do Adobe Launch.
sub-product: Dynamic Media
feature: Asset Insights
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 17%

---


# Uso de visualizadores do Dynamic Media com o Adobe Analytics e o Adobe Launch{#using-dynamic-media-viewers-adobe-analytics-launch}

Para clientes com o Dynamic Media e o Adobe Analytics, agora é possível rastrear o uso de Visualizadores do Dynamic Media no seu site usando a Extensão do Visualizador do Dynamic Media.

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> Execute o Adobe Experience Manager no modo Dynamic Media Scene7 para essa funcionalidade. Você também precisa [integrar o Adobe Experience Platform Launch à sua instância do AEM](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html).

Com a introdução da extensão Dynamic Media Viewer, o Adobe Experience Manager agora oferece suporte avançado a análises para ativos fornecidos com visualizadores do Dynamic Media (5.13), fornecendo controle mais granular sobre o rastreamento de eventos quando um Visualizador do Dynamic Media é usado em uma página do Sites.

Se já tiver o AEM Assets e Sites, é possível integrar a propriedade do Launch com a instância do autor do AEM. Depois que a integração do launch estiver associada ao site, você poderá adicionar componentes de mídia dinâmica à página com o rastreamento de eventos para visualizadores habilitados.

Para clientes exclusivos do AEM Assets ou clientes do Dynamic Media Classic, o usuário pode obter o código de inserção de um visualizador e adicioná-lo à página. As bibliotecas de script do Launch podem ser adicionadas manualmente à página para o rastreamento de eventos do visualizador.

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

* [Integração do Adobe Experience Manager com o Adobe Launch](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html)
* [Execução do Adobe Experience Manager no modo Dynamic Media Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
* [Integração dos visualizadores do Dynamic Media ao Adobe Analytics e ao Adobe Launch](https://helpx.adobe.com/experience-manager/6-5/assets/using/launch.html)
