---
title: Uso de imagens otimizadas com o AEM Headless
description: Saiba como solicitar URLs de imagem otimizadas com o AEM Headless.
version: Experience Manager as a Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-10253
thumbnail: KT-10253.jpeg
last-substantial-update: 2023-04-19T00:00:00Z
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
duration: 300
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 4%

---

# Imagens otimizadas com o AEM Headless {#images-with-aem-headless}

As imagens são um aspecto crítico do [desenvolvimento de experiências headless avançadas e atraentes do AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=pt-BR). O AEM Headless oferece suporte ao gerenciamento de ativos de imagem e sua entrega otimizada.

Fragmentos de conteúdo usados na modelagem de conteúdo do AEM Headless, geralmente fazem referência a ativos de imagem destinados à exibição na experiência headless. As consultas do GraphQL da AEM podem ser gravadas para fornecer URLs para imagens com base no local de referência da imagem.

O tipo `ImageRef` tem quatro opções de URL para referências de conteúdo:

+ `_path` é o caminho referenciado no AEM e não inclui uma origem AEM (nome do host)
+ `_dynamicUrl` é a URL para a entrega otimizada para a Web do ativo de imagem.
   + O `_dynamicUrl` não inclui uma origem AEM, portanto, o domínio (Autor do AEM ou Serviço de Publicação do AEM) deve ser fornecido pelo aplicativo cliente.
+ `_authorUrl` é a URL completa para o ativo de imagem no Autor do AEM
   + O [AEM Author](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) pode ser usado para fornecer uma experiência de visualização do aplicativo headless.
+ `_publishUrl` é a URL completa para o ativo de imagem na Publicação do AEM
   + A [Publicação do AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) geralmente é de onde a implantação de produção do aplicativo headless exibe imagens.

O `_dynamicUrl` é a URL recomendada para usar na entrega de ativos de imagem e deve substituir o uso de `_path`, `_authorUrl` e `_publishUrl` sempre que possível.

|                                | AEM as a Cloud Service | AEM AS A CLOUD SERVICE RDE | AEM SDK | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| Suporta imagens otimizadas para a Web? | ✔ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="Imagens com o AEM headless"
>abstract="Saiba como o AEM Headless oferece suporte ao gerenciamento de ativos de imagem e sua entrega otimizada."

## Modelo de fragmentos do conteúdo

Verifique se o campo Fragmento de Conteúdo que contém a referência de imagem é do tipo de dados __referência de conteúdo__.

Os tipos de campo são revisados no [Modelo de fragmento de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html), selecionando o campo e inspecionando a guia __Propriedades__ à direita.

![Modelo de fragmento de conteúdo com referência de conteúdo a uma imagem](./assets/images/content-fragment-model.jpeg)

## Consulta persistente do GraphQL

Na consulta GraphQL, retorne o campo como o tipo `ImageRef` e solicite o campo `_dynamicUrl`. Por exemplo, consultar uma aventura no [projeto do Site WKND](https://github.com/adobe/aem-guides-wknd) e incluir a URL da imagem para as referências do ativo da imagem em seu campo `primaryImage`, pode ser feito com uma nova consulta persistente `wknd-shared/adventure-image-by-path` definida como:

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

A variável `$path` usada no filtro `_path` requer o caminho completo para o fragmento de conteúdo (por exemplo, `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`).

O `_assetTransform` define como o `_dynamicUrl` é construído para otimizar a representação da imagem fornecida. URLs de imagens otimizadas para a Web também podem ser ajustadas no cliente alterando os parâmetros de consulta do URL.

| Parâmetro do GraphQL | Descrição | Obrigatório | Valores de variáveis do GraphQL |
|-------------------|------------------------------------------------------------------------------------------------------|----------|-------------------------------------------------------------|
| `format` | O formato do ativo de imagem. | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`, `WEBP`, `WEBPLL`, `WEBPLY` |
| `seoName` | Nome do segmento de arquivo no URL. Se não for fornecido, o nome do ativo de imagem será usado. | ✘ | Alfanumérico, `-` ou `_` |
| `crop` | O quadro de corte retirado da imagem deve estar dentro do tamanho da imagem | ✘ | Inteiros positivos que definem uma região de corte dentro dos limites das dimensões da imagem original |
| `size` | Tamanho da imagem de saída (altura e largura) em pixels. | ✘ | Inteiros positivos |
| `rotation` | Rotação da imagem em graus. | ✘ | `R90`, `R180`, `R270` |
| `flip` | Inverter a imagem. | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` |
| `quality` | Qualidade da imagem em porcentagem da qualidade original. | ✘ | 1-100 |
| `width` | Largura da imagem de saída em pixels. Quando `size` é fornecido, `width` é ignorado. | ✘ | Número inteiro positivo |
| `preferWebP` | Se `true` e o AEM fornecerem um WebP, se o navegador permitir, independentemente do `format`. | ✘ | `true`, `false` |


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

Para carregar a imagem otimizada para a Web da imagem referenciada em seu aplicativo, o usou o `_dynamicUrl` de `primaryImage` como a URL de origem da imagem.

No React, a exibição de uma imagem otimizada para a Web do AEM Publish é semelhante a:

```jsx
// The AEM host is usually read from a environment variable of the SPA.
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

Lembre-se, `_dynamicUrl` não inclui o domínio AEM, portanto, você deve fornecer a origem desejada para a URL da imagem resolver.

## URLs responsivos

O exemplo acima mostra o uso de uma imagem de tamanho único, no entanto, em experiências da Web, conjuntos de imagens responsivas geralmente são necessários. Imagens responsivas podem ser implementadas usando [img srcsets](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) ou [picture elements](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset). O trecho de código a seguir mostra como usar o `_dynamicUrl` como base. `width` é um parâmetro de URL que você pode anexar a `_dynamicUrl` para potencializar diferentes modos de exibição responsivos.

```javascript
// The AEM host is usually read from a environment variable of the SPA.
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

Vamos criar um aplicativo React simples que exiba imagens otimizadas para a Web seguindo [padrões de imagem responsivos](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/). Há dois padrões principais para imagens responsivas:

+ [Elemento de imagem com srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) para aumentar o desempenho
+ [Elemento de imagem](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) para controle de design

### Elemento Img com srcset

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[Elementos de imagem com srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) são usados com o atributo `sizes` para fornecer ativos de imagem diferentes para telas de tamanhos diferentes. Os conjuntos de imagens são úteis ao fornecer ativos de imagem diferentes para tamanhos de tela diferentes.

### Elemento de imagem

[Elementos de imagem](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) são usados com vários elementos `source` para fornecer ativos de imagem diferentes para telas de tamanhos diferentes. Os elementos de imagem são úteis ao fornecer representações de imagem diferentes para tamanhos de tela diferentes.

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

### Exemplo de código

Este aplicativo simples do React usa o [AEM Headless SDK](./aem-headless-sdk.md) para consultar as APIs do AEM Headless quanto a um conteúdo Adventure e exibe a imagem otimizada para a Web usando o [elemento img com srcset](#img-element-with-srcset) e o [elemento picture](#picture-element). O `srcset` e o `sources` usam uma função `setParams` personalizada para anexar o parâmetro de consulta de entrega otimizado para a Web ao `_dynamicUrl` da imagem. Portanto, altere a representação da imagem entregue com base nas necessidades do cliente Web.

A consulta ao AEM é executada no gancho React personalizado [useAdventureByPath que usa o AEM Headless SDK](./aem-headless-sdk.md#graphql-persisted-queries).

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
