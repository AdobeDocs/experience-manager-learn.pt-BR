---
title: Uso do Panorama e do Visualizador de Imagem Vertical com o AEM Assets Dynamic Media
description: Os aprimoramentos do Visualizador do Dynamic Media no AEM 6.4 incluem a adição do Visualizador de Imagem Panorâmica, Visualizador de Imagem de Realidade Virtual Panorâmica e Visualizador de Imagem Vertical. O Visualizador de panorâmica é uma maneira fácil de fornecer uma experiência envolvente e imersiva da sala, propriedade, localização ou paisagem, sem qualquer desenvolvimento personalizado.
sub-product: dynamic-media
feature: Video Profiles, Video Profiles, 360 VR Video
version: 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 3%

---


# Uso do Panorama e do Visualizador de Imagem Vertical com o AEM Assets Dynamic Media{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

Os aprimoramentos do Visualizador do Dynamic Media no AEM 6.4 incluem a adição do Visualizador de Imagem Panorâmica, Visualizador de Imagem de Realidade Virtual Panorâmica e Visualizador de Imagem Vertical. O Visualizador de panorâmica é uma maneira fácil de fornecer uma experiência envolvente e imersiva da sala, propriedade, localização ou paisagem, sem qualquer desenvolvimento personalizado.

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>O Vídeo assume que a instância do AEM está sendo executada no modo Dynamic Media S7. [Instruções sobre como configurar o AEM com o Dynamic Media podem ser encontradas aqui.](https://helpx.adobe.com/br/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Visualizador VR panorâmico e panorâmico

Uma imagem é considerada panorâmica com base em sua proporção ou palavras-chave. Por padrão, uma imagem com proporção de aspecto de 2 será considerada uma imagem panorâmica. As predefinições do visualizador de Imagem panorâmica ficarão disponíveis para uma visualização de imagem se ela atender aos critérios acima. O critério de proporção da imagem panorâmica pode ser modificado na configuração do DMS7 da empresa, especificando a propriedade dupla s7PanorâmicaAR em /conf/global/settings/cloudconfigs/dmscene7/jcr:content. As palavras-chave são armazenadas na propriedade dc:keyword do nó de metadados do ativo. Se as palavras-chave contiverem qualquer uma das seguintes combinações :

* Retangular
* esférico + panorâmica,
* esférico + panorama,

será considerado um ativo de Imagem panorâmica, independentemente de sua proporção.

## Visualizador de imagem vertical

Com amostras horizontais, dependendo do tamanho da tela da área de trabalho do consumidor, às vezes as amostras não ficavam visíveis até que o usuário rolasse a página para baixo. Ao usar o visualizador de imagem vertical e colocar amostras verticais, ele garante que as amostras sejam visíveis, independentemente do tamanho da tela. Também maximiza o tamanho da imagem principal. Com amostras horizontais, foi necessário reservar espaço na página para garantir que elas tenham uma alta probabilidade de serem visíveis e diminuam o tamanho da imagem principal. Com um layout vertical, não é necessário se preocupar com a alocação desse espaço e, portanto, pode maximizar o tamanho da imagem principal.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>Visualizador panorâmico e VR</td>
   <td>Visualizador de imagem vertical</td>
  </tr>
  <tr>
   <td>Modo de execução do Dynamic Media</td>
   <td>Somente Modo do Dynamic Media Scene7</td>
   <td>DMS7 e Dynamic Media</td>
  </tr>
  <tr>
   <td>Caso de uso </td>
   <td><p>O visualizador panorâmico e o visualizador de Realidade virtual fornecem aos usuários uma experiência mais envolvente. Um usuário pode fazer check-out de um quarto de hotel mesmo antes de fazer uma reserva ou fazer check-out de uma propriedade de aluguel sem precisar agendar um compromisso. Um usuário pode verificar um local e muitas outras possibilidades. O foco principal aqui é fornecer ao consumidor uma melhor experiência quando ele visitar seu site e eventualmente aumentar sua taxa de conversão.</p> <p> </p> </td> 
   <td><p>O Visualizador de imagem vertical ajuda a maximizar a experiência de visualização de imagens do produto para fornecer aos consumidores a melhor representação do produto, aumentando a conversão e minimizando as devoluções.</p> <p> </p> </td>
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
>Os visualizadores panorâmicos trabalham com imagens panorâmicas e não devem ser usados com imagens normais.
