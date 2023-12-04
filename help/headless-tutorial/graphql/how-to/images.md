---
title: Uso de imagens otimizadas com AEM Headless
description: Saiba como solicitar URLs de imagem otimizadas com AEM Headless.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-10253
thumbnail: KT-10253.jpeg
last-substantial-update: 2023-04-19T00:00:00Z
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
duration: 449
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '901'
ht-degree: 5%

---

# Imagens otimizadas com o AEM Headless {#images-with-aem-headless}

As imagens são um aspecto crítico do [desenvolver experiências ricas e atraentes com o AEM headless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=pt-BR). O AEM Headless suporta o gerenciamento de ativos de imagem e sua entrega otimizada.

Fragmentos de conteúdo usados na modelagem de conteúdo AEM Headless, geralmente fazem referência a ativos de imagem destinados à exibição na experiência headless. Consultas ao AEM GraphQL podem ser escritas para fornecer URLs a imagens com base no local de referência da imagem.

A variável `ImageRef` O tipo tem quatro opções de URL para referências de conteúdo:

+ `_path` é o caminho referenciado no AEM e não inclui uma origem AEM (nome do host)
+ `_dynamicUrl` é o URL completo do ativo de imagem preferencial otimizado para a Web.
   + A variável `_dynamicUrl` não inclui uma origem de AEM, portanto, o domínio (AEM Author ou AEM Publish service) deve ser fornecido pelo aplicativo cliente.
+ `_authorUrl` é o URL completo do ativo de imagem no AEM Author
   + [Autor do AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) pode ser usado para fornecer uma experiência de visualização do aplicativo headless.
+ `_publishUrl` é o URL completo do ativo de imagem na publicação do AEM
   + [Publicação no AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) O geralmente é o local de onde a implantação em produção do aplicativo headless exibe imagens do.

A variável `_dynamicUrl` é o URL preferencial para usar em ativos de imagem e deve substituir o uso de `_path`, `_authorUrl`, e `_publishUrl` sempre que possível.

|                                | AEM as a Cloud Service | RDE AS A CLOUD SERVICE AEM | SDK do AEM | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| Suporta imagens otimizadas para a Web? | ✔ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="Imagens com o AEM headless"
>abstract="Saiba como o AEM Headless oferece suporte ao gerenciamento de ativos de imagem e sua entrega otimizada."

## Modelo de fragmentos do conteúdo

Verifique se o campo Fragmento de conteúdo que contém a referência da imagem é do __referência de conteúdo__ tipo de dados.

Os tipos de campo são revisados na variável [Modelo de fragmento de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html), selecionando o campo e inspecionando o __Propriedades__ à direita.

![Modelo de fragmento de conteúdo com referência de conteúdo a uma imagem](./assets/images/content-fragment-model.jpeg)

## Consulta persistente do GraphQL

Na consulta do GraphQL, retorne o campo como a variável `ImageRef` e solicite a `_dynamicUrl` campo. Por exemplo, consultar uma aventura na [Projeto do site WKND](https://github.com/adobe/aem-guides-wknd) e incluindo o URL da imagem para as referências do ativo de imagem em sua `primaryImage` , pode ser feita com uma nova consulta persistente `wknd-shared/adventure-image-by-path` definido como:

```graphql {highlight="11"}
query($path: String!, $imageFormat: AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int, $imageQuality: Int) {
  adventureByPath(
    _path: $path
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
    }
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
  "imageFormat": "JPG",
  "imageWidth": 1000,
}
```

A variável `$path` variável usada no `_path` O filtro requer o caminho completo para o fragmento de conteúdo (por exemplo, `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`).

A variável `_assetTransform` define como o `_dynamicUrl` é construído para otimizar a representação da imagem fornecida. URLs de imagens otimizadas para a Web também podem ser ajustadas no cliente alterando os parâmetros de consulta do URL.

| Parâmetro do GraphQL | Parâmetro de URL | Descrição | Obrigatório | Valores de variáveis do GraphQL | Valores de parâmetro de URL | Exemplo de parâmetro de URL |
|:---------|:----------|:-------------------------------|:--:|:--------------------------|:---|:--|
| `format` | N/A | O formato do ativo de imagem. | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`,  `WEBP`, `WEBPLL`, `WEBPLY` | N/A | N/A |
| `seoName` | N/A | Nome do segmento de arquivo no URL. Se não for fornecido, o nome do ativo de imagem será usado. | ✘ | Alfanumérico, `-`ou `_` | N/A | N/A |
| `crop` | `crop` | O quadro de corte retirado da imagem deve estar dentro do tamanho da imagem | ✘ | Inteiros positivos que definem uma região de corte dentro dos limites das dimensões da imagem original | String delimitada por vírgulas de coordenadas numéricas `<X_ORIGIN>,<Y_ORIGIN>,<CROP_WIDTH>,<CROP_HEIGHT>` | `?crop=10,20,300,400` |
| `size` | `size` | Tamanho da imagem de saída (altura e largura) em pixels. | ✘ | Inteiros positivos | Inteiros positivos delimitados por vírgulas na ordem `<WIDTH>,<HEIGHT>` | `?size=1200,800` |
| `rotation` | `rotate` | Rotação da imagem em graus. | ✘ | `R90`, `R180`, `R270` | `90`, `180`, `270` | `?rotate=90` |
| `flip` | `flip` | Inverter a imagem. | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` | `h`, `v`, `hv` | `?flip=h` |
| `quality` | `quality` | Qualidade da imagem em porcentagem da qualidade original. | ✘ | 1-100 | 1-100 | `?quality=80` |
| `width` | `width` | Largura da imagem de saída em pixels. Quando `size` é fornecido `width` é ignorado. | ✘ | Número inteiro positivo | Número inteiro positivo | `?width=1600` |
| `preferWebP` | `preferwebp` | Se `true` e AEM fornece um WebP se o navegador oferecer suporte a ele, independentemente da `format`. | ✘ | `true`, `false` | `true`, `false` | `?preferwebp=true` |

## Resposta do GraphQL

A resposta JSON resultante contém os campos solicitados que contêm o URL otimizado para a Web para os ativos de imagem.

```json {highlight="8"}
{
  "data": {
    "adventureByPath": {
      "item": {
        "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
        "title": "Bali Surf Camp",
        "primaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--a38886f7-4537-4791-aa20-3f6ef0ac3fcd/adobestock_175749320.jpg?preferwebp=true&width=1000&quality=80"
        }
      }
    }
  }
}
```

Para carregar a imagem otimizada para a Web da imagem referenciada em seu aplicativo, o usou o `_dynamicUrl` do `primaryImage` como o URL de origem da imagem.

No React, exibir uma imagem otimizada para a Web do AEM Publish é semelhante a:

```jsx
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

Lembre-se: `_dynamicUrl` O não inclui o domínio AEM, portanto, é necessário fornecer a origem desejada para o URL da imagem resolver.

## URLs responsivos

O exemplo acima mostra o uso de uma imagem de tamanho único, no entanto, em experiências da Web, conjuntos de imagens responsivas geralmente são necessários. Imagens responsivas podem ser implementadas usando [img srcsets](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) ou [elementos de imagem](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset). O trecho de código a seguir mostra como usar o `_dynamicUrl` como um baseado em e anexar diferentes parâmetros de largura para potencializar diferentes exibições responsivas. Não só o `width` O parâmetro de consulta pode ser usado, mas outros parâmetros de consulta podem ser adicionados pelo cliente para otimizar ainda mais o ativo de imagem com base em suas necessidades.

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
    src="${dynamicUrl}&width=1000}"
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

## Exemplo do React

Vamos criar um aplicativo React simples que exibe imagens otimizadas para a Web seguindo [padrões de imagem responsivos](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/). Há dois padrões principais para imagens responsivas:

+ [Elemento Img com srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) para melhor desempenho
+ [Elemento de imagem](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) para controle de design

### Elemento Img com srcset

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[Elementos Img com srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) são usados com o `sizes` para fornecer ativos de imagem diferentes para telas de tamanhos diferentes. Os conjuntos de imagens são úteis ao fornecer ativos de imagem diferentes para tamanhos de tela diferentes.

### Elemento de imagem

[Elementos de imagem](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) são usados com vários `source` elementos para fornecer ativos de imagem diferentes para tamanhos de tela diferentes. Os elementos de imagem são úteis ao fornecer representações de imagem diferentes para tamanhos de tela diferentes.

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

### Exemplo de código

Este aplicativo simples do React usa o [SDK do AEM Headless](./aem-headless-sdk.md) para consultar APIs AEM Headless para um conteúdo Adventure e exibe a imagem otimizada para a Web usando [elemento img com srcset](#img-element-with-srcset) e [elemento de imagem](#picture-element). A variável `srcset` e `sources` usar um personalizado `setParams` para anexar o parâmetro de consulta de entrega otimizada para a Web à `_dynamicUrl` da imagem, altere a representação da imagem entregue com base nas necessidades do cliente da Web.

A consulta ao AEM é executada no gancho React personalizado [useAdventureByPath que usa o SDK AEM Headless](./aem-headless-sdk.md#graphql-persisted-queries).

```javascript
// src/App.js

import "./App.css";
import { useAdventureByPath } from './api/persistedQueries'

const AEM_HOST = process.env.AEM_HOST;

function App() {

  /**
   * Update the dynamic URL with client-specific query parameters
   * @param {*} imageUrl the image URL
   * @param {*} params the AEM web-optimized image query parameters
   * @returns the dynamic URL with the query parameters
   */
  function setOptimizedImageUrlParams(imageUrl, params) {
    let url = new URL(imageUrl);
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
        { imageFormat: "JPG" }
      );

  // Wait for AEM Headless APIs to provide data
  if (!data) { return <></> }

  const alt = data.adventureByPath.item.title;
  const imageUrl =  AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;

  return (
    <div className="app">
      
      <h1>Web-optimized images</h1>

      {/* Render the web-optimized image img with srcset for the Adventure Primary Image */}
      <h2>Img srcset</h2>

      <img
        alt={alt}
        src={setOptimizedImageUrlParams(imageUrl, { width: 1000 })}
        srcSet={
            `${setOptimizedImageUrlParams(imageUrl, { width: 1000 })} 1000w,
             ${setOptimizedImageUrlParams(imageUrl, { width: 1600 })} 1600w,
             ${setOptimizedImageUrlParams(imageUrl, { width: 2000 })} 2000w`
        }
        sizes="calc(100vw - 10rem)"/>

       {/* Render the web-optimized picture for the Adventure Primary Image */}
        <h2>Picture element</h2>

        <picture>
          {/* When viewport width is greater than 2001px */}
          <source srcSet={setOptimizedImageUrlParams(imageUrl, { width : 2600 })} media="(min-width: 2001px)"/>        
          {/* When viewport width is between 1000px and 2000px */}
          <source srcSet={setOptimizedImageUrlParams(imageUrl, { width : 2000})} media="(min-width: 1000px)"/>
          {/* When viewport width is less than 799px */}
          <img src={setOptimizedImageUrlParams(imageUrl, { width : 400, crop: "550,300,400,400" })} alt={alt}/>
        </picture>
    </div>
  );
}

export default App;
```
