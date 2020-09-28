---
title: Usando o Panorama e o Visualizador de Imagem Vertical com o AEM Assets Dynamic Media
seo-title: Usando o Panorama e o Visualizador de Imagem Vertical com o AEM Assets Dynamic Media
description: Os aprimoramentos do Visualizador de mídia dinâmica no AEM 6.4 incluem a adição do Visualizador de imagem panorâmico, do Visualizador de imagem de realidade virtual panorâmica e do Visualizador de imagem vertical. O Visualizador panorâmico oferece uma maneira fácil de oferecer uma experiência envolvente e imersiva da sala, propriedade, local ou paisagem sem qualquer desenvolvimento personalizado.
seo-description: Os aprimoramentos do Visualizador de mídia dinâmica no AEM 6.4 incluem a adição do Visualizador de imagem panorâmico, do Visualizador de imagem de realidade virtual panorâmica e do Visualizador de imagem vertical. O Visualizador panorâmico oferece uma maneira fácil de oferecer uma experiência envolvente e imersiva da sala, propriedade, local ou paisagem sem qualquer desenvolvimento personalizado.
sub-product: dynamic-media
feature: video-profiles, video-profiles, vr-360
topics: videos, renditions, authoring
doc-type: feature video
audience: all
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 1%

---


# Usando o Panorama e o Visualizador de Imagem Vertical com o AEM Assets Dynamic Media{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

Os aprimoramentos do Visualizador de mídia dinâmica no AEM 6.4 incluem a adição do Visualizador de imagem panorâmico, do Visualizador de imagem de realidade virtual panorâmica e do Visualizador de imagem vertical. O Visualizador panorâmico oferece uma maneira fácil de oferecer uma experiência envolvente e imersiva da sala, propriedade, local ou paisagem sem qualquer desenvolvimento personalizado.

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>O vídeo supõe que sua instância AEM esteja em execução no modo Dynamic Media S7. [As instruções sobre como configurar o AEM com o Dynamic Media podem ser encontradas aqui.](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Visualizador VR panorâmico e panorâmico

Uma imagem é considerada panorâmica com base em sua proporção ou palavras-chave. Por padrão, uma imagem com proporção de 2 será considerada uma imagem panorâmica. As predefinições do visualizador de imagens panorâmicas estarão disponíveis para uma pré-visualização de imagem se ela atender aos critérios acima. O critério de proporção da imagem panorâmica pode ser modificado na configuração do empresa DMS7 especificando a propriedade do duplo s7PanorâmicaAR em /conf/global/settings/cloudconfigs/dmsceno7/jcr:content. As palavras-chave são armazenadas na propriedade dc:keyword do nó de metadados do ativo. Se as palavras-chave contiverem qualquer uma das seguintes combinações:

* equirectangular,
* esférico + panorâmico,
* esférico + panorama,

será considerado um ativo de Imagem panorâmica, independentemente da sua proporção.

## Visualizador de imagem vertical

Com amostras horizontais, dependendo do tamanho da tela do desktop do consumidor, às vezes as amostras não ficariam visíveis até que o usuário role para baixo na página. Ao usar o visualizador de imagem vertical e colocar amostras verticais, ele garante que as amostras fiquem visíveis independentemente do tamanho da tela. Também maximiza o tamanho da imagem principal. Com amostras horizontais, foi necessário reservar espaço na página para garantir que elas tenham alta probabilidade de serem visíveis e diminuam o tamanho da imagem principal. Com um layout vertical, não é necessário se preocupar em alocar esse espaço e, portanto, pode maximizar o tamanho da imagem principal.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>Visualizador Panorâmico e VR</td>
   <td>Visualizador de imagem vertical</td>
  </tr>
  <tr>
   <td>Modo de execução do Dynamic Media</td>
   <td>Somente modo Scene7 do Dynamic Media</td>
   <td>DMS7 e Dynamic Media</td>
  </tr>
  <tr>
   <td>Caso de uso </td>
   <td><p>O visualizador panorâmico e o visualizador de realidade virtual fornecem aos usuários uma experiência mais envolvente. Um usuário pode dar uma olhada em um quarto de hotel mesmo antes de fazer uma reserva, ou dar uma olhada em uma propriedade de aluguel sem precisar marcar uma reunião. Um usuário pode verificar um local e muito mais possibilidades. O foco principal aqui é fornecer ao consumidor uma experiência melhor quando ele visitar seu site e eventualmente aumentar sua taxa de conversão.</p> <p> </p> </td> 
   <td><p>O visualizador de Imagem vertical ajuda a maximizar a experiência de visualização de imagens de produtos para fornecer aos consumidores a melhor representação do produto, impulsionando a conversão e minimizando o retorno.</p> <p> </p> </td>
  </tr>
  <tr>
   <td>Disponível </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[Configuração do Dynamic Media no modo Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

[Configuração do Dynamic Media no modo Híbrido](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>Os visualizadores panorâmicos funcionam com imagens panorâmicas e não devem ser usados com imagens normais.
