---
title: Uso do reprodutor de vídeo no AEM Dynamic Media
seo-title: Uso do reprodutor de vídeo no AEM Dynamic Media
description: AEM player de vídeo do Dynamic Media costumava confiar no tempo de execução do Flash para suportar streaming de vídeo adaptável em clientes desktop e os navegadores se tornaram mais agressivos com o streaming de conteúdo baseado em flash. Com a introdução do HLS (HTTP Live Streaming video protocol) da Apple, o conteúdo agora pode ser transmitido em streaming sem depender do flash.
seo-description: AEM player de vídeo do Dynamic Media costumava confiar no tempo de execução do Flash para suportar streaming de vídeo adaptável em clientes desktop e os navegadores se tornaram mais agressivos com o streaming de conteúdo baseado em flash. Com a introdução do HLS (HTTP Live Streaming video protocol) da Apple, o conteúdo agora pode ser transmitido em streaming sem depender do flash.
uuid: aac6f471-4bed-4773-890f-0dd2ceee381d
discoiquuid: b01cc46b-ef64-4db9-b3b4-52d3f27bddf5
sub-product: dynamic-media
feature: media-player, video-profiles
topics: videos, renditions, authoring, best-practices
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 5%

---


# Uso do reprodutor de vídeo no AEM Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

AEM player de vídeo do Dynamic Media costumava confiar no tempo de execução do Flash para suportar streaming de vídeo adaptável em clientes desktop e os navegadores se tornaram mais agressivos com o streaming de conteúdo baseado em flash. Com a introdução do HLS (HTTP Live Streaming video protocol) da Apple, o conteúdo agora pode ser transmitido em streaming sem depender do flash.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Análise rápida do player de vídeo não-Flash {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

O suporte ao navegador HLS é o seguinte: para navegadores não suportados, revertemos para o delivery de vídeo progressivo

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
   <td> <p>Streaming de vídeo HLS</p> </td>
  </tr>
  <tr>
   <td> <p>Área de trabalho</p> </td>
   <td> <p>Firefox 23-44</p> </td>
   <td> <p>Download progressivo</p> </td>
  </tr>
  <tr> 
   <td> <p>Área de trabalho</p> </td>
   <td> <p>Firefox 45 ou posterior</p> </td>
   <td> <p>Streaming de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Área de trabalho</p> </td>
   <td> <p>Cromo</p> </td>
   <td> <p>Streaming de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Área de trabalho</p> </td>
   <td> <p>Safari (Mac)</p> </td>
   <td> <p>Streaming de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvel</p> </td>
   <td> <p>Chrome (Android 6 ou anterior)</p> </td>
   <td> <p>Download progressivo</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvel</p> </td>
   <td> <p>Chrome (Android 7 ou posterior)</p> </td>
   <td> <p>Streaming de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvel</p> </td>
   <td> <p>Android (navegador padrão)</p> </td>
   <td> <p>Download progressivo</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvel</p> </td>
   <td> <p>Safari (iOS)</p> </td>
   <td> <p>Streaming de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvel</p> </td>
   <td> <p>Chrome (iOS)</p> </td>
   <td> <p>Streaming de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvel</p> </td>
   <td> <p>Blackberry</p> </td>
   <td> <p>Streaming de vídeo HLS</p> </td>
  </tr>
 </tbody>
</table>