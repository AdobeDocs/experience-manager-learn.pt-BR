---
title: Uso de rich text com o AEM Headless
description: Saiba como criar conteúdo e incorporar conteúdo referenciado usando um editor de rich text de várias linhas com Fragmentos de conteúdo do Adobe Experience Manager e como o rich text é entregue pelas APIs do GraphQL da AEM como JSON para ser consumido por aplicativos headless.
version: Experience Manager as a Cloud Service
doc-type: article
jira: KT-9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
level: Intermediate
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
duration: 785
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1409'
ht-degree: 0%

---

# Rich text com AEM Headless

O campo de texto multilinha é um tipo de dados dos Fragmentos de conteúdo que permite aos autores criar conteúdo rich text. As referências a outro conteúdo, como imagens ou outros Fragmentos de conteúdo, podem ser inseridas dinamicamente em linha no fluxo do texto. O campo Texto de linha única é outro tipo de dados dos Fragmentos de conteúdo que devem ser usados para elementos de texto simples.

A API do GraphQL do AEM oferece um recurso robusto para retornar rich text como HTML, texto simples ou JSON puro. A representação JSON é poderosa, pois fornece ao aplicativo cliente controle total sobre como renderizar o conteúdo.

## Editor de várias linhas

>[!VIDEO](https://video.tv.adobe.com/v/342104?quality=12&learn=on)

No Editor de fragmento de conteúdo, a barra de menus do campo de texto multilinha fornece aos autores recursos padrão de formatação de rich text, como **negrito**, *itálico* e sublinhado. A abertura do campo de várias linhas no modo de tela cheia habilita [ferramentas de formatação adicionais, como o Tipo de parágrafo, localizar e substituir, verificação ortográfica e muito mais](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html?lang=pt-BR).

>[!NOTE]
>
> Os plug-ins de rich text no editor de várias linhas não podem ser personalizados.

## Tipo de dados de texto multilinha {#multi-line-data-type}

Use o tipo de dados **Texto de várias linhas** ao definir seu Modelo de fragmento de conteúdo para habilitar a criação de rich text.

![Tipo de dados de rich text de várias linhas](assets/rich-text/multi-line-rich-text.png)

Várias propriedades do campo de várias linhas podem ser configuradas.

A propriedade **Renderizar como** pode ser definida como:

* Área de texto - renderiza um único campo de várias linhas
* Multiple Field - renderiza vários campos de várias linhas


O **Tipo Padrão** pode ser definido como:

* Texto formatado
* Markdown
* Texto sem formatação

A opção **Tipo Padrão** influencia diretamente a experiência de edição e determina se as ferramentas de rich text estão presentes.

Você também pode [habilitar referências embutidas](#insert-fragment-references) a outros fragmentos de conteúdo marcando a **Referência de fragmento de permissão** e configurando os **Modelos de fragmento de conteúdo permitidos**.

Marque a caixa **Traduzível** se o conteúdo deve ser localizado. Somente Rich Text e Texto sem formatação podem ser localizados. Consulte [trabalhando com conteúdo localizado para obter mais detalhes](./localized-content.md).

## Resposta em rich text com a API do GraphQL

Ao criar uma consulta do GraphQL, os desenvolvedores podem escolher tipos de resposta diferentes de `html`, `plaintext`, `markdown` e `json` de um campo de várias linhas.

Os desenvolvedores podem usar a [Visualização JSON](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html?lang=pt-BR) no editor de fragmento de conteúdo para mostrar todos os valores do fragmento de conteúdo atual que podem ser retornados usando a API do GraphQL.

## Consulta persistente do GraphQL

Selecionar o formato de resposta `json` para o campo de várias linhas oferece mais flexibilidade ao trabalhar com conteúdo rich text. O conteúdo rich text é entregue como uma matriz de tipos de nó JSON que podem ser processados exclusivamente com base na plataforma do cliente.

Abaixo está um tipo de resposta JSON de um campo de várias linhas chamado `main` que contém um parágrafo: &quot;*Este é um parágrafo que inclui conteúdo **importante**.*&quot; onde &quot;importante&quot; está marcado como **bold**.

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        json
      }
    }
  }
}
```

A variável `$path` usada no filtro `_path` requer o caminho completo para o fragmento de conteúdo (por exemplo, `/content/dam/wknd/en/magazine/sample-article`).

**Resposta do GraphQL:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            }
          ]
        }
      }
    }
  }
}
```

### Outros exemplos

Abaixo estão vários exemplos de tipos de resposta de um campo de várias linhas chamado `main` que contém um parágrafo: &quot;Este é um parágrafo que inclui conteúdo **importante**.&quot; onde &quot;importante&quot; está marcado como **bold**.

+++exemplo de HTML

**Consulta persistente do GraphQL:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        html
      }
    }
  }
}
```

**Resposta do GraphQL:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "html": "<p>This is a paragraph that includes <b>important</b> content.&nbsp;</p>\n"
        }
      }
    }
  }
}
```

+++

+++Exemplo de Markdown

**Consulta persistente do GraphQL:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        markdown
      }
    }
  }
}
```

**Resposta do GraphQL:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "markdown": "This is a paragraph that includes **important** content. \n\n ",
        }
      }
    }
  }
}
```

+++

+++Exemplo de texto sem formatação

**Consulta persistente do GraphQL:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        plaintext
      }
    }
  }
}
```

**Resposta do GraphQL:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "plaintext": "This is a paragraph that includes important content. ",
        }
      }
    }
  }
}
```

A opção de renderização `plaintext` elimina qualquer formatação.

+++


## Renderização de uma resposta JSON de rich text {#render-multiline-json-richtext}

A resposta JSON de rich text do campo de várias linhas é estruturada como uma árvore hierárquica. Cada objeto ou nó representa um bloco HTML diferente do rich text.

Abaixo está uma amostra da resposta JSON de um campo de texto de várias linhas. Observe que cada objeto ou nó inclui um `nodeType` que representa o bloco HTML do rich text como `paragraph`, `link` e `text`. Cada nó opcionalmente contém `content`, que é uma submatriz que contém quaisquer filhos do nó atual.

```json
"json": [// root "content" or child nodes
            {
                "nodeType": "paragraph", // node for a paragraph
                "content": [ // children of current node
                {
                    "nodeType": "text", // node for a text
                    "value": "This is the first paragraph. "
                },
                {
                    "nodeType": "link",
                    "data": {
                        "href": "http://www.adobe.com"
                    },
                    "value": "An external link"
                }
                ],
            },
            {
                "nodeType": "paragraph",
                "content": [
                {
                    "nodeType": "text",
                    "value": "This is the second paragraph."
                },
                ],
            },
]
```

A maneira mais fácil de renderizar a resposta de várias linhas do `json` é processar cada objeto ou nó na resposta e, em seguida, processar qualquer filho do nó atual. Uma função recursiva pode ser usada para atravessar a árvore JSON.

Abaixo está um exemplo de código que ilustra uma abordagem de travessia recursiva. As amostras são baseadas em JavaScript e usam o [JSX](https://reactjs.org/docs/introducing-jsx.html) do React, no entanto, os conceitos de programação podem ser aplicados a qualquer linguagem.

```javascript
// renderNodeList - renders a list of nodes
function renderNodeList(childNodes) {
    
    if(!childNodes) {
        // null check
        return null;
    }

    return childNodes.map(node, index) => {
        return renderNode(node);
    }
}
```

`renderNodeList` é uma função recursiva que utiliza uma matriz de `childNodes`. Cada nó na matriz é passado para uma função `renderNode`, que, por sua vez, chama `renderNodeList` se o nó tiver filhos.

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

A função `renderNode` espera um único objeto chamado `node`. Um nó pode ter filhos que são processados recursivamente usando a função `renderNodeList` descrita acima. Finalmente, um `nodeMap` é usado para renderizar o conteúdo do nó com base em seu `nodeType`.

```javascript
// nodeMap - object literal that maps a JSX response based on a given key (nodeType)
const nodeMap = {
    'paragraph': (node, children) => <p>{children}</p>,
    'link': node => <a href={node.data.href} target={node.data.target}>{node.value}</a>,
    'text': node => node.value,
    'unordered-list': (node, children) => <ul>{children}</ul>,
    'ordered-list': (node, children) => <ol>{children}</ol>,
    'list-item': (node, children) => <li>{children}</li>,
    ...
}
```

`nodeMap` é um literal de Objeto JavaScript usado como mapa. Cada uma das &quot;chaves&quot; representa uma `nodeType` diferente. Parâmetros de `node` e `children` podem ser passados para as funções resultantes que renderizam o nó. O tipo de retorno usado neste exemplo é JSX. No entanto, a abordagem pode ser adaptada para criar um literal de string que representa o conteúdo do HTML.

### Exemplo de código completo

Um utilitário de renderização de rich text reutilizável pode ser encontrado no [exemplo do WKND GraphQL React](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/utils/renderRichText.js) - utilitário reutilizável que expõe uma função `mapJsonRichText`. Esse utilitário pode ser usado por componentes que desejam renderizar uma resposta JSON de rich text como JSX de reação.
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) - Componente de exemplo que faz uma solicitação GraphQL que inclui rich text. O componente usa o utilitário `mapJsonRichText` para renderizar o rich text e quaisquer referências.


## Adicionar referências embutidas ao rich text {#insert-fragment-references}

O campo Múltiplo permite que os autores insiram imagens ou outros ativos digitais do AEM Assets no fluxo do rich text.

![inserir imagem](assets/rich-text/insert-image.png)

A captura de tela acima representa uma imagem inserida no campo de várias linhas usando o botão **Inserir ativo**.

As referências a outros fragmentos de conteúdo também podem ser vinculadas ou inseridas no campo de várias linhas usando o botão **Inserir fragmento de conteúdo**.

![Inserir referência de fragmento de conteúdo](assets/rich-text/insert-contentfragment.png)

A captura de tela acima representa outro fragmento de conteúdo, Guia do Ultimate para os parques de skate de Los Angeles, sendo inserido no campo de várias linhas. Os tipos de fragmentos de conteúdo que podem ser inseridos em um campo são controlados pela configuração **Modelos de fragmento de conteúdo permitidos** no [tipo de dados de várias linhas](#multi-line-data-type) no Modelo de fragmento de conteúdo.

## Consultar referências em linha com o GraphQL

A API do GraphQL permite que os desenvolvedores criem um query que inclui propriedades adicionais sobre quaisquer referências inseridas em um campo de várias linhas. A resposta JSON inclui um objeto `_references` separado que lista essas propriedades extras. A resposta JSON fornece aos desenvolvedores controle total sobre como renderizar as referências ou links, em vez de precisar lidar com HTML opinativos.

Por exemplo, talvez você queira:

* Inclua uma lógica de roteamento personalizada para gerenciar links para outros Fragmentos de conteúdo ao implementar um Aplicativo de página única, como usar o Roteador React ou o Next.js
* Renderize uma imagem embutida usando o caminho absoluto para um ambiente de Publicação do AEM como o valor `src`.
* Determine como renderizar uma referência incorporada a outro fragmento de conteúdo com propriedades personalizadas adicionais.

Use o tipo de retorno `json` e inclua o objeto `_references` ao construir uma consulta GraphQL:

**Consulta persistente do GraphQL:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true })
  {
    item {
      _path
      main {
        json
      }
    }
    _references {
      ...on ImageRef {
        _dynamicUrl
        __typename
      }
      ...on ArticleModel {
        _path
        author
        __typename
      }  
    }
  }
}
```

Na consulta acima, o campo `main` é retornado como JSON. O objeto `_references` inclui fragmentos para manipular quaisquer referências que sejam do tipo `ImageRef` ou do tipo `ArticleModel`.

**Resposta JSON:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "reference",
                  "data": {
                    "path": "/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
                    "mimetype": "image/jpeg"
                  }
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "Reference another Content Fragment: "
                },
                {
                  "nodeType": "reference",
                  "data": {
                    "href": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
                    "type": "fragment"
                  },
                  "value": "Ultimate Guide to LA Skateparks"
                }
              ]
            }
          ]
        }
      },
      "_references": [
        {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/sport-climbing.jpg?preferwebp=true",
          "__typename": "ImageRef"
        },
        {
          "_path": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
          "author": "Stacey Roswells",
          "__typename": "ArticleModel"
        }
      ]
    }
  }
}
```

A resposta JSON inclui onde a referência foi inserida no rich text com o `"nodeType": "reference"`. O objeto `_references` inclui cada referência.

## Renderização de referências em linha em rich text

Para renderizar referências embutidas, a abordagem recursiva explicada em [Renderizando uma resposta JSON de várias linhas](#render-multiline-json-richtext) pode ser expandida.

Onde `nodeMap` é o mapa que renderiza os nós JSON.

```javascript
const nodeMap = {
        'reference': (node, children) => {

            // variable for reference in _references object
            let reference;
            
            // asset reference
            if (node.data.path) {
                // find reference based on path
                reference = references.find( ref => ref._path === node.data.path);
            }
            // Fragment Reference
            if (node.data.href) {
                // find in-line reference within _references array based on href and _path properties
                reference = references.find( ref => ref._path === node.data.href);
            }

            // if reference found, merge properties of reference and current node, then return render method of it using __typename property
            return reference ? renderReference[reference.__typename]({...reference, ...node}) : null;
        }
    }
```

A abordagem de alto nível é inspecionar sempre que um `nodeType` for igual a `reference` na resposta JSON de Várias Linhas. Uma função de renderização personalizada pode ser chamada, incluindo o objeto `_references` retornado na resposta do GraphQL.

O caminho de referência embutido pode ser comparado à entrada correspondente no objeto `_references` e outro mapa personalizado `renderReference` pode ser chamado.

```javascript
const renderReference = {
    // node contains merged properties of the in-line reference and _references object
    'ImageRef': (node) => {
        // when __typename === ImageRef
        return <img src={node._dynamicUrl} alt={'in-line reference'} /> 
    },
    'ArticleModel': (node) => {
        // when __typename === ArticleModel
        return <Link to={`/article:${node._path}`}>{`${node.value}`}</Link>;
    }
    ...
}
```

O `__typename` do objeto `_references` pode ser usado para mapear diferentes tipos de referência para diferentes funções de renderização.

### Exemplo de código completo

Um exemplo completo de como escrever um renderizador de referências personalizadas pode ser encontrado em [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) como parte do [exemplo do WKND GraphQL React](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

## Exemplo completo

>[!VIDEO](https://video.tv.adobe.com/v/3449705?quality=12&learn=on&captions=por_br)

>[!NOTE]
>
> O vídeo acima usa `_publishUrl` para renderizar a referência da imagem. Em vez disso, prefira o `_dynamicUrl` como explicado na [apresentação de imagens otimizadas para a Web](./images.md);


O vídeo anterior mostra um exemplo completo:

1. Atualização de um campo de texto de várias linhas do modelo de fragmento de conteúdo para permitir Referências de fragmento
2. Usar o Editor de fragmentos de conteúdo para incluir uma imagem e fazer referência a outro fragmento em um campo de texto de várias linhas.
3. Criando uma consulta GraphQL que inclua a resposta de texto multilinha como JSON e qualquer `_references` usada.
4. Gravação de um SPA do React que renderiza as referências embutidas da resposta em rich text.
