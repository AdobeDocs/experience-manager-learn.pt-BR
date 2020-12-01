---
title: Determine a estrutura da sua pasta e a convenção de nomenclatura de arquivos
description: A nomeação de arquivos talvez seja a decisão mais importante que você tomará ao implementar o Dynamic Media Classic. A estrutura da pasta também é importante. Saiba por que é tão importante e possível adotar abordagens para a estrutura de pastas e nomes de arquivos.
sub-product: dynamic-media
feature: null
doc-type: tutorial
activity: develop
topics: development, authoring, configuring, architecture
audience: all
translation-type: tm+mt
source-git-commit: e7a02900b0582fe9b329e5f9bd568f3c54d8d63d
workflow-type: tm+mt
source-wordcount: '1211'
ht-degree: 0%

---


# Determine a estrutura da sua pasta e a convenção de nomenclatura de arquivos {#folder-structure-filenaming}

Antes de acessar e fazer upload do start de todo o seu conteúdo, convém considerar a estrutura de pastas que você usará e, em particular, sua convenção de nomenclatura de arquivos. Provavelmente economizará tempo e precisará refazer tarefas mais tarde. É melhor coordenar essas decisões em todos os grupos.

## Hierarquia de pastas e Convenção de nomenclatura de arquivos

A nomeação de arquivos é geralmente a decisão mais importante que você toma em relação à implementação do Dynamic Media Classic. Entretanto, para entender por que isso é importante, vamos primeiro falar sobre a estrutura de pastas.

### Hierarquia da pasta

A hierarquia de pastas é importante para você e sua empresa apenas para fins organizacionais. seus URLs do Dynamic Media Classic apenas fazem referência ao nome do ativo, não à pasta ou ao caminho. Independentemente de onde você carregou um arquivo, o URL será o mesmo. Isso é bem diferente de como a maioria das pessoas organiza suas imagens e conteúdo para a Web, mas com o Dynamic Media Classic isso não faz diferença.

Outra consideração importante é o número de ativos ou pastas a serem armazenados em cada pasta. Se muitos ativos forem armazenados em uma pasta, o desempenho diminuirá ao exibir ativos no Dynamic Media Classic. Não armazene milhares de ativos em uma pasta. Em vez disso, desenvolva uma hierarquia organizacional com menos de 500 ativos ou pastas em um determinado ramo da sua hierarquia. Esse não é um requisito restrito, mas ajudará a manter tempos de resposta aceitáveis ao visualizar ou pesquisar ativos. Na verdade, a recomendação é criar hierarquias que sejam largas e superficiais, em vez de estreitas e profundas.

A maneira mais fácil de criar suas pastas é fazer upload de toda a estrutura de pastas usando o FTP e ativar a opção **Incluir subpastas**. Essa opção faz com que o Dynamic Media Classic recrie a estrutura de pastas no site FTP no Dynamic Media Classic.

Queremos que você considere a estrutura da sua pasta antes de fazer upload de todos os seus arquivos em start, pois é muito mais fácil organizar e gerenciar os arquivos e pastas localmente no seu computador do que no Dynamic Media Classic. Por exemplo, você só pode arrastar e soltar arquivos, mas não pastas inteiras, dentro do Dynamic Media Classic.

### Estratégias de pastas

Para sua estratégia de pasta, considere o que faz sentido para sua organização. Estes são alguns cenários comuns de nomeação de pastas:

- Detalhamento do site ou produto espelhado. Por exemplo, se você vendeu roupas, talvez tenha pastas para Masculino, Mulher e Acessórios e subpastas para Camisas e Sapatos.
- Estratégia baseada em SKU ou ID de produto. Por exemplo, com varejistas que têm milhares de itens, pode fazer sentido usar números de SKU ou IDs de produto como nomes de pastas.
- Estratégia da marca. Por exemplo, os fabricantes que têm várias marcas podem escolher seus nomes de marca como pastas de nível superior.

## Convenção de nomenclatura de arquivo

A maneira como você escolhe nomear seus arquivos é talvez a decisão mais importante que você tomará no início do Dynamic Media Classic. Isso ocorre porque todos os ativos no Dynamic Media Classic devem ter nomes exclusivos, independentemente de onde estejam armazenados na conta.

Todos os URLs e transações no Dynamic Media Classic são direcionados por uma ID de ativo, que é um identificador exclusivo do ativo no banco de dados. Quando você carrega um arquivo, a ID do ativo é criada com o nome do arquivo e a remoção da extensão. Por exemplo, _896649.jpg_ obtém o Asset _ID 896649_.

Regras referentes às IDs de ativos:

- Nenhum dos dois ativos pode ter o mesmo nome no Dynamic Media Classic, independentemente da pasta em que os ativos estão.
- Os nomes fazem distinção entre maiúsculas e minúsculas. Por exemplo, President.jpg, President.jpg e CHAIR.jpg criariam três IDs de ativos diferentes.
- Como prática recomendada, as IDs de ativos não devem conter espaços em branco ou símbolos. O uso de espaços e símbolos dificulta a implementação, pois será necessário codificar esses caracteres por URL. Por exemplo, um espaço &quot; &quot; torna-se &quot;%20&quot;.

Sua convenção de nomenclatura é essencialmente a forma como você se integra ao Dynamic Media Classic. Geralmente, você não integra seus sistemas de back-office ao Dynamic Media Classic, pois ele é um sistema fechado. É um parceiro passivo, aguardando instruções na forma de URLs.

A maioria dos usuários baseia sua convenção de nomenclatura em seus SKUs internos ou IDs de produtos para que, quando uma página da Web é chamada com informações sobre esse SKU, a página possa procurar automaticamente uma imagem que tenha um nome semelhante. Se não houver conexão entre o nome do arquivo e o SKU ou ID, seu sistema de back-office precisará rastrear manualmente cada nome de arquivo e uma pessoa deverá manter essas associações. em resumo, muito trabalho para as equipes de TI e de conteúdo.

### Estratégias de nomenclatura de arquivos

Sua estratégia de nomeação deve ser flexível para expansão futura, para que você não precise renomear após ter sido iniciado. Estas são algumas estratégias típicas de nomeação:

**Nenhuma imagem alternativa.** Nesse cenário, você só tem uma imagem por produto e nenhuma visualização alternativa ou colorida. Você nomearia estritamente cada imagem de acordo com seu SKU exclusivo ou número de ID de produto. Quando a página é carregada, o modelo de página chama a ID do ativo com o mesmo número SKU.

| SKU/PID | Nome de arquivo | ID do ativo |
| ------- | ---------- | -------- |
| 896649 | 896649.jpg | 896649 |
| SKU123 | SKU123.png | SKU123 |

Este é um sistema muito simples, e bom se você tem necessidades modestas. No entanto, não é muito flexível. Só porque você não tem imagens alternativas hoje não significa que você não terá essas imagens amanhã. O próximo cenário oferta mais flexibilidade.

**Usando a imagem, visualizações alternativas, versões coloridas e amostras.** Essa estratégia permite visualizações alternativas e/ou visualizações coloridas, se você as tiver. Em vez de nomear a imagem somente depois do SKU, adicione um modificador como &quot;_1&quot; e &quot;_2&quot; para visualizações alternativas e um código de cor como &quot;_RED&quot; ou &quot;_BLU&quot; para visualizações coloridas. Se você tiver imagens coloridas e visualizações alternativas para o mesmo produto, talvez você adicione &quot;_RED_1&quot; e &quot;_RED_2&quot; para a primeira e a segunda visualização vermelha. As amostras seriam nomeadas com o SKU, código de cor e uma extensão &quot;_SW&quot;.

| SKU/PID | Categoria | Nome de arquivo | ID do ativo |
| ------- | ----------------------- | ------------------------------------------- | ------------------------------- |
| AA123 | Visualizações Alt | AA123_1.tif AA123_2.tif AA123_3.tif | AA123_1 AA123_2 AA123_3 |
|  | Visualizações coloridas | AA123_BLU.tif AA123_RED.tif AA123_BROWN.tif | AA123_BLU AA123_RED AA123_BROWN |
|  | Amostras | AA123_BLU_SW.tif | AA123_BLU_SW |
|  | Conjunto de imagens ou conjunto de amostras |  | AA123 ou AA123_SET | — |

Ao lidar com coleções definidas, como Conjuntos de imagens e Conjuntos de amostras, o próprio conjunto também deve ter um nome exclusivo. Portanto, nesse caso, o conjunto poderia receber o SKU base como seu nome, ou o SKU com uma extensão &quot;_SET&quot;.

### Nomeando convenção e automação

Uma última palavra sobre a importância de nomear a convenção. Se você quiser usar conjuntos (como Conjuntos de imagens ou Conjuntos de amostras), uma convenção de nomenclatura previsível permitirá que você automatize sua criação. Qualquer método com script, como uma predefinição de conjunto de lotes, sobre a qual você aprenderá em uma seção separada deste tutorial, pode ser excluído de uma convenção de nomenclatura.

O método alternativo é criar manualmente seus conjuntos. Enquanto a criação manual de conjuntos de imagens para 200 imagens pode não ser um grande trabalho, imagine se você tiver mais de 100.000 imagens. Isso é quando a automação da criação de conjuntos se torna crucial.
