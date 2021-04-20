---
title: Imagem, amostra, rotação e conjuntos de mídia mista
description: Uma das capacidades mais úteis e eficientes do Dynamic Media Classic é o suporte para a criação de conjuntos de mídia avançada, como Imagem, Amostra, Rotação e Conjuntos de mídia mista. Saiba o que cada conjunto de mídia avançada é e como criar cada tipo no Dynamic Media Classic. Em seguida, saiba mais sobre Predefinições de conjunto de lotes, que automatizam o processo de criação de conjuntos de mídia avançada após o upload.
sub-product: dynamic-media
feature: Dynamic Media Classic, Image Sets, Mix Media Sets, Spin Sets
doc-type: tutorial
topics: sets, development, authoring, configuring
audience: all
activity: use
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1471'
ht-degree: 1%

---


# Imagem, amostra, rotação e conjuntos de mídia mista {#media-sets}

Além de imagens únicas para dimensionamento dinâmico e zoom, as coleções de conjunto do Dynamic Media Classic permitem uma experiência online mais rica. Esta seção do tutorial explorará como criar os seguintes conjuntos de mídia avançada no Dynamic Media Classic:

- Definição de imagem
- Conjunto de amostras
- Grupo de rotação
- Conjunto de mix de mídia

Também explicará como usar Predefinições de conjunto de lotes para automatizar a criação de conjuntos por meio de um upload.

## Tudo O Que Você Sempre Queria Saber Sobre Conjuntos

Ao lado do dimensionamento dinâmico básico e do zoom, os conjuntos são provavelmente o subproduto mais usado do Dynamic Media Classic. Os conjuntos são ativos essencialmente &quot;virtuais&quot; que não contêm imagens reais, mas consistem em um conjunto de relacionamentos com outras imagens e/ou vídeo. O principal apelo dos conjuntos é que são miniaplicações que estão prontas &quot;fora da prateleira&quot;. Por isso, queremos dizer que cada visualizador de conjunto contém sua própria lógica e interface para que tudo o que você precisa fazer é chamá-los no site. Além disso, eles exigem que você rastreie uma única ID de ativo por conjunto, em vez de precisar gerenciar todos os ativos e relacionamentos do membro por conta própria.

Ao criar um conjunto, esse conjunto é gerenciado como um ativo separado que deve ser marcado para publicação e publicado antes de poder ser veiculado a partir de um URL. Todos os ativos dos membros também devem ser publicados.

### Tipos de conjuntos

Saiba mais sobre os quatro tipos de conjuntos que você pode criar no Dynamic Media Classic: Imagem, amostra, rotação e conjuntos de mídia mista.

## Definição de imagem

Esse é o tipo mais comum de conjunto. Normalmente, você o usará para exibições alternativas do mesmo item. Ele consiste em várias imagens que você carrega no visualizador clicando na miniatura associada dessa imagem.

![imagem](assets/media-sets/image-set-1.jpg)

_Exemplo de um conjunto de imagens_

O URL do conjunto de imagens acima pode ser exibido como:

![imagem](assets/media-sets/image-set-url-1.png)

- Saiba mais sobre Conjuntos de imagens com o [Início rápido para Conjuntos de imagens](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sets/quick-start-image-sets.html).
- Saiba como [Criar um conjunto de imagens](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sets/creating-image-set.html#creating-an-image-set).

### Conjunto de amostras

Normalmente, esse tipo de conjunto é usado para exibir exibições coloridas do mesmo item. Consiste em pares de imagens e amostras de cores.

A principal diferença entre uma Amostra e um Conjunto de imagens é que os Conjuntos de amostras usam uma imagem diferente como uma amostra clicável, enquanto os Conjuntos de imagens usam uma versão em miniatura clicável da imagem original.

Os conjuntos de amostras não colorem imagens (um equívoco comum). As imagens estão sendo simplesmente trocadas, exatamente como em um Conjunto de imagens. As miniimagens de amostra poderiam ter sido criadas com o Photoshop, cada cor poderia ter sido fotografada separadamente, ou a ferramenta Recortar no Dynamic Media Classic poderia ter sido usada para fazer uma amostra de uma das imagens coloridas.

![imagem](assets/media-sets/image-set-2.jpg)

_Exemplo de um conjunto de amostras_

O URL do conjunto de amostras acima pode ser exibido como:

![imagem](assets/media-sets/image-set_url.png)

- Saiba mais sobre Conjuntos de amostras com o [Início rápido para Conjuntos de amostras](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html).
- Saiba como [Criar um conjunto de amostras](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html#creating-a-swatch-set).

### Grupo de rotação

Normalmente, esse conjunto é usado para mostrar uma exibição de 360 graus de um item. Como Conjuntos de amostras, os Conjuntos de rotação não usam mágica em 3D — o trabalho real é criar muitas fotos de uma imagem de todos os lados. O visualizador simplesmente permite que você alterne entre as imagens como uma animação stop-motion.

Os Conjuntos de rotação podem girar em uma direção ao longo de um único eixo ou, se criados alternadamente como um Conjunto de rotação 2D — giram em vários eixos. Por exemplo, um carro pode ser rodado enquanto todas as rodas estão no chão, e depois pode ser &quot;virado&quot; para cima e rodado nas rodas traseiras também. Para um Conjunto de rotação 2D corretamente configurado, o número de imagens por linha para cada eixo deve ser o mesmo. Em outras palavras, se você estiver girando em dois eixos, precisará de duas vezes mais imagens do que um giro de ângulo único.

![imagem](assets/media-sets/image-set-3.png)

_Exemplo de um conjunto de rotação_

O URL do conjunto de rotação acima pode aparecer como:

![imagem](assets/media-sets/spin-set.png)

- Saiba mais sobre Conjuntos de rotação com o [Início rápido para Conjuntos de rotação](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html).
- Saiba como [Criar um conjunto de rotação](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/spin-sets/creating-spin-set.html#creating-a-spin-set).

## Conjunto de mix de mídia

Este é um conjunto de combinações. Ele permite combinar qualquer um dos conjuntos anteriores, bem como adicionar vídeo, em um único visualizador. Nesse fluxo de trabalho, você cria qualquer um dos conjuntos de componentes primeiro e os reúne em um Conjunto de mídias mistas.

![imagem](assets/media-sets/image-set-4.png)

_Exemplo de um conjunto de mídia mista_

O URL do conjunto de mídia mista acima pode ser exibido como:

![imagem](assets/media-sets/image-set-url-1.png)

- Saiba mais sobre Conjuntos de mídias mistas com o [Início rápido para Conjuntos de mídias mistas](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html).

- Saiba como [Criar um conjunto de mídia mista](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html#creating-a-mixed-media-set).

Para exibir uma imagem para zoom, um conjunto ou um vídeo no seu site, você a chama em um &quot;visualizador&quot; do Dynamic Media Classic. O Dynamic Media Classic inclui visualizadores de ativos de mídia avançada, como Conjuntos de amostras, Conjuntos de rotação, vídeo e muitos outros.

Saiba mais sobre [Visualizadores do AEM Assets e Dynamic Media Classic](https://docs.adobe.com/content/help/en/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html).

## Predefinições de conjunto de lotes

Até agora, discutimos como criar conjuntos manualmente usando a função Dynamic Media Classic Build. No entanto, é possível automatizar a criação de Conjuntos de imagens e Conjuntos de rotação usando uma Predefinição de conjunto de lotes, desde que você tenha uma convenção de nomenclatura padronizada.

Cada predefinição é um conjunto de instruções autocontidas e com nome exclusivo que define como construir o conjunto usando imagens que correspondem às convenções de nomenclatura definidas. Na predefinição, primeiro defina as convenções de nomenclatura dos ativos que deseja agrupar em um conjunto. Uma predefinição de conjunto de lotes pode ser criada para fazer referência a essas imagens.

Embora seja possível criar a predefinição por conta própria (elas são encontradas em **Configuração > Configuração do aplicativo > Predefinições do conjunto de lotes** ), como prática recomendada, você deve ter sua equipe de consultoria ou o Suporte técnico configurado para você. Veja o porquê:

- As predefinições do conjunto de lotes podem ser complexas para configuração. Elas são fornecidas por expressões regulares e, a menos que você seja um desenvolvedor, essa sintaxe pode ser desconhecida ou confusa.
- Depois de criados, eles são ativados por padrão. Não há função &quot;desfazer&quot;. Se você começar a fazer o upload de milhares de imagens e sua predefinição estiver configurada incorretamente, poderá acabar com centenas ou milhares de conjuntos quebrados que devem ser encontrados e excluídos manualmente.

Uma convenção de nomenclatura simples foi sugerida anteriormente, o que seria muito fácil de criar em uma predefinição de conjunto de lotes. No entanto, como as predefinições são muito flexíveis, elas podem lidar com estratégias de nomeação complexas. Resumindo, as imagens que pertencem a um conjunto devem ser vinculadas por um nome comum — geralmente é o número SKU ou a ID do produto. No Dynamic Media Classic, você informa a ela uma convenção de nomenclatura padrão para que todas as suas imagens sejam usadas para uma predefinição, ou você pode criar várias predefinições, cada uma com regras de nomenclatura diferentes.

As predefinições do conjunto de lotes são aplicadas somente no upload; elas não podem ser executadas depois que as imagens são carregadas. Portanto, é importante planejar sua convenção de nomenclatura e obter uma predefinição criada antes de iniciar o carregamento de todas as imagens.

Depois que as predefinições forem criadas, o Administrador da empresa poderá escolher se estão ativas ou inativas. Ativo significa que elas aparecerão na página de upload em **Opções de trabalho**, enquanto as predefinições inativas permanecerão ocultas.

Saiba como [Criar uma predefinição de conjunto de lotes](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#creating-a-batch-set-preset).

### Uso das predefinições do conjunto de lotes no upload

Veja como usar as Predefinições de conjunto de lotes no upload depois que elas tiverem sido criadas:

1. Clique em **Fazer upload** e escolha **Do desktop** ou **Via FTP**.
2. Clique em **Opções de trabalho**.
3. Abra a opção **Predefinições do conjunto de lotes** e marque ou desmarque a predefinição para usá-la com o upload.
4. Depois que o upload terminar, procure os conjuntos concluídos na sua pasta.

Saiba mais sobre [Predefinições de Conjuntos de Lotes](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#batch-set-presets).
