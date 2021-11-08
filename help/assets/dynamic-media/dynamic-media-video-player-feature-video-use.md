---
title: Uso do reprodutor de vídeo no AEM Dynamic Media
description: AEM Dynamic Media video player costumava confiar no tempo de execução do Flash para suportar streaming de vídeo adaptável em clientes de desktop e os navegadores tornaram-se mais agressivos no streaming de conteúdo baseado em flash. Com a introdução do HLS (Apple HTTP Live Streaming video delivery protocol), o conteúdo agora pode ser transmitido em fluxo sem depender do flash.
sub-product: dynamic-media
feature: Video Profiles
version: 6.3, 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
source-git-commit: c921594d5c352f98e0d830d7a85e026844fd5da6
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 6%

---


# Uso do reprodutor de vídeo no AEM Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

AEM Dynamic Media video player costumava confiar no tempo de execução do Flash para suportar streaming de vídeo adaptável em clientes de desktop e os navegadores tornaram-se mais agressivos no streaming de conteúdo baseado em flash. Com a introdução do HLS (Apple HTTP Live Streaming video delivery protocol), o conteúdo agora pode ser transmitido em fluxo sem depender do flash.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Análise rápida do reprodutor de vídeo que não é o Flash {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

O suporte a navegador HLS é o seguinte, para navegadores não compatíveis, fallback para entrega progressiva de vídeo

>[!NOTE]
>
> O Dynamic Media Hybrid NÃO oferecerá suporte ao streaming de vídeo no Internet Explorer 11 após maio de 2022.

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
