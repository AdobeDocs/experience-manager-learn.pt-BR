---
title: Uso de imagens otimizadas com AEM headless
description: Saiba como solicitar URLs de imagem otimizadas com AEM Headless.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10253
thumbnail: KT-10253.jpeg
last-substantial-update: 2023-04-19T00:00:00Z
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
source-git-commit: 2096c207ce14985b550b055ea0f51451544c085c
workflow-type: tm+mt
source-wordcount: '918'
ht-degree: 3%

---

# Imagens otimizadas com AEM headless {#images-with-aem-headless}

As imagens são um aspecto crítico do [desenvolvimento de experiências ricas e convincentes AEM sem interface](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=pt-BR). AEM Headless suporta o gerenciamento de ativos de imagem e sua entrega otimizada.

Fragmentos de conteúdo usados AEM modelagem de conteúdo sem cabeçalho, geralmente fazem referência a ativos de imagem destinados à exibição na experiência sem cabeçalho. AEM consultas do GraphQL podem ser escritas para fornecer URLs para imagens com base em onde a imagem é referenciada.

O `ImageRef` tem quatro opções de URL para referências de conteúdo:

+ `_path` é o caminho referenciado no AEM e não inclui uma origem AEM (nome do host)
+ `_dynamicUrl` é o URL completo do ativo de imagem preferido, otimizado para a Web.
   + O `_dynamicUrl` não inclui uma origem AEM, portanto, o domínio (AEM Author ou AEM Publish service) deve ser fornecido pelo aplicativo cliente.
+ `_authorUrl` é o URL completo para o ativo de imagem no Autor do AEM
   + [Autor do AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) pode ser usada para fornecer uma experiência de visualização do aplicativo sem periféricos.
+ `_publishUrl` é o URL completo para o ativo de imagem no AEM Publish
   + [Publicação do AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) geralmente é onde a implantação de produção do aplicativo sem cabeçalho exibe imagens.

O `_dynamicUrl` é o URL preferencial para usar em ativos de imagem e deve substituir o uso de `_path`, `_authorUrl`e `_publishUrl` sempre que possível.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="Imagens com AEM headless"
>abstract="Saiba como o AEM Headless suporta o gerenciamento de ativos de imagem e seu delivery otimizado."

## Modelo de fragmentos do conteúdo

Verifique se o campo Fragmento do conteúdo que contém a referência da imagem é do __referência de conteúdo__ tipo de dados.

Os tipos de campo são revisados na seção [Modelo de fragmento de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html), selecionando o campo e inspecionando o __Propriedades__ à direita.

![Modelo de fragmento de conteúdo com referência de conteúdo para uma imagem](./assets/images/content-fragment-model.jpeg)

## Consulta persistente do GraphQL

Na query de GraphQL, retorne o campo como o `ImageRef` e solicite o `_dynamicUrl` campo. Por exemplo, querendo uma aventura no [Projeto de Site WKND](https://github.com/adobe/aem-guides-wknd) e incluindo o URL da imagem para as referências do ativo de imagem em sua `primaryImage` , pode ser feito com uma nova consulta persistente `wknd-shared/adventure-image-by-path` definido como:

```graphql
query($path: String!, $assetTransform: AssetTransform!) {
  adventureByPath(
    _path: $path
    _assetTransform: $assetTransform
  ) {
    item {
      _path
      title
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

### Variáveis de consulta

```json
{ 
  "path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
  "assetTransform": { "format": "JPG", "quality": 80, "preferWebp": true}
}
```

O `$path` usada na variável `_path` o filtro requer o caminho completo para o fragmento de conteúdo (por exemplo, `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`).

O `_assetTransform` define como o `_dynamicUrl` é construída para otimizar a representação da imagem servida. As URLs de imagens otimizadas para a Web também podem ser ajustadas no cliente alterando os parâmetros de consulta do URL.

| Parâmetro GraphQL | Parâmetro de URL | Descrição | Obrigatório | Valores de variável GraphQL | Valores de parâmetro de URL | Exemplo de variável GraphQL | Exemplo de parâmetro de URL |
|:---------|:----------|:-------------------------------|:--:|:--------------------------|:---|:---|:--|
| `format` | `format` | O formato do ativo de imagem. | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`,  `WEBP`, `WEBPLL`, `WEBPLY` | N/A | `{ format: JPG }` | N/A |
| `seoName` | N/A | Nome do segmento de arquivo no URL. Se não fornecido, o nome do ativo de imagem será usado. | ✘ | Alfanumérico, `-`ou `_` | N/A | `{ seoName: "bali-surf-camp" }` | N/A |
| `crop` | `crop` | O quadro de corte retirado da imagem deve estar dentro do tamanho da imagem | ✘ | Números inteiros positivos que definem uma região de corte dentro dos limites das dimensões da imagem original | String delimitada por vírgulas de coordenadas numéricas `<X_ORIGIN>,<Y_ORIGIN>,<CROP_WIDTH>,<CROP_HEIGHT>` | `{ crop: { xOrigin: 10, yOrigin: 20, width: 300, height: 400} }` | `?crop=10,20,300,400` |
| `size` | `size` | Tamanho da imagem de saída (altura e largura) em pixels. | ✘ | Inteiros positivos | Inteiros positivos delimitados por vírgulas na ordem `<WIDTH>,<HEIGHT>` | `{ size: { width: 1200, height: 800 } }` | `?size=1200,800` |
| `rotation` | `rotate` | Rotação da imagem em graus. | ✘ | `R90`, `R180`, `R270` | `90`, `180`, `270` | `{ rotation: R90 }` | `?rotate=90` |
| `flip` | `flip` | Inverta a imagem. | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` | `h`, `v`, `hv` | `{ flip: horizontal }` | `?flip=h` |
| `quality` | `quality` | Qualidade da imagem em porcentagem da qualidade original. | ✘ | 1-100 | 1-100 | `{ quality: 80 }` | `?quality=80` |
| `width` | `width` | Largura da imagem de saída em pixels. When `size` é fornecido `width` é ignorada. | ✘ | Número inteiro positivo | Número inteiro positivo | `{ width: 1600 }` | `?width=1600` |
| `preferWebP` | `preferwebp` | If `true` e AEM um WebP, se o navegador oferece suporte a ele, independentemente do `format`. | ✘ | `true`, `false` | `true`, `false` | `{ preferWebp: true }` | `?preferwebp=true` |

## Resposta do GraphQL

A resposta JSON resultante contém os campos solicitados contendo o URL otimizado para a Web para os ativos de imagem.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
        "title": "Bali Surf Camp",
        "primaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--a38886f7-4537-4791-aa20-3f6ef0ac3fcd/adobestock_175749320.jpg?preferwebp=true&quality=80"
        }
      }
    }
  }
}
```

Para carregar a imagem otimizada para a Web da imagem referenciada em seu aplicativo, use o `_dynamicUrl` do `primaryImage` como o URL de origem da imagem.

No React, a exibição de uma imagem otimizada para a Web na AEM Publish é semelhante a:

```jsx
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

Lembre-se, `_dynamicUrl` não inclui o domínio AEM, portanto, você deve fornecer a origem desejada para que o URL da imagem seja resolvido.

### URLs responsivos

O exemplo acima é exibido usando uma imagem de tamanho único, no entanto, em experiências da Web, conjuntos de imagens responsivos geralmente são necessários. Imagens responsivas podem ser implementadas usando [img srcsets](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) ou [elementos de imagem](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset). O trecho de código a seguir mostra como usar o `_dynamicUrl` como base e anexe diferentes parâmetros de largura para alimentar diferentes visualizações responsivas. Não apenas a função `width` parâmetro de consulta pode ser usado, mas outros parâmetros de consulta podem ser adicionados pelo cliente para otimizar ainda mais o ativo da imagem com base em suas necessidades.

```javascript
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
// Read the data from GraphQL response
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
let alt = data.adventureByPath.item.title;
...
{/*-- Example img srcset --*/}
document.body.innerHTML=`<img>
    alt="${alt}"
    src="${${dynamicUrl}&width=1000}"
    srcset="`
      ${dynamicUrl}&width=1000 1000w,
      ${dynamicUrl}&width=1600 1600w,
      ${dynamicUrl}&width=2000 2000w,
      `"
    sizes="calc(100vw - 10rem)"/>`;
...
{/*-- Example picture --*/}
document.body.innerHTML=`<picture>
      <source srcset="${dynamicUrl}&width=2600" media="(min-width: 2001px)"/>
      <source srcset="${dynamicUrl}&width=2000" media="(min-width: 1000px)"/>
      <img src="${dynamicUrl}&width=400" alt="${alt}"/>
    </picture>`;
```

### Exemplo de reação

Vamos criar um aplicativo React simples que exibe imagens otimizadas para a Web seguindo [padrões de imagem responsivos](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/). Existem dois padrões principais para imagens responsivas:

+ [Elemento Img com srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) para maior desempenho
+ [Elemento de imagem](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) para controle de design

#### Elemento Img com srcset

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[Img elements com o srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) são usados com o `sizes` para fornecer ativos de imagem diferentes para tamanhos de tela diferentes. Os conjuntos de imagens são úteis ao fornecer ativos de imagem diferentes para tamanhos de tela diferentes.

#### Elemento de imagem

[Elementos de imagem](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) são usados com vários `source` elementos para fornecer ativos de imagem diferentes para tamanhos de tela diferentes. Os elementos de imagem são úteis ao fornecer representações de imagem diferentes para tamanhos de tela diferentes.

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

#### Exemplo de código

Este aplicativo React simples usa a variável [SDK sem cabeçalho AEM](./aem-headless-sdk.md) para consultar AEM APIs headless para um conteúdo do Adventure e exibir a imagem otimizada para a Web usando [elemento img com srcset](#img-element-with-srcset) e [elemento de imagem](#picture-element). O `srcset` e `sources` usar um `setParams` para anexar o parâmetro de consulta de delivery otimizado para a Web à função `_dynamicUrl` da imagem, altere a representação da imagem entregue com base nas necessidades do cliente Web.

A consulta contra AEM é realizada no gancho React personalizado [useAdventureByPath que usa o SDK sem cabeçalho AEM](./aem-headless-sdk.md#graphql-persisted-queries).

```javascript
// src/App.js

import "./App.css";
import { useAdventureByPath } from './api/persistedQueries'

const AEM_HOST = process.env.AEM_HOST;

function App() {

  /**
   * Update the dynamic URL with client-specific query parameters
   * @param {*} dynamicUrl the base dynamic URL for the web-optimized image
   * @param {*} params the AEM web-optimized image query parameters
   * @returns the dynamic URL with the query parameters
   */
  function setParams(dynamicUrl, params) {
    let url = new URL(dynamicUrl);
    Object.keys(params).forEach(key => {
      url.searchParams.set(key, params[key]);
    });
    return url.toString();
  }

  // Get data from AEM using GraphQL persisted query as defined above 
  // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
  // The 2nd parameter define the base GraphQL query parameters used to request the web-optimized image
  let { data, error } = useAdventureByPath(
        "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp", 
        { assetTransform: { format: "JPG", preferWebp: true } }
      );

  // Wait for AEM Headless APIs to provide data
  if (!data) { return <></> }

  return (
    <div className="app">
      
      <h1>Web-optimized images</h1>

      {/* Render the web-optimized image img with srcset for the Adventure Primary Image */}
      <h2>Img srcset</h2>

      <img
        alt={alt}
        src={setParams(dynamicUrl, { width: 1000 })}
        srcSet={
            `${setParams(dynamicUrl, { width: 1000 })} 1000w,
             ${setParams(dynamicUrl, { width: 1600 })} 1600w,
             ${setParams(dynamicUrl, { width: 2000 })} 2000w`
        }
        sizes="calc(100vw - 10rem)"/>

       {/* Render the web-optimized picture for the Adventure Primary Image */}
        <h2>Picture element</h2>

        <picture>
          {/* When viewport width is greater than 2001px */}
          <source srcSet={setParams(dynamicUrl, { width : 2600 })} media="(min-width: 2001px)"/>        
          {/* When viewport width is between 1000px and 2000px */}
          <source srcSet={setParams(dynamicUrl, { width : 2000})} media="(min-width: 1000px)"/>
          {/* When viewport width is less than 799px */}
          <img src={setParams(dynamicUrl, { width : 400, crop: "550,300,400,400" })} alt={alt}/>
        </picture>
    </div>
  );
}

export default App;
```
