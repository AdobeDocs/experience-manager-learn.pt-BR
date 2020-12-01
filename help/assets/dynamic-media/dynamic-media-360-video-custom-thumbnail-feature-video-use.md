---
title: Uso de vídeos do Dynamic Media 360 e miniatura de vídeo personalizada com o AEM Assets
seo-title: Uso de vídeos do Dynamic Media 360 e miniatura de vídeo personalizada com o AEM Assets
description: As melhorias do Visualizador de mídia dinâmica no AEM 6.5 incluem a adição de suporte para renderização de vídeo 360, visualizadores de mídia 360 (vídeo360Social e vídeo360VR) e a capacidade de selecionar miniaturas de vídeo personalizadas.
seo-description: As melhorias do Visualizador de mídia dinâmica no AEM 6.5 incluem a adição de suporte para renderização de vídeo 360, visualizadores de mídia 360 (vídeo360Social e vídeo360VR) e a capacidade de selecionar miniaturas de vídeo personalizadas.
uuid: 44b91c22-635c-48c2-af27-49bdbfb61639
discoiquuid: 67d5e0f2-3fde-4ea7-9e53-4fc0cf8b9f2a
sub-product: dynamic-media
feature: video-profiles, viewer-presets
topics: images, videos, renditions, authoring, integrations, publishing, metadata
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 2%

---


# Uso de vídeos do Dynamic Media 360 e miniatura de vídeo personalizada com o AEM Assets

As melhorias do Visualizador de mídia dinâmica no AEM 6.5 incluem a adição de suporte para renderização de vídeo 360, visualizadores de mídia 360 (vídeo360Social e vídeo360VR) e a capacidade de selecionar miniaturas de vídeo personalizadas.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>O vídeo supõe que sua instância AEM esteja em execução no modo Dynamic Media S7.  [As instruções sobre como configurar o AEM com o Dynamic Media podem ser encontradas aqui](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Quando você carrega um vídeo, por padrão, o Dynamic Media processa a gravação como um vídeo 360, se ela tiver uma proporção de 2:1. ou seja, a relação largura/altura é de 2:1.

>[!NOTE]
>
>Os componentes do Dynamic Media 360 Media suportam apenas 360 vídeos.

## Vídeos do Dynamic Media 360

Os vídeos de 360 graus, também conhecidos como vídeos esféricos, são gravações de vídeo em que uma visualização em todas as direções é gravada ao mesmo tempo, fotografada usando uma câmera onidirecional ou uma coleção de câmeras. Durante a reprodução em uma tela plana, o usuário tem controle da direção de visualização e a reprodução em dispositivos móveis normalmente aproveita o controle de giroscópio integrado.  Ele permite que você se expanda além dos limites da fotografia única. Os profissionais de marketing podem oferecer aos usuários uma experiência envolvente com a ajuda de 360 vídeos.  Vamos começar. O critério de proporção da imagem panorâmica pode ser modificado na configuração do empresa DMS7 especificando a propriedade do duplo s7PanorâmicaAR em /conf/global/settings/cloudconfigs/dmsceno7/jcr:content.

## Vídeos do Dynamic Media 360

O vídeo do Dynamic Media agora oferece suporte à capacidade de selecionar uma miniatura personalizada para seu vídeo. Um usuário pode selecionar um ativo existente do AEM Assets ou selecionar um quadro de vídeo como miniatura.

## Visualizadores de mídia dinâmicos 360

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Vídeo360Visualizador Social**</td>
      <td>**Visualizador de vídeo360VR**</td>
   </tr>
   <tr>
      <td>Modo de execução do Dynamic Media</td>
      <td>Somente modo Scene7 do Dynamic Media</td>
      <td>Modo Scene7 do Dynamic Media apenas<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>Caso de uso </td>
      <td>
         <p>Para sites e dispositivos que não suportam giroscópio</p>
         <p> </p>
      </td>
      <td>
         <p>Fornece uma experiência de realidade virtual para um dispositivo compatível com giroscópio </p>
      </td>
   </tr>
   <tr>
      <td>Áudio - Modo estéreo</td>
      <td>Não</td>
      <td>Sim</td>
   </tr>
   <tr>
      <td>Reprodução de vídeo</td>
      <td>Sim</td>
      <td>Sim</td>
   </tr>
   <tr>
      <td>Navegação no ponto de visualização</td>
      <td>
         <ul>
            <li>Arrastar o mouse (em sistemas de desktop)</li>
            <li>Deslizar (dispositivos de toque)</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>As opções de mouse e arrastar estão desativadas</li>
            <li>Usa giroscópio integrado</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>HTML5 Player</td>
      <td>Sim</td>
      <td>Sim</td>
   </tr>
   <tr>
      <td>Opções de compartilhamento em redes sociais</td>
      <td>Sim</td>
      <td>Não</td>
   </tr>
</tbody>
</table>

## Recursos adicionais{#additional-resources}

[Configuração do Dynamic Media no modo Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
