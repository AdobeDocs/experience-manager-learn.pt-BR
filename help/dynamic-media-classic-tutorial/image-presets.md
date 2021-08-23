---
title: Predefinições de imagem
description: As predefinições de imagens no Dynamic Media Classic contêm todas as configurações necessárias para criar uma imagem em um tamanho, formato, qualidade e nitidez específicos. As predefinições de imagens são um componente essencial do dimensionamento dinâmico. Ao observar um URL no Dynamic Media Classic, é possível ver se uma predefinição de imagem está em uso. Saiba mais sobre Predefinições de imagem, por que elas são tão úteis e como criar uma.
sub-product: dynamic-media
feature: Dynamic Media Classic, predefinições de imagem
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Gerenciamento de conteúdo
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '706'
ht-degree: 1%

---


# Predefinições de imagem {#image-presets}

Uma predefinição de imagem é essencialmente uma receita que contém todas as configurações necessárias para criar uma imagem em um tamanho, formato, qualidade e nitidez específicos. As predefinições de imagens são um componente essencial do dimensionamento dinâmico.

Se você observar os URLs de qualquer cliente do Dynamic Media Classic, provavelmente verá uma predefinição de imagem em uso. Basta procurar $name$ no final do URL (com qualquer palavra ou palavra substituída por nome).

As predefinições de imagem reduzem o URL, de modo que, em vez de escrever várias instruções de Exibição de imagem por solicitação, você pode gravar uma única predefinição de imagem. Por exemplo, esses dois URLs produzem a mesma imagem JPEG 300 x 300 com nitidez, mas o segundo usa uma predefinição de imagem:

![imagem](assets/image-presets/image-preset-2.png)

O verdadeiro valor das Predefinições de Imagem é que qualquer Administrador da Empresa pode atualizar a definição dessa Predefinição de Imagem e afetar todas as imagens usando esse formato — sem alterar nenhum código da Web. Você verá os resultados de qualquer alteração em uma Predefinição de imagem depois que o cache do URL for limpo.

>[!IMPORTANT]
>
>Ao redimensionar uma imagem, a proporção do aspecto, a proporção da largura da imagem com sua altura, deve ser sempre mantida proporcional para que a imagem não fique distorcida.

Uma predefinição de imagem tem um cifrão ($) em ambos os lados do nome e segue o ponto de interrogação (?) separador.

>[!TIP]
>
>Crie uma predefinição de imagem por tamanho de imagem exclusivo no site. Por exemplo, se você precisar de uma imagem 350 X 350 para a página de detalhes do produto, uma imagem 120 X 120 para as páginas de navegação/pesquisa e uma imagem 90 X 90 para um item de venda cruzada/em destaque, você precisará de três Predefinições de imagem, quer você tenha 500 imagens ou 500.000.

- Saiba mais sobre [Predefinições de imagem](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html).
- Saiba como [Criar uma predefinição de imagem](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset).

## Predefinições e nitidez da imagem

As predefinições de imagens normalmente redimensionam uma imagem e, sempre que você redimensionar uma imagem do tamanho original, deve adicionar nitidez. Isso ocorre porque o redimensionamento faz com que muitos pixels se mesclem e se misturem em um espaço menor, fazendo com que a imagem pareça macia e borrada. A nitidez aumenta o contraste das bordas e as áreas de alto contraste em uma imagem.

Esperamos que as imagens de alta resolução carregadas no Dynamic Media Classic não precisem de nitidez quando visualizadas em tamanho real — quando ampliadas. No entanto, em qualquer tamanho menor, a nitidez geralmente é desejável.

>[!TIP]
>
>Sempre ajuste a nitidez ao redimensionar imagens! Isso significa que você precisará adicionar nitidez a cada Predefinição de imagem (e Predefinição do visualizador, que discutiremos posteriormente).
>
>Se suas imagens não parecem bem, é possível que elas precisem de nitidez ou talvez a qualidade tenha sido ruim para começar.

A nitidez a ser adicionada é totalmente subjetiva. Algumas pessoas gostam de imagens mais suaves, enquanto outras gostam muito agudas. É fácil aprimorar uma imagem executando uma combinação de filtros de nitidez em uma imagem. No entanto, também é fácil ir além e tornar uma imagem mais nítida.

O gráfico a seguir mostra três níveis de nitidez. Da direita para a esquerda você não tem nitidez, apenas a quantidade certa, e muito.

![imagem](assets/image-presets/image-presets-1.jpg)

O Dynamic Media Classic permite três tipos de nitidez: Nitidez simples, modo de reamostra e Tirar nitidez da máscara.

Saiba mais sobre as [Opções de nitidez do Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image).

## Recursos adicionais

[Guia de predefinição de imagem](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf). Configurações a serem usadas para otimizar a qualidade da imagem e a velocidade de carregamento.

[A Imagem É Tudo Parte 2: Nunca é apenas um desfoque — Qualidade versus Velocidade](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). Uma publicação do blog discute o uso de Predefinições de imagem para fornecer imagens de alta qualidade e de carregamento rápido.

[A Imagem É Todos Os Webinars](https://dynamicmediaseries2019.enterprise.adobeevents.com/). Links para gravações de todos os três webinars na série _Imagem é tudo_. [O Webinar 2](https://seminars.adobeconnect.com/p6lqaotpjnd3) discute as Predefinições de imagem.
