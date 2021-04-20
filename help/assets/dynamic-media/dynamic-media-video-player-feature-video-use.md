---
title: Uso do reprodutor de vídeo no AEM Dynamic Media
description: O reprodutor de vídeo do AEM Dynamic Media usado para depender do tempo de execução do Flash para suportar streaming de vídeo adaptável em clientes de desktop e navegadores tornou-se mais agressivo no streaming de conteúdo baseado em flash. Com a introdução do HLS (protocolo de entrega de vídeo HTTP Live Streaming da Apple), o conteúdo agora pode ser transmitido sem depender do flash.
sub-product: dynamic-media
feature: Video Profiles
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 8%

---


# Uso do reprodutor de vídeo no AEM Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

O reprodutor de vídeo do AEM Dynamic Media usado para depender do tempo de execução do Flash para suportar streaming de vídeo adaptável em clientes de desktop e navegadores tornou-se mais agressivo no streaming de conteúdo baseado em flash. Com a introdução do HLS (protocolo de entrega de vídeo HTTP Live Streaming da Apple), o conteúdo agora pode ser transmitido sem depender do flash.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Análise rápida do reprodutor de vídeo não Flash {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

O suporte a navegador HLS é o seguinte, para navegadores não compatíveis, fallback para entrega progressiva de vídeo

<table> 
 <thead> 
  <tr> 
   <th> <p>Device</p> </th>
   <th> <p>Navegador</p> </th>
   <th > <p>Modo de reprodução de vídeo</p> </th>
  </tr>
 </thead>
 <tbody>
  <tr> 
   <td> <p>Área de trabalho</p> </td>
   <td> <p>Internet Explorer 9 e 10</p> </td>
   <td> <p>Download progressivo</p> </td>
  </tr>
  <tr>
   <td> <p>Área de trabalho</p> </td>
   <td> <p>Internet Explorer 11+</p> </td>
   <td> <p>Transmissão de vídeo HLS</p> </td>
  </tr>
  <tr>
   <td> <p>Área de trabalho</p> </td>
   <td> <p>Firefox 23-44</p> </td>
   <td> <p>Download progressivo</p> </td>
  </tr>
  <tr> 
   <td> <p>Área de trabalho</p> </td>
   <td> <p>Firefox 45 ou superior</p> </td>
   <td> <p>Transmissão de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Área de trabalho</p> </td>
   <td> <p>Cromo</p> </td>
   <td> <p>Transmissão de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Área de trabalho</p> </td>
   <td> <p>Safari (Mac)</p> </td>
   <td> <p>Transmissão de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvel</p> </td>
   <td> <p>Chrome (Android 6 ou anterior)</p> </td>
   <td> <p>Download progressivo</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvel</p> </td>
   <td> <p>Chrome (Android 7 ou posterior)</p> </td>
   <td> <p>Transmissão de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvel</p> </td>
   <td> <p>Android (navegador padrão)</p> </td>
   <td> <p>Download progressivo</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvel</p> </td>
   <td> <p>Safari (iOS)</p> </td>
   <td> <p>Transmissão de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvel</p> </td>
   <td> <p>Chrome (iOS)</p> </td>
   <td> <p>Transmissão de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvel</p> </td>
   <td> <p>Blackberry</p> </td>
   <td> <p>Transmissão de vídeo HLS</p> </td>
  </tr>
 </tbody>
</table>