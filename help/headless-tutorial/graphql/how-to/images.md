---
title: Uso de imagens com AEM headless
description: Saiba como solicitar URLs de referência de conteúdo de imagem e usar representações personalizadas com AEM Headless.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10253
thumbnail: KT-10253.jpeg
source-git-commit: 4966a48c29ae1b5d0664cb43feeb4ad94f43b4e1
workflow-type: tm+mt
source-wordcount: '1084'
ht-degree: 2%

---


# Imagens com AEM headless

As imagens são um aspecto crítico do [desenvolvimento de experiências ricas e convincentes AEM sem interface](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=pt-BR). AEM Headless suporta o gerenciamento de ativos de imagem e sua entrega otimizada.

Fragmentos de conteúdo usados AEM modelagem de conteúdo sem cabeçalho, geralmente fazem referência a ativos de imagem destinados à exibição na experiência sem cabeçalho. AEM consultas GraphQL podem ser gravadas para fornecer URLs para imagens com base no local de onde a imagem é referenciada.

O `ImageRef` tem três opções de URL para referências de conteúdo:

+ `_path` é o caminho referenciado no AEM e não inclui uma origem AEM (nome do host)
+ `_authorUrl` é o URL completo para o ativo de imagem no Autor do AEM
   + [Autor do AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) pode ser usada para fornecer uma experiência de visualização do aplicativo sem periféricos.
+ `_publishUrl` é o URL completo para o ativo de imagem no AEM Publish
   + [Publicação do AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) geralmente é onde a implantação de produção do aplicativo sem cabeçalho exibe imagens.

Os campos são melhor usados com base nos seguintes critérios:

| Campos ImageRef | Aplicativo Web do cliente fornecido pelo AEM | O aplicativo cliente consulta o autor do AEM | Consultas de aplicativo do cliente AEM Publish |
|--------------------|:------------------------------:|:-----------------------------:|:------------------------------:|
| `_path` | ✔ | ✘ | ✘ |
| `_authorUrl` | ✘ | ✔ | ✘ |
| `_publishUrl` | ✘ | ✘ | ✔ |

Utilização de `_authorUrl` e `_publishUrl` deve estar alinhado com o ponto de extremidade GraphQL da AEM que está sendo usado para gerar a resposta GraphQL.

## Modelo de fragmentos do conteúdo

Verifique se o campo Fragmento do conteúdo que contém a referência da imagem é do __referência de conteúdo__ tipo de dados.

Os tipos de campo são revisados na seção [Modelo de fragmento de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html), selecionando o campo e inspecionando o __Propriedades__ à direita.

![Modelo de fragmento de conteúdo com referência de conteúdo para uma imagem](./assets/images/content-fragment-model.jpeg)

## Consulta GraphQL

Na query GraphQL, retorne o campo como o `ImageRef` e solicitar os campos apropriados `_path`, `_authorUrl`ou `_publishUrl` exigido pelo seu aplicativo.

```javascript
{
  adventureByPath(_path: "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp") {
    item {
      adventurePrimaryImage {
        ... on ImageRef {
          _path,
          _authorUrl,
          _publishUrl
        }
      }
    }
  }  
}
```

## Resposta GraphQL

A resposta JSON resultante contém os campos solicitados contendo os URLs para os ativos de imagem.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/AdobeStock_175749320.jpg",
          "_authorUrl": "https://author-p123-e456.adobeaemcloud.com/content/dam/wknd/en/adventures/bali-surf-camp/AdobeStock_175749320.jpg",
          "_publishUrl": "https://publish-p123-e789.adobeaemcloud.com/content/dam/wknd/en/adventures/bali-surf-camp/AdobeStock_175749320.jpg"
        }
      }
    }
  }
}
```

Para carregar a imagem referenciada em seu aplicativo, use o campo apropriado, `_path`, `_authorUrl`ou `_publishUrl` do `adventurePrimaryImage` como o URL de origem da imagem.

Os domínios da `_authorUrl` e `_publishUrl` são automaticamente definidas por AEM as a Cloud Service usando a variável [Externalizar](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/externalizer.htmli).

No React, a exibição da imagem da publicação do AEM é semelhante a:

```html
<img src={ data.adventureByPath.item.adventurePrimaryImage._publishUrl } />
```

## Representações de imagem

Suporte a ativos de imagem personalizáveis [representações](../../../assets/authoring/renditions.md), que são representações alternativas do ativo original. As representações personalizadas podem ajudar na otimização de uma experiência sem cabeçalho. Em vez de solicitar o ativo de imagem original, que geralmente é um arquivo grande de alta resolução, as representações otimizadas podem ser solicitadas pelo aplicativo sem cabeçalho.

### Criar representações

Os administradores do AEM Assets definem as representações personalizadas usando Perfis de processamento. Os Perfis de processamento podem ser aplicados a árvores ou ativos de pasta específicos diretamente para gerar as representações desses ativos.

#### Processando perfis

As especificações de representações de ativos são definidas em [Processando perfis](../../../assets/configuring//processing-profiles.md) pelos administradores do AEM Assets.

Crie ou atualize um Perfil de processamento e adicione definições de representação para os tamanhos de imagem necessários para o aplicativo sem cabeçalho. As representações podem ter qualquer nome, mas devem ser nomeadas semanticamente.

![AEM representações otimizadas para cabeçalho](./assets/images/processing-profiles.jpg)

Neste exemplo, três renderizações são criadas:

| Nome da representação | Extensão | Largura máxima |
|----------------|:---------:|----------:|
| grande | jpeg | 1200px |
| médio | jpeg | 900px |
| pequena | jpeg | 600px |

Os atributos chamados na tabela acima são importantes:

+ __Nome da representação__ é usada para solicitar a representação.
+ __Extensão__ é a extensão usada para solicitar a variável __nome da representação__.
+ __Largura máxima__ é usada para informar ao desenvolvedor qual renderização deve ser usada com base em seu uso no aplicativo sem periféricos.

As definições de representação dependem das necessidades do aplicativo sem periféricos, portanto, defina a representação ideal definida para o caso de uso e seja nomeada semanticamente em relação a como estão sendo usadas.

#### Reprocessar ativos{#reprocess-assets}

Com o Perfil de processamento criado (ou atualizado), reprocesse os ativos para gerar as novas representações definidas no Perfil de processamento. Se os ativos não forem processados com as novas representações, elas não existirão.

+ De preferência, [atribuído o Perfil de processamento a uma pasta](../../../assets/configuring//processing-profiles.md) para que qualquer novo ativo carregado nessa pasta gere automaticamente as representações. Os ativos existentes devem ser reprocessados usando a abordagem ad hoc abaixo.

+ Ou, ad-hoc, selecionando uma pasta ou ativo, selecionando __Reprocessar ativos__ e selecionando o novo nome do Perfil de processamento.

   ![Ad-hoc reprocessar ativos](./assets/images/ad-hoc-reprocess-assets.jpg)

#### Revisar representações

As representações podem ser validadas por [abrir a exibição de representações de um ativo](../../../assets/authoring/renditions.md)e selecionar as novas representações para visualização no painel de representações. Se as renderizações estiverem ausentes, [verifique se os ativos são processados usando o Perfil de processamento](#reprocess-assets).

![Revisar representações](./assets/images/review-renditions.jpg)

#### Publicar ativos

Certifique-se de que os ativos com as novas representações sejam [(re)publicado](../../../assets/sharing/publish.md) assim, as novas representações são acessíveis no AEM Publish.

### Acessar representações

As representações são acessadas diretamente ao anexar a variável __nomes de representação__ e __extensões de representação__ definido no Perfil de processamento no URL do ativo.

| URL do ativo | Subcaminho de representações | Nome da representação | Extensão de representação |  | URL de representação |
|-----------|:------------------:|:--------------:|--------------------:|:--:|---|
| https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpg | /_jcr_content/renditions/ | grande | .jpeg | → | https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpg/_jcr_content/renditions/large.jpeg |
| https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpg | /_jcr_content/renditions/ | médio | .jpeg | → | https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpg/_jcr_content/renditions/medium.jpeg |
| https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpg | /_jcr_content/renditions/ | pequena | .jpeg | → | https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpg/_jcr_content/renditions/small.jpeg |

{style=&quot;table-layout:auto&quot;}

### Consulta GraphQL{#renditions-graphl-query}

AEM GraphQL não requer sintaxe extra para solicitar representações de imagem. Em vez disso [imagens são consultadas](#images-graphql-query) da maneira habitual, e a representação desejada é especificada no código. É importante [garantir que os ativos de imagem usados pelo aplicativo sem cabeçalho tenham as mesmas representações nomeadas](#reprocess-assets).

### Exemplo de reação

Vamos criar um aplicativo React simples que exibe três representações, pequenas, médias e grandes, de um único ativo de imagem.

![Representações de ativos de imagem Exemplo de reação](./assets/images/react-example-renditions.jpg)

#### Criar componente de imagem{#react-example-image-component}

Crie um componente React que renderize as imagens. Esse componente aceita quatro propriedades:

+ `assetUrl`: O URL do ativo de imagem, conforme fornecido pela resposta da consulta GraphQL.
+ `renditionName`: O nome da representação a ser carregada.
+ `renditionExtension`: A extensão da representação a ser carregada.
+ `alt`: O texto alternativo da imagem; acessibilidade é importante!

Esse componente constrói o [URL de representação usando o formato descrito em __Acessar representações__](#access-renditions). Um `onError` O manipulador é definido para exibir o ativo original no caso de a representação estar ausente.

Este exemplo usa o url do ativo original como fallback na `onError` manipulador, no evento, uma representação está ausente.

```javascript
// src/Image.js

export default function Image({ assetUrl, renditionName, renditionExtension, alt }) {
  // Construct the rendition Url in the format:
  //   <ASSET URL>/_jcr_content/renditions<RENDITION NAME>.<RENDITION EXTENSION>
  const renditionUrl = `${assetUrl}/_jcr_content/renditions/${renditionName}.${renditionExtension}`;

  // Load the original image asset in the event the named rendition is missing
  const handleOnError = (e) => { e.target.src = assetUrl; }

  return (
    <>
      <img src={renditionUrl} 
            alt={alt} 
            onError={handleOnError}/>
    </>
  );
}
```

#### Defina as `App.js`{#app-js}

Este simples `App.js` consultas AEM uma imagem da Aventura e exibem as três representações dessa imagem: pequeno, médio e grande.

A consulta contra AEM é realizada no gancho React personalizado [useGraphQL que usa o SDK sem cabeçalho do AEM](./aem-headless-sdk.md#graphql-queries).

Os resultados da query e os parâmetros de representação específicos são passados para a função [Componente de reação de imagem](#react-example-image-component).

```javascript
// src/App.js

import "./App.css";
import { useGraphQL } from "./useGraphQL";
import Image from "./Image";

function App() {

  // The GraphQL that returns an image
  const adventureQuery = `{
        adventureByPath(_path: "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp") {
          item {
            adventureTitle,
            adventurePrimaryImage {
              ... on ImageRef {
                _path,
                _authorUrl,
                _publishUrl
              }
            }
          }
        }  
    }`;

  // Get data from AEM using GraphQL
  let { data } = useGraphQL(adventureQuery);

  // Wait for GraphQL to provide data
  if (!data) { return <></> }

  return (
    <div className="app">
      
      <h2>Small rendition</h2>
      {/* Render the small rendition for the Adventure Primary Image */}
      <Image
        assetUrl={data.adventureByPath.item.adventurePrimaryImage._publishUrl}
        renditionName="small"
        renditionExtension="jpeg"
        alt={data.adventureByPath.item.adventureTitle}
      />

      <hr />

      <h2>Medium rendition</h2>
      {/* Render the medium rendition for the Adventure Primary Image */}
      <Image
        assetUrl={data.adventureByPath.item.adventurePrimaryImage._publishUrl}
        renditionName="medium"
        renditionExtension="jpeg"
        alt={data.adventureByPath.item.adventureTitle}
      />

      <hr />

      <h2>Large rendition</h2>
      {/* Render the large rendition for the Adventure Primary Image */}
      <Image
        assetUrl={data.adventureByPath.item.adventurePrimaryImage._publishUrl}
        renditionName="large"
        renditionExtension="jpeg"
        alt={data.adventureByPath.item.adventureTitle}
      />
    </div>
  );
}

export default App;
```
