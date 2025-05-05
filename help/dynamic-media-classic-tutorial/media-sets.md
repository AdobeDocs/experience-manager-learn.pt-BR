---
title: Imagem, Amostra, Rotação e Conjuntos de mídia mista
description: Uma das capacidades mais úteis e eficientes do Dynamic Media Classic é o suporte à criação de conjuntos de mídia avançada como Imagem, Amostra, Rotação e Conjuntos de mídia mista. Saiba o que é cada conjunto de mídia avançada e como criar cada tipo no Dynamic Media Classic. Saiba mais sobre Predefinições de conjunto de lotes, que automatizam o processo de criação do conjunto de mídia avançada após o upload.
feature: Dynamic Media Classic, Image Sets, Mixed Media Sets, Spin Sets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 45c86ff2-d991-46a7-a8d1-25c9fec142d9
duration: 280
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1359'
ht-degree: 1%

---

# Imagem, Amostra, Rotação e Conjuntos de mídia mista {#media-sets}

Indo além das imagens únicas para dimensionamento e zoom dinâmicos, as coleções definidas pelo Dynamic Media Classic permitem uma experiência online mais rica. Esta seção do tutorial explorará como criar os seguintes conjuntos de mídia avançada no Dynamic Media Classic:

- Definição de imagem
- Conjunto de amostras
- Grupo de rotação
- Conjunto de mix de mídia

Ele também explicará como usar Predefinições de conjunto de lotes para automatizar a criação de conjuntos por meio de um upload.

## Tudo O Que Você Sempre Quis Saber Sobre Conjuntos

Ao lado do dimensionamento dinâmico básico e do zoom, os conjuntos são provavelmente o subproduto Dynamic Media Classic mais usado. Os conjuntos são essencialmente ativos &quot;virtuais&quot; que não contêm imagens reais, mas consistem em um conjunto de relacionamentos com outras imagens e/ou vídeos. O principal apelo dos conjuntos é que eles são miniaplicações que estão prontas &quot;de prateleira&quot;. Significa que cada visualizador de conjunto contém sua própria lógica e interface, de modo que tudo o que você precisa fazer é chamar no site. Além disso, eles exigem que você rastreie apenas uma única ID de ativo por conjunto, em vez de precisar gerenciar todos os ativos e relacionamentos do membro por conta própria.

Ao criar um conjunto, ele é gerenciado como um ativo separado que deve ser marcado para publicação e publicado antes de ser disponibilizado a partir de um URL. Todos os ativos do membro também devem ser publicados.

### Tipos de conjuntos

Saiba mais sobre os quatro tipos de conjuntos que você pode criar no Dynamic Media Classic: Imagem, Amostra, Rotação e Conjuntos de mídia mista.

## Definição de imagem

Esse é o tipo mais comum de conjunto. Normalmente, você o usará para exibições alternativas do mesmo item. Consiste em várias imagens carregadas no visualizador clicando na miniatura associada dessa imagem.

![imagem](assets/media-sets/image-set-1.jpg)

_Exemplo de um conjunto de imagens_

O URL para o Conjunto de imagens acima pode aparecer como:

![imagem](assets/media-sets/image-set-url-1.png)

- Saiba mais sobre Conjuntos de imagens com o [Início rápido para Conjuntos de imagens](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/quick-start-image-sets.html?lang=pt-BR).
- Saiba como [Criar um Conjunto de Imagens](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/creating-image-set.html?lang=pt-BR#creating-an-image-set).

### Conjunto de amostras

Normalmente, esse tipo de conjunto é usado para exibir exibições coloridas do mesmo item. Ele consiste em pares de imagens e amostras de cores.

A principal diferença entre uma Amostra e um Conjunto de imagens é que os Conjuntos de amostras usam uma imagem diferente como uma amostra clicável, enquanto os Conjuntos de imagens usam uma versão em miniatura e em miniatura da imagem original.

Os Conjuntos de amostras não colorem imagens (um equívoco comum). As imagens estão simplesmente sendo trocadas, exatamente como em um Conjunto de imagens. As miniimagens de amostra poderiam ter sido criadas usando o Photoshop, cada cor poderia ter sido fotografada separadamente ou a ferramenta Corte demarcado no Dynamic Media Classic poderia ter sido usada para criar uma amostra de uma das imagens coloridas.

![imagem](assets/media-sets/image-set-2.jpg)

_Exemplo de um Conjunto de Amostras_

O URL do Conjunto de amostras acima pode aparecer como:

![imagem](assets/media-sets/image-set_url.png)

- Saiba mais sobre Conjuntos de amostras com o [Início rápido para Conjuntos de amostras](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html?lang=pt-BR).
- Saiba como [Criar um conjunto de amostras](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html?lang=pt-BR#creating-a-swatch-set).

### Grupo de rotação

Normalmente, esse conjunto é usado para mostrar uma exibição de 360 graus de um item. Como os Conjuntos de amostras, os Conjuntos de rotação não usam mágica 3D — o trabalho real é criar muitas fotos de uma imagem de todos os lados. O visualizador simplesmente permite alternar entre as imagens, como uma animação em stop motion.

Os Conjuntos de rotação podem girar em uma direção ao longo de um único eixo ou, se alternadamente criados como um Conjunto de rotação 2D, girar em vários eixos. Por exemplo, um carro pode ser girado enquanto todas as rodas estão no chão, e então pode ser &quot;virado&quot; para cima e girado em suas rodas traseiras também. Para um Conjunto de rotação 2D corretamente configurado, o número de imagens por linha para cada eixo deve ser o mesmo. Em outras palavras, se você estiver girando em dois eixos, precisará do dobro de imagens como uma rotação com um único ângulo.

![imagem](assets/media-sets/image-set-3.png)

_Exemplo de um grupo de rotação_

O URL do Spin Set acima pode aparecer como:

![imagem](assets/media-sets/spin-set.png)

- Saiba mais sobre Conjuntos de rotação com o [Início rápido para Conjuntos de rotação](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html?lang=pt-BR).
- Saiba como [Criar um conjunto de rotação](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/creating-spin-set.html?lang=pt-BR#creating-a-spin-set).

## Conjunto de mix de mídia

Este é um conjunto de combinações. Ele permite combinar qualquer um dos conjuntos anteriores, bem como adicionar vídeo, em um único visualizador. Nesse fluxo de trabalho, primeiro você cria qualquer um dos conjuntos de componentes e, em seguida, os reúne em um Conjunto de mídias mistas.

![imagem](assets/media-sets/image-set-4.png)

_Exemplo de conjunto de mídia mista_

O URL para o Conjunto de mídias mistas acima pode aparecer como:

![imagem](assets/media-sets/image-set-url-1.png)

- Saiba mais sobre Conjuntos de mídia mista com o [Início rápido para Conjuntos de mídia mista](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html?lang=pt-BR).

- Saiba como [Criar um conjunto de mídia mista](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html?lang=pt-BR#creating-a-mixed-media-set).

Para exibir uma imagem para zoom, um conjunto ou um vídeo em seu site, você o chama em um &quot;visualizador&quot; do Dynamic Media Classic. O Dynamic Media Classic inclui visualizadores para ativos de mídia avançada, como Conjuntos de amostras, Conjuntos de rotação, vídeo e muitos outros.

Saiba mais sobre [Visualizadores para AEM Assets e Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html?lang=pt-BR).

## Predefinições de conjunto de lotes

Até agora discutimos como criar conjuntos manualmente usando a função Build do Dynamic Media Classic. No entanto, é possível automatizar a criação de Conjuntos de imagens e Conjuntos de rotação usando uma Predefinição de conjunto de lotes, desde que você tenha uma convenção de nomenclatura padronizada.

Cada predefinição é um conjunto de instruções com nome exclusivo e independente que define como criar o conjunto usando imagens que correspondem às convenções de nomenclatura definidas. Na predefinição, primeiro você define as convenções de nomenclatura para os ativos que deseja agrupar em um conjunto. Uma Predefinição de conjunto de lotes pode ser criada para fazer referência a essas imagens.

Embora seja possível criar a predefinição por conta própria (elas são encontradas em **Configuração > Configuração do aplicativo > Predefinições do conjunto de lotes** ), como prática recomendada, você deve ter a equipe de Consultoria ou o Suporte Técnico configurado para você. Veja o porquê:

- As predefinições de conjunto de lotes podem ser complexas de configurar — elas são alimentadas por expressões regulares e, a menos que você seja um desenvolvedor, essa sintaxe pode não ser familiar ou ser confusa.
- Depois de criadas, elas são ativadas por padrão. Não há uma função &quot;desfazer&quot;. Se você começar a carregar milhares de imagens e sua predefinição estiver configurada incorretamente, você poderá ter centenas ou milhares de conjuntos corrompidos, que devem ser localizados e excluídos manualmente.

Uma convenção de nomenclatura simples foi sugerida anteriormente e seria muito fácil criar uma Predefinição de conjunto de lotes. No entanto, como as predefinições são muito flexíveis, elas podem lidar com estratégias de nomenclatura complexas. Resumindo, as imagens que pertencem a um conjunto devem ser vinculadas por algum nome comum — geralmente, é o número SKU ou a ID do produto. No Dynamic Media Classic, você informa uma convenção de nomenclatura padrão para todas as imagens a serem usadas para uma predefinição ou pode criar várias predefinições, cada uma com regras de nomenclatura diferentes.

As Predefinições de conjunto de lotes são aplicadas somente durante o upload; elas não podem ser executadas após as imagens terem sido carregadas. Portanto, é importante planejar sua convenção de nomenclatura e criar uma predefinição antes de começar a carregar todas as imagens.

Depois que as predefinições forem criadas, o Administrador da empresa poderá escolher se elas estarão ativas ou inativas. Ativo significa que elas aparecerão na página de upload em **Opções de trabalho**, enquanto predefinições inativas permanecerão ocultas.

Saiba como [Criar uma predefinição de conjunto de lotes](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=pt-BR#creating-a-batch-set-preset).

### Uso de predefinições de conjunto de lotes ao fazer upload

Veja como usar as Predefinições de conjunto de lotes ao fazer upload após sua criação:

1. Clique em **Carregar** e escolha **Da Área de Trabalho** ou **Via FTP**.
2. Clique em **Opções de Trabalho**.
3. Abra a opção **Predefinições de conjunto de lotes** e marque ou desmarque a predefinição para usá-la com o carregamento.
4. Quando o upload estiver concluído, procure os conjuntos concluídos na pasta.

Saiba mais sobre [Predefinições de conjunto de lotes](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=pt-BR#batch-set-presets).
