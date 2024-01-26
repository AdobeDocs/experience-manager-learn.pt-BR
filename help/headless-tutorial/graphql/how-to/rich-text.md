---
title: Uso de rich text com AEM Headless
description: Saiba como criar conteúdo e incorporar conteúdo referenciado usando um editor de rich text de várias linhas com Fragmentos de conteúdo do Adobe Experience Manager e como o rich text é entregue pelas APIs do AEM GraphQL como JSON para ser consumido por aplicativos headless.
version: Cloud Service
doc-type: article
jira: KT-9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
level: Intermediate
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
duration: 821
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1409'
ht-degree: 0%

---

# Rich text com AEM Headless

O campo de texto multilinha é um tipo de dados dos Fragmentos de conteúdo que permite aos autores criar conteúdo rich text. As referências a outro conteúdo, como imagens ou outros Fragmentos de conteúdo, podem ser inseridas dinamicamente em linha no fluxo do texto. O campo Texto de linha única é outro tipo de dados dos Fragmentos de conteúdo que devem ser usados para elementos de texto simples.

A API do GraphQL do AEM oferece uma capacidade robusta de retornar rich text como HTML, texto simples ou JSON puro. A representação JSON é poderosa, pois fornece ao aplicativo cliente controle total sobre como renderizar o conteúdo.

## Editor de várias linhas

>[!VIDEO](https://video.tv.adobe.com/v/342104?quality=12&learn=on)

No Editor de fragmento de conteúdo, a barra de menu do campo de texto multilinha fornece aos autores recursos padrão de formatação de rich text, como **negrito**, *itálico* e sublinhado. Abrir o campo multilinha no modo de tela cheia habilita [ferramentas adicionais de formatação, como tipo de parágrafo, localizar e substituir, verificação ortográfica e muito mais](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html?lang=pt-BR).

>[!NOTE]
>
> Os plug-ins de rich text no editor de várias linhas não podem ser personalizados.

## Tipo de dados de texto multilinha {#multi-line-data-type}

Use o **Texto multilinha** tipo de dados ao definir o Modelo de fragmento de conteúdo para permitir a criação de rich text.

![Tipo de dados rich text de várias linhas](assets/rich-text/multi-line-rich-text.png)

Várias propriedades do campo de várias linhas podem ser configuradas.

A variável **Renderizar como** pode ser definida como:

* Área de texto - renderiza um único campo de várias linhas
* Multiple Field - renderiza vários campos de várias linhas


A variável **Tipo padrão** pode ser definido como:

* Texto formatado
* Markdown
* Texto sem formatação

A variável **Tipo padrão** A opção influencia diretamente a experiência de edição e determina se as ferramentas de rich text estão presentes.

Também é possível [ativar referências em linha](#insert-fragment-references) para outros Fragmentos de conteúdo, marcando a **Permitir referência de fragmento** e configuração do **Modelos de fragmentos do conteúdo permitido**.

Verifique a **Traduzível** , se o conteúdo for localizado. Somente Rich Text e Texto sem formatação podem ser localizados. Consulte [trabalhar com conteúdo localizado para obter mais detalhes](./localized-content.md).

## Resposta em rich text com a API do GraphQL

Ao criar uma consulta do GraphQL, os desenvolvedores podem escolher tipos de resposta diferentes entre `html`, `plaintext`, `markdown`, e `json` de um campo de várias linhas.

Os desenvolvedores podem usar o [Visualização JSON](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html) no editor de Fragmento de conteúdo para mostrar todos os valores do Fragmento de conteúdo atual que podem ser retornados usando a API do GraphQL.

## Consulta persistente do GraphQL

Selecionar o `json` o formato de resposta para o campo de várias linhas oferece mais flexibilidade ao trabalhar com conteúdo rich text. O conteúdo rich text é entregue como uma matriz de tipos de nó JSON que podem ser processados exclusivamente com base na plataforma do cliente.

Abaixo está um tipo de resposta JSON de um campo de várias linhas chamado `main` que contém um parágrafo: &quot;*Este é um parágrafo que inclui **importante**conteúdo.*&quot; onde &quot;importante&quot; está marcado como **negrito**.

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

A variável `$path` variável usada no `_path` O filtro requer o caminho completo para o fragmento de conteúdo (por exemplo, `/content/dam/wknd/en/magazine/sample-article`).

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

Abaixo estão vários exemplos de tipos de resposta de um campo de várias linhas chamado `main` que contém um parágrafo: &quot;Este é um parágrafo que inclui **importante** conteúdo.&quot; onde &quot;importante&quot; é marcado como **negrito**.

+++HTML exemplo

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

A variável `plaintext` A opção de renderização elimina qualquer formatação.

+++


## Renderização de uma resposta JSON de rich text {#render-multiline-json-richtext}

A resposta JSON de rich text do campo de várias linhas é estruturada como uma árvore hierárquica. Cada objeto ou nó representa um bloco HTML diferente do rich text.

Abaixo está uma amostra da resposta JSON de um campo de texto de várias linhas. Observe que cada objeto ou nó inclui um `nodeType` que representa o bloco HTML do rich text como `paragraph`, `link`, e `text`. Cada nó contém, opcionalmente `content` que é um subarray contendo qualquer filho do nó atual.

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

A maneira mais fácil de renderizar a multilinha `json` a resposta é processar cada objeto ou nó na resposta e, em seguida, processar qualquer filho do nó atual. Uma função recursiva pode ser usada para atravessar a árvore JSON.

Abaixo está um exemplo de código que ilustra uma abordagem de travessia recursiva. As amostras são baseadas em JavaScript e usam o [JSX](https://reactjs.org/docs/introducing-jsx.html)No entanto, os conceitos de programação podem ser aplicados a qualquer linguagem.

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

`renderNodeList` é uma função recursiva que utiliza uma matriz de `childNodes`. Cada nó na matriz é passado para uma função `renderNode`, que por sua vez chama `renderNodeList` se o nó tiver filhos.

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

A variável `renderNode` A função espera um único objeto chamado `node`. Um nó pode ter filhos que são processados recursivamente usando o `renderNodeList` descrita acima. Por último, uma `nodeMap` é usado para renderizar o conteúdo do nó com base em sua `nodeType`.

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

A variável `nodeMap` é um literal de objeto JavaScript usado como mapa. Cada uma das &quot;chaves&quot; representa uma chave `nodeType`. Parâmetros de `node` e `children` pode ser passado para as funções resultantes que renderizam o nó. O tipo de retorno usado neste exemplo é JSX. No entanto, a abordagem pode ser adaptada para criar um literal de string que representa o conteúdo HTML.

### Exemplo de código completo

Um utilitário de renderização de rich text reutilizável pode ser encontrado no [Exemplo do WKND GraphQL React](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/utils/renderRichText.js) - utilitário reutilizável que expõe uma função `mapJsonRichText`. Esse utilitário pode ser usado por componentes que desejam renderizar uma resposta JSON de rich text como JSX de reação.
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) - Componente de exemplo que faz uma solicitação do GraphQL que inclui rich text. O componente usa o `mapJsonRichText` para renderizar o rich text e quaisquer referências.


## Adicionar referências embutidas ao rich text {#insert-fragment-references}

O campo Múltiplo permite que os autores insiram imagens ou outros ativos digitais do AEM Assets no fluxo do rich text.

![inserir imagem](assets/rich-text/insert-image.png)

A captura de tela acima representa uma imagem inserida no campo de várias linhas usando o **Inserir ativo** botão.

As referências a outros Fragmentos de conteúdo também podem ser vinculadas ou inseridas no campo de várias linhas usando o **Inserir fragmento de conteúdo** botão.

![Inserir referência do fragmento de conteúdo](assets/rich-text/insert-contentfragment.png)

A captura de tela acima representa outro fragmento de conteúdo, o Guia final para os parques de skate de Los Angeles, sendo inserido no campo de várias linhas. Os tipos de Fragmentos de conteúdo que podem ser inseridos em um campo são controlados pelo **Modelos de fragmentos do conteúdo permitido** configuração no [tipo de dados multilinha](#multi-line-data-type) no Modelo de fragmento de conteúdo.

## Consultar referências em linha com o GraphQL

A API do GraphQL permite que os desenvolvedores criem um query que inclui propriedades adicionais sobre quaisquer referências inseridas em um campo de várias linhas. A resposta JSON inclui uma `_references` objeto que lista essas propriedades extras. A resposta JSON fornece aos desenvolvedores controle total sobre como renderizar as referências ou links, em vez de precisar lidar com HTML opinativo.

Por exemplo, talvez você queira:

* Inclua uma lógica de roteamento personalizada para gerenciar links para outros Fragmentos de conteúdo ao implementar um Aplicativo de página única, como usar o Roteador React ou o Next.js
* Renderize uma imagem em linha usando o caminho absoluto para um ambiente de publicação AEM como a `src` valor.
* Determine como renderizar uma referência incorporada a outro fragmento de conteúdo com propriedades personalizadas adicionais.

Use o `json` tipo de retorno e inclua o `_references` ao criar uma consulta GraphQL:

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

Na consulta acima, a variável `main` é retornado como JSON. A variável `_references` objeto inclui fragmentos para manipular quaisquer referências que sejam do tipo `ImageRef` ou do tipo `ArticleModel`.

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

A resposta JSON inclui onde a referência foi inserida no rich text com o `"nodeType": "reference"`. A variável `_references` O objeto do inclui cada referência de.

## Renderização de referências em linha em rich text

Para renderizar referências em linha, a abordagem recursiva explicada em [Renderização de uma resposta JSON de várias linhas](#render-multiline-json-richtext) pode ser expandido.

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

A abordagem de alto nível consiste em inspecionar sempre que um `nodeType` igual a `reference` na resposta JSON de várias linhas. Uma função de renderização personalizada pode ser chamada, incluindo a variável `_references` objeto retornado na resposta do GraphQL.

O caminho de referência em linha pode ser comparado à entrada correspondente no `_references` objeto e outro mapa personalizado `renderReference` pode ser chamado.

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

A variável `__typename` do `_references` pode ser usado para mapear diferentes tipos de referência para diferentes funções de renderização.

### Exemplo de código completo

Um exemplo completo de como escrever um renderizador de referências personalizado pode ser encontrado em [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) como parte da [Exemplo do WKND GraphQL React](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

## Exemplo completo

>[!VIDEO](https://video.tv.adobe.com/v/342105?quality=12&learn=on)

>[!NOTE]
>
> O vídeo acima usa `_publishUrl` para renderizar a referência da imagem. Em vez disso, prefira `_dynamicUrl` tal como explicado na [instruções sobre imagens otimizadas para a Web](./images.md);


O vídeo anterior mostra um exemplo completo:

1. Atualização de um campo de texto de várias linhas do modelo de fragmento de conteúdo para permitir Referências de fragmento
2. Usar o Editor de fragmentos de conteúdo para incluir uma imagem e fazer referência a outro fragmento em um campo de texto de várias linhas.
3. Criação de uma consulta GraphQL que inclui a resposta de texto multilinha como JSON e qualquer `_references` usado.
4. Escrever um SPA React que renderiza as referências em linha da resposta em rich text.
