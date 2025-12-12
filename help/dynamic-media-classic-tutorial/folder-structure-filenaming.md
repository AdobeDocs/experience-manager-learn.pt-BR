---
title: Determinar a estrutura de pastas e a convenção de nomenclatura de arquivos
description: A nomeação de arquivos talvez seja a decisão mais importante que você tomará ao implementar o Dynamic Media Classic. A estrutura de pastas também é importante. Saiba por que é tão importante e as possíveis abordagens a serem seguidas para a estrutura de pastas e os nomes de arquivos.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 15121896-9196-4ce0-aff2-9178563326b4
duration: 253
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1202'
ht-degree: 1%

---

# Determinar a estrutura de pastas e a convenção de nomenclatura de arquivos {#folder-structure-filenaming}

Antes de começar a carregar todo o seu conteúdo, é recomendável considerar a estrutura de pastas que você usará e, em particular, a convenção de nomenclatura de arquivos. Isso provavelmente economizará tempo e terá que refazer as tarefas mais tarde. É melhor coordenar essas decisões em todos os grupos.

## Hierarquia de pastas e convenção de nomeação de arquivos

A nomeação de arquivos geralmente é a decisão mais importante que você toma em relação à implementação do Dynamic Media Classic. No entanto, para entender por que é importante, vamos falar primeiro sobre a estrutura de pastas.

### Hierarquia da pasta

A hierarquia de pastas é importante para você e sua empresa somente para fins organizacionais. Os URLs do Dynamic Media Classic só fazem referência ao nome do ativo, não à pasta ou ao caminho. Independentemente de onde você faça upload de um arquivo, o URL é o mesmo. Isso é bem diferente de como a maioria das pessoas organiza suas imagens e conteúdo para a Web, mas com o Dynamic Media Classic não faz diferença.

Outra consideração importante é o número de ativos ou pastas a serem armazenados em cada pasta. Se muitos ativos estiverem armazenados em uma pasta, o desempenho será reduzido ao visualizar ativos no Dynamic Media Classic. Não armazene milhares de ativos em uma pasta. Em vez disso, desenvolva uma hierarquia organizacional com menos de cerca de 500 ativos ou pastas em uma determinada ramificação da hierarquia. Isso não é um requisito estrito, mas ajuda a manter tempos de resposta aceitáveis ao visualizar ou pesquisar ativos. Na verdade, a recomendação é criar hierarquias largas e superficiais em vez de estreitas e profundas.

A maneira mais fácil de criar pastas é carregar toda a estrutura de pastas usando FTP e habilitar a opção **Incluir subpastas**. Essa opção faz com que o Dynamic Media Classic recrie a estrutura de pastas no site FTP no Dynamic Media Classic.

Queremos que você considere sua estrutura de pastas antes de começar a fazer upload de todos os arquivos, pois é muito mais fácil organizar e gerenciar seus arquivos e pastas localmente no computador do que dentro do Dynamic Media Classic. Por exemplo, você só pode arrastar e soltar arquivos, mas não pastas inteiras, dentro do Dynamic Media Classic.

### Estratégias de pasta

Para sua estratégia de pastas, considere o que faz sentido para sua organização. Estes são alguns cenários comuns de nomenclatura de pastas:

- Detalhamento do site espelho ou do produto. Por exemplo, se você vendeu roupas, pode ter pastas para Homens, Mulheres e Acessórios, e subpastas para Camisas e Sapatos.
- Estratégia baseada em SKU ou ID de produto. Por exemplo, com varejistas que possuem milhares de itens, pode ser interessante usar números de SKU ou IDs de produtos como nomes de pastas.
- Estratégia de marca. Por exemplo, os fabricantes com várias marcas podem escolher seus nomes de marca como pastas de nível superior.

## Convenção de nomenclatura de arquivos

A maneira como você escolhe nomear os arquivos talvez seja a decisão mais importante a ser tomada antecipadamente em relação ao Dynamic Media Classic. Isso ocorre porque todos os ativos no Dynamic Media Classic devem ter nomes exclusivos, independentemente de onde estejam armazenados na conta.

Todos os URLs e transações no Dynamic Media Classic são orientados por uma ID de ativo, que é o identificador exclusivo de um ativo no banco de dados. Ao fazer upload de um arquivo, a ID do ativo é criada pegando o nome do arquivo e removendo a extensão. Por exemplo, _896649.jpg_ obtém o Ativo _ID 896649_.

Regras sobre IDs de ativos:

- Nenhum ativo pode ter o mesmo nome no Dynamic Media Classic, independentemente da pasta em que os ativos estão.
- Os nomes diferenciam maiúsculas de minúsculas. Por exemplo, Chair.jpg, chair.jpg e CHAIR.jpg criariam três IDs de ativos diferentes.
- Como prática recomendada, as IDs de ativos não devem conter espaços em branco ou símbolos. O uso de espaços e símbolos torna a implementação mais difícil, pois será necessário codificar esses caracteres no URL. Por exemplo, um espaço &quot; &quot; torna-se &quot;%20&quot;.

Sua convenção de nomenclatura é essencialmente como você se integra ao Dynamic Media Classic. Normalmente, você não integra os sistemas de back-office à Dynamic Media Classic porque é um sistema fechado. É um parceiro passivo, aguardando instruções na forma de URLs.

A maioria dos usuários baseia sua convenção de nomenclatura em seu SKU interno ou IDs de produto para que, quando uma página da Web for chamada com informações sobre esse SKU, a página possa procurar automaticamente uma imagem com um nome semelhante. Se não houver conexão entre o nome do arquivo e o SKU ou a ID, o sistema de back-office precisará rastrear manualmente cada nome de arquivo, e uma pessoa terá que manter essas associações — em resumo, muito trabalho para as equipes de TI e de conteúdo.

### Estratégias de nomenclatura de arquivos

Sua estratégia de nomenclatura deve ser flexível para expansão futura, para que você possa evitar a necessidade de renomear após iniciar o. Estas são algumas estratégias de nomenclatura típicas:

**Nenhuma imagem alternativa.** Neste cenário, você tem apenas uma imagem por produto e nenhuma exibição alternativa ou colorida. Você nomearia cada imagem estritamente de acordo com seu SKU exclusivo ou número de ID do produto. Quando a página é carregada, o modelo de página chama a ID do ativo com o mesmo número SKU.

| SKU/PID | Nome de arquivo | ID do ativo |
| ------- | ---------- | -------- |
| 896649 | 896649.jpg | 896649 |
| SKU123 | SKU123.png | SKU123 |

Este é um sistema muito simples, e bom se você tem necessidades modestas. No entanto, não é muito flexível. Só porque você não tem imagens alternativas hoje não significa que você não terá essas imagens amanhã. O próximo cenário oferece mais flexibilidade.

**Usando a imagem, modos de exibição alternativos, versões coloridas e amostras.** Esta estratégia permite visões alternativas e/ ou coloridas, se você as tiver. Em vez de nomear a imagem somente após o SKU, você adiciona um modificador, como &quot;_1&quot; e &quot;_2&quot; para exibições alternativas, e um código de cor de &quot;_RED&quot; ou &quot;_BLU&quot; para exibições coloridas. Se você tiver imagens coloridas e visualizações alternativas para o mesmo produto, talvez adicione &quot;_RED_1&quot; e &quot;_RED_2&quot; para a primeira e segunda visualização em cor vermelha. As amostras seriam nomeadas com o SKU, o código de cor e uma extensão &quot;_SW&quot;.

| SKU/PID | Categoria | Nome de arquivo | ID do ativo |
| ------- | ----------------------- | ------------------------------------------- | ------------------------------- |
| AA123 | Alternar visualizações | AA123_1.tif AA123_2.tif AA123_3.tif | AA123_1 AA123_2 AA123_3 |
|         | Exibições coloridas | AA123_BLU.tif AA123_RED.tif AA123_BROWN.tif | AA123_BLU AA123_RED AA123_BROWN |
|         | Amostras | AA123_BLU_SW.tif | AA123_BLU_SW |
|         | Conjunto de imagens ou Conjunto de amostras |                                             | AA123 ou AA123_SET |

Ao lidar com coleções definidas, como Conjuntos de imagens e Conjuntos de amostras, o próprio conjunto também deve ter um nome exclusivo. Nesse caso, o conjunto pode receber o SKU base como seu nome ou o SKU com uma extensão &quot;_SET&quot;.

### Convenção de nomenclatura e automação

Uma última palavra sobre a importância da convenção de nomenclatura. Se você quiser usar conjuntos (como Conjuntos de imagens ou Conjuntos de amostras), uma convenção de nomenclatura previsível permitirá automatizar sua criação. Qualquer método com script, como uma Predefinição de conjunto de lotes, que você aprenderá em uma seção separada deste tutorial, pode ser removido de uma convenção de nomenclatura.

O método alternativo é criar manualmente os conjuntos. Embora a criação manual de conjuntos de imagens para 200 imagens possa não ser um grande trabalho, imagine se você tiver mais de 100.000 imagens. É quando a automação de criação de conjuntos se torna crucial.
