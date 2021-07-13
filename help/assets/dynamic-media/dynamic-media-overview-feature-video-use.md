---
title: Visão geral do Dynamic Media com AEM Assets
description: Esta série de vídeo oferece uma visão geral de como o conteúdo de mídia é gerenciado e acessado usando o Adobe Experience Manager Dynamic Media como um serviço de fornecimento de conteúdo. O Dynamic Media permite gerenciar e publicar experiências digitais dinâmicas — um recurso exclusivo dos Ativos Experience Manager. Nossa estrutura e conjunto de componentes permitem que os profissionais de marketing personalizem e forneçam experiências interativas de multimídia em todos os dispositivos.
sub-product: dynamic-media
feature: Recorte inteligente, perfis de vídeo, perfis de imagem, predefinições do visualizador, vídeo 360 VR, conjuntos de imagens, conjuntos de rotação
version: 6.3, 6.4, 6.5
topic: Gerenciamento de conteúdo
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '888'
ht-degree: 0%

---


# Uso do Dynamic Media com AEM Assets {#understanding-aem-dynamic-media}

Esta série de vídeos de várias partes oferece uma visão geral de como o conteúdo de mídia é gerenciado e acessado usando o Adobe Experience Manager Dynamic Media como um serviço de fornecimento de conteúdo. O Dynamic Media permite gerenciar e publicar experiências digitais dinâmicas — um recurso exclusivo dos Ativos Experience Manager. Nossa estrutura e conjunto de componentes permitem que os profissionais de marketing personalizem e forneçam experiências interativas de multimídia em todos os dispositivos.

## Visão geral da Dynamic Media

>[!VIDEO](https://video.tv.adobe.com/v/27144/?quality=9&learn=on)

>[!NOTE]
>
>Funcionalidade demonstrada aqui está disponível com o modo de execução Dynamic Media DMS7, nosso modo de execução atualmente suportado, não necessariamente o modo de execução DMHybrid, que o DMS7 substituiu.

Este vídeo descreve como o conteúdo de mídia é gerenciado e acessado usando o Adobe Experience Manager Dynamic Media como um serviço de fornecimento de conteúdo. A Dynamic Media opera em uma única metodologia de Ativos Principais, na qual você faz upload de um ativo de imagem ou ativo de vídeo que pode ser solicitado a atender a um conjunto ilimitado de variações consumíveis necessárias ou representações derivadas. Incluído:

* Explicação do ativo Principal único para o produto de URL entregue
* Opções de processamento de imagens
* Opções do visualizador de conteúdo
* Copiar URLs para imagens e visualizadores responsivos
* Variações de recorte inteligente para URLs

## Dynamic Media com AEM Sites e qualquer outro CMS

>[!VIDEO](https://video.tv.adobe.com/v/27145/?quality=9&learn=on)

>[!NOTE]
>
>Funcionalidade demonstrada aqui está disponível com o modo de execução Dynamic Media DMS7, nosso modo de execução atualmente suportado, não necessariamente o modo de execução DMHybrid, que o DMS7 substituiu. Este vídeo faz referência aos conceitos descritos na Parte 1 do vídeo (Visão geral do Dynamic Media).

Este vídeo descreve como o conteúdo de mídia é gerenciado no Adobe Experience Manager Dynamic Media e pode ser facilmente usado no AEM Sites, com um componente, para ser simples e automaticamente cortado de forma a otimizar com base na largura responsiva da página. Crie facilmente banner de imagem interativo e gere cópias de URL para usar em qualquer Sistema de gerenciamento de conteúdo.

* Flexibilidade do componente Dynamic Media AEM Sites
* Download local com predefinições de imagem
* Criação e publicação de banner interativo

## Dynamic Media para criar a coleção de mídia mista e o visualizador personalizado

>[!VIDEO](https://video.tv.adobe.com/v/27146/?quality=9&learn=on)

>[!NOTE]
>
>Funcionalidade demonstrada aqui está disponível com o modo de execução Dynamic Media DMS7, nosso modo de execução atualmente suportado, não necessariamente o modo de execução DMHybrid, que o DMS7 substituiu. Este vídeo faz referência aos conceitos descritos na Parte 1 do vídeo (Visão geral do Dynamic Media).

Este vídeo descreve o processo de criação simples de uma coleção de ativos de mídia do visualizador de Mídia mista, incluindo um conjunto de rotação, um vídeo e uma coleção de imagens de produtos. Adicione conteúdo ao Conjunto de mídias mistas e crie um visualizador personalizado para escolher no componente final Copiar URL ou AEM Sites .

* Criar conjunto de rotação a partir de fotos de produto apropriadas
* Fazer upload e codificar o vídeo principal para o Dynamic Media Video
* Criar conjunto de mídias mistas a partir do conjunto de rotação, vídeo e fotos
* Editar e usar o visualizador de Mídia mista personalizado

## Predefinições de imagem do Dynamic Media

>[!VIDEO](https://video.tv.adobe.com/v/27320/?quality=9&learn=on)

Este vídeo descreve como as Predefinições de imagem são criadas e o que é uma predefinição de imagem, um redutor de URL para uma série de argumentos do Servidor de imagem que operam em uma imagem sempre que um URL a solicita. Saiba mais sobre técnicas valiosas para estender e editar Predefinições de imagem.

* Encurtador de predefinições de imagens ocultando a coleção de comandos explícitos do servidor de imagens
* Use apenas uma dimensão de pixel - largura OU altura - para estar em conformidade com novas imagens redimensionadas sem preenchimento
* Sempre usar nitidez
* Campo do modificador de URL para adicionar comandos extras para redimensionar a predefinição de imagem

## Modificadores de URL avançados do Dynamic Media

>[!VIDEO](https://video.tv.adobe.com/v/27319/?quality=9&learn=on)

Este vídeo descreve ir além do redimensionamento de imagens para aproveitar os recursos do próprio arquivo de origem - transparência de plano de fundo, traçados de recorte e recortes e texto como variáveis - com os modificadores de URL da Dynamic Media.

* Uso de modificadores de URL no campo Dynamic Media Modifier
* Alteração da cor do fundo em imagens com transparência
* Recortar para um caminho de imagem
* Recortar em um caminho de imagem
* Criação de um modelo de texto a partir de um arquivo Photoshop

## Dynamic Media Controlando o tamanho do arquivo JPEG em Kilobytes

>[!VIDEO](https://video.tv.adobe.com/v/27404/?quality=9&learn=on)


>[!NOTE]
>
>A QUALIDADE da imagem é medida em porcentagens da compressão inversa, onde a qualidade de 100% é menos compactada, resultando em imagens de alta qualidade, mas em tamanhos de arquivo relativamente grandes. A compactação Jpeg é um esquema de compactação com perdas no qual as configurações de compactação determinam a qualidade da imagem e o tamanho do arquivo.

Equilibre a qualidade da imagem jpeg em relação ao tamanho de arquivo resultante (em kilobytes) para melhorar a velocidade de carregamento da página, usando 2 comandos para ajustar as configurações de compactação jpeg. O QLT define a qualidade da imagem ajustando as configurações de qualidade da compactação jpeg. O comando Tamanho JPEG permite designar qual tamanho de arquivo precisa ser obtido usando compactação.

## Adicionar legendas ocultas CC ao vídeo do Dynamic Media

>[!VIDEO](https://video.tv.adobe.com/v/28074/?quality=9&learn=on)

Adicione facilmente as Legendas ocultas ao vídeo do Dynamic Media ao anexar o URL da cópia para apontar para um documento de arquivo adicional de Legendas ocultas, um arquivo de vídeo Web.VTT, contendo as informações CC de qualquer vídeo.

## Uso da nitidez da imagem com o AEM Dynamic Media

Este vídeo aborda o motivo pelo qual a nitidez de uma imagem é essencial para manter a fidelidade da imagem e como usar configurações avançadas para criar a imagem perfeita.

>[!VIDEO](https://demos-pub.assetsadobe.com/etc/dam/viewers/s7viewers/html5/VideoViewer.html?asset=%2Fcontent%2Fdam%2Fdm-public-facing-upgrade-portal-video%2F04_DynamicImagery_AdvancedSettings_071917_BH.mp4&amp;config=/etc/dam/presets/viewer/Video_social&amp;serverUrl=https%3A%2F%2Fadobedemo62-h.assetsadobe.com%2Fis%2Fimage%2F&amp;contenturl=%2F&amp;config2=/etc/dam/presets/analytics&amp;videoserverurl=https://gateway-na.assetsadobe.com/DMGateway/public/demoCo&amp;posterimage=/content/dam/dm-public-facing-upgrade-portal-video/04_DynamicImagery_AdvancedSettings_071917_BH.mp4)
