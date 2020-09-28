---
title: Predefinições de imagem
description: As predefinições de imagem no Dynamic Media Classic contêm todas as configurações necessárias para criar uma imagem com um tamanho, formato, qualidade e nitidez específicos. As predefinições de imagens são um componente chave do dimensionamento dinâmico. Ao observar um URL no Dynamic Media Classic, você pode ver facilmente se uma predefinição de imagem está em uso. Saiba mais sobre as predefinições de imagens, por que são tão úteis e como criar uma.
sub-product: dynamic-media
feature: image-presets
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
translation-type: tm+mt
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '705'
ht-degree: 1%

---


# Predefinições de imagem {#image-presets}

Uma predefinição de imagem é essencialmente uma fórmula que contém todas as configurações necessárias para criar uma imagem em um tamanho, formato, qualidade e nitidez específicos. As predefinições de imagens são um componente chave do dimensionamento dinâmico.

Se você observar os URLs de qualquer cliente do Dynamic Media Classic, provavelmente verá uma predefinição de imagem em uso. Basta procurar $name$ no final do URL (com qualquer palavra ou palavras substituídas pelo nome).

As predefinições de imagem reduzem o URL, de modo que, em vez de escrever várias instruções de disponibilização de imagem por solicitação, você pode gravar uma única predefinição de imagem. Por exemplo, esses dois URLs produzem a mesma imagem JPEG 300 x 300 com nitidez, mas o segundo usa uma predefinição de imagem:

![imagem](assets/image-presets/image-preset-2.png)

O valor real das predefinições de imagens é que qualquer administrador de Empresas pode atualizar a definição dessa predefinição de imagem e afetar todas as imagens usando esse formato. sem alterar qualquer código da Web. Você verá os resultados de qualquer alteração em uma predefinição de imagem depois que o cache do URL for limpo.

>[!IMPORTANT]
>
>Ao redimensionar uma imagem, a proporção entre a largura e a altura da imagem deve ser sempre mantida proporcional para que a imagem não fique distorcida.

Uma predefinição de imagem tem um cifrão ($) em ambos os lados do seu nome e segue o ponto de interrogação (?) separador.

>[!TIP]
>
>Crie uma predefinição de imagem por tamanho de imagem exclusivo no site. Por exemplo, se você precisar de uma imagem de 350 X 350 para a página de detalhes do produto, uma imagem de 120 X 120 para as páginas de navegação/pesquisa e uma imagem de 90 X 90 para um item de venda cruzada/destaque, você precisará de três predefinições de imagens, quer você tenha 500 imagens ou 500.000.

- Saiba mais sobre as predefinições [de](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sizing/setting-image-presets.html)imagens.
- Saiba como [criar uma predefinição](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset)de imagem.

## Predefinições e nitidez de imagem

As predefinições de imagens normalmente redimensionam uma imagem e, a qualquer momento que você redimensiona uma imagem a partir de seu tamanho original, deve adicionar nitidez. Isso ocorre porque o redimensionamento faz com que muitos pixels se mesclem e se mesclem em um espaço menor, fazendo com que a imagem pareça suave e indefinida. A nitidez aumenta o contraste das bordas e as áreas de alto contraste em uma imagem.

Esperamos que as imagens de alta resolução carregadas no Dynamic Media Classic não precisem de nitidez quando visualizadas em tamanho normal. quando se aproximou. No entanto, em qualquer tamanho menor, geralmente é desejável a nitidez.

>[!TIP]
>
>Sempre aumentar a nitidez ao redimensionar imagens! Isso significa que você precisará adicionar nitidez a cada predefinição de imagem (e predefinição do visualizador, que discutiremos mais tarde).
>
>Se as suas imagens não parecem boas, provavelmente são porque precisam de ser afiadas ou talvez a qualidade tenha sido ruim para começar.

A quantidade de nitidez a adicionar é totalmente subjetiva. Algumas pessoas gostam de imagens mais suaves, enquanto outras gostam muito nítidas. É fácil aprimorar uma imagem executando uma combinação de filtros de nitidez em uma imagem. No entanto, também é fácil deslocar-se borda fora e aumentar a nitidez de uma imagem.

O gráfico a seguir mostra três níveis de nitidez. Da direita para a esquerda você não tem nitidez, apenas a quantidade certa, e muito.

![imagem](assets/image-presets/image-presets-1.jpg)

O Dynamic Media Classic permite três tipos de nitidez: Nitidez simples, Modo de nova amostra e Máscara de nitidez.

Saiba mais sobre as Opções [de nitidez do](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image)Dynamic Media Classic.

## Recursos adicionais

[Guia](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf)predefinido de imagem. Configurações a serem usadas para otimizar a qualidade da imagem e a velocidade de carregamento.

[A Imagem É Tudo Parte 2: Nunca é apenas um desfoque. Qualidade versus Velocidade](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). Uma postagem do blog discute o uso das predefinições de imagens para fornecer imagens de alta qualidade e de carregamento rápido.

[A Imagem É Tudo Webinars](https://dynamicmediaseries2019.enterprise.adobeevents.com/). Links para gravações de todos os três webinars da série _Imagem é tudo_ . [O Webinar 2](https://seminars.adobeconnect.com/p6lqaotpjnd3) discute as predefinições de imagens.
