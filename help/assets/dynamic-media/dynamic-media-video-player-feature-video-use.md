---
title: Utilização do reprodutor de vídeo no AEM Dynamic Media
description: O reprodutor de vídeo AEM Dynamic Media, usado para depender do tempo de execução do Flash para suportar o streaming de vídeo adaptável em clientes desktop e navegadores, tornou-se mais agressivo no streaming de conteúdo baseado em flash. Apple Com a introdução do HLS (HTTP Live Streaming video delivery protocol), o conteúdo agora pode ser transmitido sem depender do flash.
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 6%

---


# Utilização do reprodutor de vídeo no AEM Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

O reprodutor de vídeo AEM Dynamic Media, usado para depender do tempo de execução do Flash para suportar o streaming de vídeo adaptável em clientes desktop e navegadores, tornou-se mais agressivo no streaming de conteúdo baseado em flash. Apple Com a introdução do HLS (HTTP Live Streaming video delivery protocol), o conteúdo agora pode ser transmitido sem depender do flash.

>[!VIDEO](https://video.tv.adobe.com/v/16791?quality=12&learn=on)

## Busca rápida no reprodutor de vídeo sem Flash {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429?quality=12&learn=on)

O suporte para navegador HLS é o seguinte: para navegadores não compatíveis, fazemos o fallback para a entrega de vídeo progressiva

>[!NOTE]
>
> O Dynamic Media Hybrid NÃO é compatível com o streaming de vídeo no Internet Explorer 11 a partir de 15 de março de 2022. Atualize para a versão 6.5.12 ou superior para voltar à reprodução progressiva no IE 11.

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
   <td> <p>Desktop</p> </td>
   <td> <p>Internet Explorer 9 e 10</p> </td>
   <td> <p>Download progressivo</p> </td>
  </tr>
  <tr>
   <td> <p>Desktop</p> </td>
   <td> <p>Internet Explorer 11+</p> </td>
   <td> <p>Dynamic Media - Modo Scene 7: Transmissão de vídeo HLS</p> 
        <p>Dynamic Media - Modo híbrido: Download progressivo</p>
   </td>
  </tr>
  <tr>
   <td> <p>Desktop</p> </td>
   <td> <p>Firefox 23-44</p> </td>
   <td> <p>Download progressivo</p> </td>
  </tr>
  <tr> 
   <td> <p>Desktop</p> </td>
   <td> <p>Firefox 45 ou posterior</p> </td>
   <td> <p>Transmissão de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Desktop</p> </td>
   <td> <p>Chrome</p> </td>
   <td> <p>Transmissão de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Desktop</p> </td>
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
