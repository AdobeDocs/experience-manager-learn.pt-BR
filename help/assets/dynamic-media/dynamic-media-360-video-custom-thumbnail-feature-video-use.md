---
title: Uso de vídeos do Dynamic Media 360 e miniatura de vídeo personalizada com o AEM Assets
description: As melhorias do Visualizador do Dynamic Media no AEM 6.5 incluem a adição de suporte para renderização de vídeo 360, visualizadores de mídia 360 (video360Social e video360VR) e a capacidade de selecionar miniaturas de vídeo personalizadas.
feature: Video Profiles
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 4ee0b68f-3897-4104-8615-9de8dbb8f327
duration: 656
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 3%

---

# Uso de vídeos do Dynamic Media 360 e miniatura de vídeo personalizada com o AEM Assets

As melhorias do Visualizador do Dynamic Media no AEM 6.5 incluem a adição de suporte para renderização de vídeo 360, visualizadores de mídia 360 (video360Social e video360VR) e a capacidade de selecionar miniaturas de vídeo personalizadas.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=12&learn=on)

>[!NOTE]
>
>O vídeo presume que a instância do AEM está sendo executada no modo Dynamic Media S7.  [As instruções sobre como configurar o AEM com Dynamic Media podem ser encontradas aqui](https://helpx.adobe.com/br/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Ao carregar um vídeo, por padrão, o Dynamic Media processa a sequência de imagens como um vídeo 360, se ela tiver uma proporção largura/altura de 2:1. ou seja, a relação largura/altura é de 2:1.

>[!NOTE]
>
>Os componentes do Dynamic Media 360 Media são compatíveis apenas com 360 vídeos.

## Vídeos do Dynamic Media 360

Vídeos de 360 graus, também conhecidos como vídeos esféricos, são gravações de vídeo em que uma visualização em todas as direções é gravada ao mesmo tempo, filmada usando uma câmera omnidireccional ou uma coleção de câmeras. Durante a reprodução em uma tela plana, o usuário tem controle da direção de exibição e a reprodução em dispositivos móveis normalmente aproveita o controle do giroscópio integrado.  Ele permite expandir além dos limites de uma única fotografia. Os profissionais de marketing podem fornecer aos usuários uma experiência envolvente com a ajuda de 360 vídeos.  Vamos começar. O critério de proporção da imagem panorâmica pode ser modificado na configuração DMS7 da empresa especificando a propriedade dupla s7PanoramicAR em /conf/global/settings/cloudconfigs/dmscene7/jcr:content.

## Vídeos do Dynamic Media 360

O Dynamic Media Video agora é compatível com a capacidade de selecionar uma miniatura personalizada para o seu vídeo. Um usuário pode selecionar um ativo existente do AEM Assets ou selecionar um quadro de vídeo como miniatura.

## Visualizadores do Dynamic 360 Media

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Visualizador de vídeo360Social**</td>
      <td>**Visualizador de Video360VR**</td>
   </tr>
   <tr>
      <td>Modo de execução do Dynamic Media</td>
      <td>Somente modo Scene7 do Dynamic Media</td>
      <td>Somente modo Scene7 do Dynamic Media<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>Caso de uso</td>
      <td>
         <p>Para sites e dispositivos que não aceitam giroscópio</p>
         <p> </p>
      </td>
      <td>
         <p>Fornece uma experiência de Realidade Virtual para um dispositivo com suporte a giroscópio </p>
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
      <td>Navegação do ponto de vista</td>
      <td>
         <ul>
            <li>Arrastar com o mouse (em sistemas desktop)</li>
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

[Configurando o Dynamic Media no modo Scene7](https://helpx.adobe.com/br/experience-manager/6-5/assets/using/config-dms7.html)
