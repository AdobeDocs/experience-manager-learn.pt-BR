---
title: Predefinições de imagem
description: As Predefinições de imagem no Dynamic Media Classic contêm todas as configurações necessárias para criar uma imagem em um tamanho, formato, qualidade e nitidez específicos. As predefinições de imagem são um componente importante do dimensionamento dinâmico. Ao observar um URL no Dynamic Media Classic, é possível ver facilmente se uma Predefinição de imagem está em uso. Saiba mais sobre as Predefinições de imagem, por que elas são tão úteis e como criá-las.
feature: Dynamic Media Classic, Image Presets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: e472db7c-ac3f-4f66-85af-5a4c68ba609e
duration: 127
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 1%

---

# Predefinições de imagem {#image-presets}

Uma Predefinição de imagem é essencialmente uma fórmula que contém todas as configurações necessárias para criar uma imagem em um tamanho, formato, qualidade e nitidez específicos. As predefinições de imagem são um componente importante do dimensionamento dinâmico.

Se você observar os URLs de praticamente qualquer cliente do Dynamic Media Classic, provavelmente verá uma Predefinição de imagem em uso. Basta procurar por $name$ no final do URL (com qualquer palavra ou palavras substituídas pelo nome).

As Predefinições de imagem encurtam o URL, de modo que, em vez de escrever várias instruções no Servidor de imagens por solicitação, você possa escrever uma única Predefinição de imagem. Por exemplo, esses dois URLs produzem a mesma imagem de 300 x 300 JPEG com nitidez, mas o segundo usa uma Predefinição de imagem:

![imagem](assets/image-presets/image-preset-2.png)

O verdadeiro valor das Predefinições de imagem é que qualquer Administrador da empresa pode atualizar a definição dessa Predefinição de imagem e afetar todas as imagens usando esse formato, sem alterar qualquer código da Web. Você verá os resultados de qualquer alteração em uma Predefinição de imagem depois que o cache do URL for limpo.

>[!IMPORTANT]
>
>Ao redimensionar uma imagem, a proporção, a proporção da largura da imagem em relação à sua altura, deve ser sempre mantida proporcional para que a imagem não seja distorcida.

Uma predefinição de imagem tem um cifrão ($) em ambos os lados do nome e segue o ponto de interrogação (?) separador.

>[!TIP]
>
>Crie uma predefinição de imagem por tamanho de imagem exclusivo no site. Por exemplo, se você precisar de uma imagem de 350 X 350 para a página de detalhes do produto, uma imagem de 120 X 120 para as páginas de navegação/pesquisa e uma imagem de 90 X 90 para um item de venda cruzada/em destaque, você precisará de três Predefinições de imagem, independentemente de ter 500 imagens ou 500.000.

- Saiba mais sobre [Predefinições de imagem](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html).
- Saiba como [Criar uma Predefinição de Imagem](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset).

## Predefinições e nitidez da imagem

As Predefinições de imagem normalmente redimensionam uma imagem e, sempre que você redimensionar uma imagem de seu tamanho original, deverá adicionar nitidez. Isso ocorre porque o redimensionamento faz com que muitos pixels se mesclem e se misturem em um espaço menor, fazendo com que a imagem pareça suave e borrada. A nitidez aumenta o contraste das bordas e das áreas de alto contraste em uma imagem.

Esperamos que as imagens de alta resolução carregadas no Dynamic Media Classic não precisem de nitidez quando visualizadas em tamanho normal — quando ampliadas. No entanto, em qualquer tamanho menor, a nitidez é geralmente desejável.

>[!TIP]
>
>Sempre ajuste a nitidez ao redimensionar imagens! Isso significa que será necessário adicionar nitidez a cada Predefinição de imagem (e Predefinição do visualizador, que será discutida posteriormente).
>
>Se suas imagens não parecem boas, provavelmente é porque elas precisam de nitidez ou talvez a qualidade tenha sido ruim para começar.

Quanta nitidez adicionar é inteiramente subjetiva. Algumas pessoas gostam de imagens mais macias, enquanto outras gostam muito nítidas. É fácil aprimorar uma imagem executando uma combinação de filtros de nitidez em uma imagem. No entanto, também é fácil exagerar e nitidez excessiva de uma imagem.

O gráfico a seguir mostra três níveis de nitidez. Da direita para a esquerda você não tem nitidez, apenas a quantidade certa e demais.

![imagem](assets/image-presets/image-presets-1.jpg)

O Dynamic Media Classic permite três tipos de nitidez: Nitidez simples, Modo de reamostragem e Tirar nitidez da máscara.

Saiba mais sobre [Opções de nitidez do Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image).

## Recursos adicionais

[Guia de predefinição de imagem](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf). Configurações a serem usadas para otimizar a qualidade da imagem e a velocidade de carregamento.

[A Imagem É Tudo Parte 2: Nunca É Apenas Um Desfoque — Qualidade Versus Velocidade](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). Uma publicação do blog que discute o uso de predefinições de imagem para fornecer imagens de alta qualidade e carregamento rápido.
