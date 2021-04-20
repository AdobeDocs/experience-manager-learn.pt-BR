---
title: Uso de vídeos do Dynamic Media 360 e miniatura de vídeo personalizado com o AEM Assets
description: Os aprimoramentos do Visualizador do Dynamic Media no AEM 6.5 incluem a adição de suporte para 360 renderização de vídeo, 360 visualizadores de mídia (vídeo360Social e vídeo360VR) e a capacidade de selecionar miniaturas de vídeo personalizadas.
sub-product: dynamic-media
feature: Video Profiles
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 5%

---


# Uso de vídeos do Dynamic Media 360 e miniatura de vídeo personalizado com o AEM Assets

Os aprimoramentos do Visualizador do Dynamic Media no AEM 6.5 incluem a adição de suporte para 360 renderização de vídeo, 360 visualizadores de mídia (vídeo360Social e vídeo360VR) e a capacidade de selecionar miniaturas de vídeo personalizadas.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>O Vídeo assume que a instância do AEM está sendo executada no modo Dynamic Media S7.  [Instruções sobre como configurar o AEM com o Dynamic Media podem ser encontradas aqui](https://helpx.adobe.com/br/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Ao fazer upload de um vídeo, por padrão, o Dynamic Media processa a gravação como um vídeo 360, se ela tiver uma proporção de aspecto de 2:1. ou seja, a relação largura/altura é de 2:1.

>[!NOTE]
>
>Os componentes de Mídia dinâmica 360 suportam apenas 360 vídeos.

## Vídeos do Dynamic Media 360

Vídeos de 360 graus, também conhecidos como vídeos esféricos, são gravações de vídeo em que uma exibição em todas as direções é gravada ao mesmo tempo, fotografada com uma câmera onidirecional ou com uma coleção de câmeras. Durante a reprodução em uma tela plana, o usuário tem o controle da direção de exibição e a reprodução em dispositivos móveis geralmente usam o controle de giroscópio integrado.  Permite que você se expanda além dos limites da fotografia única. Os profissionais de marketing podem fornecer aos usuários uma experiência envolvente com a ajuda de 360 vídeos.  Vamos começar. O critério de proporção da imagem panorâmica pode ser modificado na configuração do DMS7 da empresa, especificando a propriedade dupla s7PanorâmicaAR em /conf/global/settings/cloudconfigs/dmscene7/jcr:content.

## Vídeos do Dynamic Media 360

O vídeo do Dynamic Media agora oferece suporte à capacidade de selecionar uma miniatura personalizada para o vídeo. Um usuário pode selecionar um ativo existente nos ativos AEM ou selecionar um quadro de vídeo como miniatura.

## Visualizadores do Dynamic 360 Media

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Vídeo360Visualizador do Social**</td>
      <td>**Visualizador de vídeo 360VR**</td>
   </tr>
   <tr>
      <td>Modo de execução do Dynamic Media</td>
      <td>Somente Modo do Dynamic Media Scene7</td>
      <td>Somente Modo do Dynamic Media Scene7<br>
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
         <p>Fornece uma experiência de Realidade Virtual para um dispositivo compatível com o giroscópio </p>
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
      <td>Navegação de ponto de vista</td>
      <td>
         <ul>
            <li>Arrastar o mouse (em sistemas de desktop)</li>
            <li>Deslizar (dispositivos de toque)</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>As opções de arrastar e mouse estão desativadas</li>
            <li>Usa giroscópio integrado</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>Reprodutor HTML5</td>
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
