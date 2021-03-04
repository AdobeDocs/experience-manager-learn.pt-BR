---
title: Determine sua estrutura de pastas e convenção de nomenclatura de arquivos
description: A nomeação de arquivos talvez seja a decisão mais importante que você tomará ao implementar o Dynamic Media Classic. A estrutura de pastas também é importante. Saiba por que é tão importante e possível adotar abordagens para a estrutura de pastas e nomes de arquivos.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
activity: develop
topics: development, authoring, configuring, architecture
audience: all
topic: Gerenciamento de conteúdo
role: Profissional
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1219'
ht-degree: 0%

---


# Determine a estrutura da pasta e a convenção de nomenclatura de arquivo {#folder-structure-filenaming}

Antes de começar a fazer upload de todo o seu conteúdo, é recomendável considerar a estrutura de pastas que você usará e, particularmente, sua convenção de nomenclatura de arquivos. Provavelmente economizará tempo e terá de refazer tarefas mais tarde. É melhor coordenar essas decisões em todos os grupos.

## Hierarquia de pastas e Convenção de nomenclatura de arquivos

A nomenclatura de arquivos é, em geral, a decisão mais importante que você toma em relação à implementação do Dynamic Media Classic. No entanto, para entender por que é importante, vamos primeiro falar sobre sua estrutura de pastas.

### Hierarquia de pastas

A hierarquia de pastas é importante somente para você e sua empresa, para fins organizacionais. Seus URLs do Dynamic Media Classic só fazem referência ao nome do ativo, não à pasta ou ao caminho. Independentemente de onde você tenha carregado um arquivo, o URL será o mesmo. Isso é bem diferente de como a maioria das pessoas organiza suas imagens e conteúdo para a Web, mas com o Dynamic Media Classic não faz diferença.

Outra consideração importante é o número de ativos ou pastas a serem armazenados em cada pasta. Se muitos ativos forem armazenados em uma pasta, o desempenho diminuirá ao visualizar ativos no Dynamic Media Classic. Não armazene milhares de ativos em uma pasta. Em vez disso, desenvolva uma hierarquia organizacional com menos de 500 ativos ou pastas em uma determinada ramificação de sua hierarquia. Isso não é um requisito restrito, mas ajudará a manter tempos de resposta aceitáveis ao visualizar ou pesquisar ativos. Na verdade, a recomendação é criar hierarquias largas e superficiais em vez de estreitas e profundas.

A maneira mais fácil de criar suas pastas é fazer upload de toda a estrutura de pastas usando FTP e ativar a opção **Incluir subpastas**. Essa opção faz com que o Dynamic Media Classic recrie a estrutura de pastas no site FTP no Dynamic Media Classic.

Queremos que você considere a estrutura de pastas antes de começar a carregar todos os arquivos, pois é muito mais fácil organizar e gerenciar os arquivos e pastas localmente no computador do que no Dynamic Media Classic. Por exemplo, você só pode arrastar e soltar arquivos, mas não pastas inteiras, dentro do Dynamic Media Classic.

### Estratégias de pastas

Para sua estratégia de pastas, considere o que faz sentido para sua organização. Estes são alguns cenários comuns de nomeação de pastas:

- Espelhar o site ou detalhamento de produto. Por exemplo, se você vendeu roupas, pode ter pastas para Homens, Mulheres e Acessórios e subpastas para Camisas e Sapatos.
- Estratégia baseada em SKU ou ID de produto. Por exemplo, com varejistas que têm milhares de itens, pode fazer sentido usar números de SKU ou IDs de produto como nomes de pastas.
- Estratégia da marca. Por exemplo, os fabricantes que têm várias marcas podem escolher seus nomes de marca como pastas de nível superior.

## Convenção de nomenclatura de arquivo

A maneira como você escolhe nomear seus arquivos talvez seja a decisão inicial mais importante que você tomará em relação ao Dynamic Media Classic. Isso ocorre porque todos os ativos no Dynamic Media Classic devem ter nomes exclusivos, independentemente de onde estejam armazenados na conta.

Todos os URLs e transações no Dynamic Media Classic são orientados por uma ID de ativo, que é o identificador exclusivo de um ativo no banco de dados. Ao fazer upload de um arquivo, a ID do ativo é criada pegando no nome do arquivo e removendo a extensão. Por exemplo, _896649.jpg_ obtém o Ativo _ID 896649_.

Regras relacionadas às IDs de ativos:

- Dois ativos não podem ter o mesmo nome no Dynamic Media Classic, independentemente da pasta em que estão os ativos.
- Os nomes fazem distinção entre maiúsculas e minúsculas. Por exemplo, chair.jpg, chair.jpg e CHAIR.jpg criariam três IDs de ativo diferentes.
- Como prática recomendada, as IDs de ativo não devem conter espaços em branco ou símbolos. O uso de espaços e símbolos dificulta a implementação, pois será necessário codificar esses caracteres por URL. Por exemplo, um espaço &quot; &quot; torna-se &quot;%20.&quot;

Sua convenção de nomenclatura é essencialmente a forma como você se integra ao Dynamic Media Classic. Geralmente, você não integra os sistemas de back-office ao Dynamic Media Classic, pois ele é um sistema fechado. É um parceiro passivo, aguardando instruções na forma de URLs.

A maioria dos usuários baseia sua convenção de nomenclatura em seus SKUs internos ou IDs de produtos, de modo que, quando uma página da Web é chamada com informações sobre esse SKU, a página pode procurar automaticamente uma imagem que tenha um nome semelhante. Se não houver conexão entre o nome do arquivo e o SKU ou ID, o sistema de back-office precisará rastrear manualmente cada nome de arquivo e uma pessoa terá que manter essas associações — em resumo, muito trabalho para as equipes de TI e de conteúdo.

### Estratégias de nomenclatura de arquivo

Sua estratégia de nomeação deve ser flexível para futura expansão, para que seja possível evitar a necessidade de renomear após o lançamento. Estas são algumas estratégias típicas de nomenclatura:

**Nenhuma imagem alternativa.** Neste cenário, você tem apenas uma imagem por produto e nenhuma visualização alternativa ou colorida. Você nomearia estritamente cada imagem de acordo com seu SKU exclusivo ou número de ID do produto. Quando a página é carregada, o modelo da página chama a ID do ativo com o mesmo número SKU.

| SKU/PID | Nome de arquivo | ID do ativo |
| ------- | ---------- | -------- |
| 896649 | 896649.jpg | 896649 |
| SKU123 | SKU123.png | SKU123 |

Este é um sistema muito simples, e bom se você tem necessidades modestas. No entanto, não é muito flexível. Só porque você não tem imagens alternativas hoje não significa que você não terá essas imagens amanhã. O próximo cenário oferece mais flexibilidade.

**Usando a imagem, exibições alternativas, versões coloridas, amostras.** Essa estratégia permite exibições alternativas e/ou coloridas, se houver. Em vez de nomear a imagem depois apenas do SKU, você adiciona um modificador como &quot;_1&quot; e &quot;_2&quot; para exibições alternativas e um código de cor de &quot;_RED&quot; ou &quot;_BLU&quot; para exibições coloridas. Se você tiver imagens coloridas e visualizações alternativas para o mesmo produto, talvez adicione &quot;_RED_1&quot; e &quot;_RED_2&quot; para a primeira e a segunda visualização colorida. As amostras seriam nomeadas com o SKU, código de cor e uma extensão &quot;_SW&quot;.

| SKU/PID | Categoria | Nome de arquivo | ID do ativo |
| ------- | ----------------------- | ------------------------------------------- | ------------------------------- |
| AA123 | Exibições Alt | AA123_1.tif AA123_2.tif AA123_3.tif | AA123_1 AA123_2 AA123_3 |
|  | Exibições coloridas | AA123_BLU.tif AA123_RED.tif AA123_BROWN.tif | AA123_BLU AA123_RED AA123_BROWN |
|  | Amostras | AA123_BLU_SW.tif | AA123_BLU_SW |
|  | Conjunto de imagens ou conjunto de amostras |  | AA123 ou AA123_SET | — |

Ao lidar com coleções de conjuntos, como Conjuntos de imagens e Conjuntos de amostras, o próprio conjunto também deve ter um nome exclusivo. Portanto, nesse caso, o conjunto poderia receber o SKU base como seu nome ou o SKU com uma extensão &quot;_SET&quot;.

### Convenção de nomenclatura e automação

Uma última palavra sobre a importância da convenção de nomenclatura. Se você quiser usar conjuntos (como Conjuntos de imagens ou Conjuntos de amostras), uma convenção de nomenclatura previsível permitirá automatizar a criação. Qualquer método com script, como uma Predefinição de conjunto de lotes, sobre a qual você aprenderá em uma seção separada deste tutorial, pode ser excluído de uma convenção de nomenclatura.

O método alternativo é criar manualmente os conjuntos. Embora a criação manual de conjuntos de imagens para 200 imagens possa não ser um grande trabalho, imagine se você tiver mais de 100.000 imagens. Isso ocorre quando a automação da criação de conjuntos se torna essencial.
