---
title: Utilização do Panorama e do Visualizador de imagem vertical com o AEM Assets Dynamic Media
description: As melhorias do Dynamic Media Viewer no AEM 6.4 incluem a adição do Panoramic Image Viewer, Panoramic Virtual Reality Image Viewer e Vertical Image Viewer. O Visualizador panorâmico oferece uma maneira fácil de proporcionar uma experiência envolvente e imersiva da sala, propriedade, localização ou paisagem sem qualquer desenvolvimento personalizado.
feature: Video Profiles, Video Profiles, 360 VR Video
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 6b2f7533-8ce0-4134-b1ae-b3c5d15a05e6
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 2%

---

# Utilização do Panorama e do Visualizador de imagem vertical com o AEM Assets Dynamic Media{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

As melhorias do Dynamic Media Viewer no AEM 6.4 incluem a adição do Panoramic Image Viewer, Panoramic Virtual Reality Image Viewer e Vertical Image Viewer. O Visualizador panorâmico oferece uma maneira fácil de proporcionar uma experiência envolvente e imersiva da sala, propriedade, localização ou paisagem sem qualquer desenvolvimento personalizado.

>[!VIDEO](https://video.tv.adobe.com/v/24156?quality=12&learn=on)

>[!NOTE]
>
>O vídeo presume que a instância do AEM está sendo executada no modo Dynamic Media S7. [As instruções sobre a configuração do AEM com o Dynamic Media podem ser encontradas aqui.](https://helpx.adobe.com/br/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Visualizador de RV Panorâmica e Panorâmica

Uma imagem é considerada panorâmica com base em sua proporção ou em palavras-chave. Por padrão, uma imagem com proporção 2 é considerada como uma imagem panorâmica. As predefinições do visualizador de imagens panorâmicas se tornam disponíveis para uma visualização de imagem se atenderem aos critérios acima. O critério de proporção da imagem panorâmica pode ser modificado na configuração DMS7 da empresa especificando a propriedade dupla s7PanoramicAR em /conf/global/settings/cloudconfigs/dmscene7/jcr:content. As palavras-chave são armazenadas na propriedade dc:keyword do nó de metadados do ativo. Se as palavras-chave contiverem qualquer uma das seguintes combinações:

* equiretangular,
* esférico + panorâmico,
* esférico + panorama,

é considerado um ativo de Imagem panorâmica independentemente de sua taxa de proporção.

## Visualizador de imagem vertical

Com amostras horizontais, dependendo do tamanho da tela do desktop do consumidor, às vezes as amostras não ficariam visíveis até que o usuário role a página para baixo. Ao usar o visualizador de imagem vertical e ao inserir amostras verticais, ele garante que as amostras estejam visíveis, independentemente do tamanho da tela. Também maximiza o tamanho da imagem principal. Com as amostras horizontais, era necessário reservar espaço na página para garantir que elas tivessem uma alta probabilidade de serem visíveis e diminuíssem o tamanho da imagem principal. Com um layout vertical, você não precisa se preocupar com a alocação desse espaço e, portanto, pode maximizar o tamanho da imagem principal.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>Visualizador panorâmico e VR</td>
   <td>Visualizador de imagem vertical</td>
  </tr>
  <tr>
   <td>Modo de execução do Dynamic Media</td>
   <td>Somente modo Dynamic Media Scene7</td>
   <td>DMS7 e Dynamic Media</td>
  </tr>
  <tr>
   <td>Caso de uso</td>
   <td><p>O visualizador panorâmico e o visualizador de realidade virtual fornecem aos usuários uma experiência mais envolvente. Um usuário pode verificar um quarto de hotel mesmo antes de fazer uma reserva ou sair de uma propriedade de aluguel sem ter que agendar uma consulta. Um usuário pode verificar um local e muitas outras possibilidades. O foco principal aqui é fornecer ao consumidor uma experiência melhor quando visitar seu site e, eventualmente, aumentar seu índice de conversão.</p> <p> </p> </td> 
   <td><p>O Visualizador de imagem vertical ajuda a maximizar a experiência de visualização de imagens do produto para fornecer aos consumidores a melhor representação do produto, impulsionando a conversão e minimizando os retornos.</p> <p> </p> </td>
  </tr>
  <tr>
   <td>Disponível </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[Configuração do Dynamic Media no modo Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

[Configuração do Dynamic Media no modo híbrido](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>Os visualizadores panorâmicos funcionam com imagens panorâmicas e não devem ser usados com imagens normais.
