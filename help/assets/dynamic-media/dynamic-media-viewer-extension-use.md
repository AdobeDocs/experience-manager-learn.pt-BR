---
title: Utilização dos visualizadores do Dynamic Media com o Adobe Analytics e o Adobe Launch
description: A extensão Visualizadores do Dynamic Media para o Adobe Launch, juntamente com o lançamento dos Visualizadores do Dynamic Media 5.13, permite que os clientes do Dynamic Media, do Adobe Analytics e do Adobe Launch usem eventos e dados específicos para os Visualizadores do Dynamic Media nas configurações do Adobe Launch.
sub-product: Dynamic Media
feature: Asset Insights
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 9d807f4c-999c-45e6-a9db-6c1776bddda1
duration: 623
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 18%

---

# Utilização dos visualizadores do Dynamic Media com o Adobe Analytics e o Adobe Launch{#using-dynamic-media-viewers-adobe-analytics-launch}

Para clientes com o Dynamic Media e o Adobe Analytics, agora é possível rastrear o uso dos Visualizadores do Dynamic Media no site usando a Extensão do Visualizador do Dynamic Media.

>[!VIDEO](https://video.tv.adobe.com/v/29308?quality=12&learn=on)

>[!NOTE]
>
> Execute o Adobe Experience Manager no modo Dynamic Media Scene7 para essa funcionalidade. Também é necessário [integrar o Adobe Experience Platform Launch com sua instância do AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=pt-BR).

Com a introdução da extensão do Visualizador do Dynamic Media, a Adobe Experience Manager agora oferece suporte avançado de análise para ativos fornecidos com visualizadores do Dynamic Media (5.13), fornecendo controle mais granular sobre o rastreamento de eventos quando um Visualizador do Dynamic Media é usado em uma página do Sites.

Se você já tiver o AEM Assets e o Sites, poderá integrar sua propriedade do Launch com sua instância de autor do AEM. Assim que a integração do Launch estiver associada ao seu site, você poderá adicionar componentes de mídia dinâmica à sua página com o rastreamento de eventos para visualizadores ativado.

Para clientes somente do AEM Assets ou clientes do Dynamic Media Classic, o usuário pode obter o código incorporado de um visualizador e adicioná-lo à página. As bibliotecas de scripts do Launch podem ser adicionadas manualmente à página para rastreamento de eventos do visualizador.

A tabela a seguir lista os eventos do Visualizador do Dynamic Media e seus argumentos compatíveis:

<table>
   <tbody>
      <tr>
         <td>Nome do evento do visualizador</td>
         <td>Referência de argumento</td>
      </tr>
      <tr>
         <td> COMUM </td>
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
         <td> ETAPA </td>
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
         <td> PAUSAR </td>
         <td> %event.detail.dm.PAUSE.timestamp% </td>
      </tr>
      <tr>
         <td> PLAY </td>
         <td> %event.detail.dm.PLAY.timestamp% </td>
      </tr>
      <tr>
         <td> ROTAÇÃO </td>
         <td> %event.detail.dm.SPIN.framenumber% </td>
      </tr>
      <tr>
         <td> PARAR </td>
         <td> %event.detail.dm.STOP.timestamp% </td>
      </tr>
      <tr>
         <td> TROCAR </td>
         <td> %event.detail.dm.SWAP.asset% </td>
      </tr>
      <tr>
         <td> AMOSTRA </td>
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

* [Integração do Adobe Experience Manager com o Adobe Launch](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=pt-BR)
* [Execução do Adobe Experience Manager no modo Dynamic Media Scene7](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dms7.html?lang=en)
* [Integração dos visualizadores do Dynamic Media ao Adobe Analytics e ao Adobe Launch](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/dynamic-media-viewer-extension-use.html)
