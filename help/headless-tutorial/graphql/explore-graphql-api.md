---
title: Explorar APIs GraphQL - Introdução ao AEM sem cabeçalho - GraphQL
description: Introdução ao Adobe Experience Manager (AEM) e ao GraphQL. Explore AEM APIs GraphQL usando o IDE GrapiQL integrado. Saiba como AEM automaticamente gera um schema GraphQL com base em um modelo de Fragmento de conteúdo. Experimente a construção de query básicos usando a sintaxe GraphQL.
sub-product: ativos
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6714
thumbnail: KT-6714.jpg
translation-type: tm+mt
source-git-commit: bfcc9dbb70753f985a2e47f329dbb9f43f5805e2
workflow-type: tm+mt
source-wordcount: '1039'
ht-degree: 0%

---


# Explore as APIs GraphQL {#explore-graphql-apis}

>[!CAUTION]
>
> A API AEM GraphQL para o Delivery de fragmentos de conteúdo está disponível sob solicitação.
> Entre em contato com o Suporte ao Adobe para habilitar a API do seu AEM como programa Cloud Service.

A API GraphQL do AEM fornece uma linguagem de query avançada para expor dados de Fragmentos de conteúdo a aplicativos downstream. Os modelos de Fragmento de conteúdo definem o schema de dados usado pelos Fragmentos de conteúdo. Sempre que um Modelo de fragmento de conteúdo é criado ou atualizado, o schema é convertido e adicionado ao &quot;gráfico&quot; que compõe a API do GraphQL.

Neste capítulo, exploraremos alguns query GraphQL comuns para coletar conteúdo. Incorporado ao AEM é um IDE chamado [GraphiQL](https://github.com/graphql/graphiql). O GraphiQL IDE permite testar e refinar rapidamente os query e os dados retornados. O GraphiQL também fornece acesso fácil à documentação, facilitando o aprendizado e a compreensão de quais métodos estão disponíveis.

## Pré-requisitos {#prerequisites}

Este é um tutorial de várias partes e presume-se que as etapas descritas em [Criação de fragmentos de conteúdo](./author-content-fragments.md) foram concluídas.

## Objetivos {#objectives}

* Saiba como usar a ferramenta GraphiQL para construir um query usando a sintaxe GraphQL.
* Saiba como query uma lista de Fragmentos de conteúdo e um único Fragmento de conteúdo.
* Saiba como filtrar e solicitar atributos de dados específicos.
* Saiba como query uma variação de um Fragmento de conteúdo.
* Saiba como participar de um query de vários modelos de Fragmento de conteúdo

## Query de uma lista de fragmentos de conteúdo {#query-list-cf}

Um requisito comum será o query de vários Fragmentos de conteúdo.

1. Navegue até GraphiQL IDE em [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html).
1. Cole o seguinte query no painel esquerdo (abaixo da lista de comentários):

   ```graphql
   {
     contributorList {
       items {
           _path
         }
     }
   }
   ```

1. Pressione o botão **Reproduzir** no menu superior para executar o query. Você deve ver os resultados dos fragmentos de conteúdo dos Colaboradores do capítulo anterior:

   ![Resultados da Lista do contribuidor](assets/explore-graphql-api/contributorlist-results.png)

1. Posicione o cursor abaixo do texto `_path` e digite **CTRL+Space** para acionar a dica de código. Adicione `fullName` e `occupation` ao query.

   ![Atualizar Query com ocorrências de código](assets/explore-graphql-api/update-query-codehinting.png)

1. Execute o query novamente pressionando o botão **Reproduzir** e você deverá ver que os resultados incluem as propriedades adicionais de `fullName` e `occupation`.

   ![Resultados do nome e da ocupação](assets/explore-graphql-api/updated-query-fullname-occupation.png)

   `fullName` e  `occupation` são propriedades simples. Lembre-se do capítulo [Definindo modelos de fragmento de conteúdo](./content-fragment-models.md) que `fullName` e `occupation` são os valores usados ao definir **Nome da propriedade** dos respectivos campos.

1. `pictureReference` e  `biographyText` representam campos mais complexos. Atualize o query com o seguinte para retornar dados sobre os campos `pictureReference` e `biographyText`.

   ```graphql
   {
   contributorList {
       items {
         _path
         fullName
         occupation
         biographyText {
           html
         }
         pictureReference {
           ... on ImageRef {
               _path
               width
               height
               }
           }
       }
     }
   }
   ```

   `biographyText` é um campo de texto de várias linhas e a API do GraphQL nos permite escolher diversos formatos para os resultados, como  `html`,  `markdown`ou  `json`   `plaintext`.

   `pictureReference` é uma referência de conteúdo e espera-se que seja uma imagem, portanto,  `ImageRef` o objeto incorporado é usado. Isso nos permite solicitar dados adicionais sobre a imagem que está sendo referenciada, como `width` e `height`.

1. Em seguida, experimente consultar uma lista de **Aventuras**. Execute o seguinte query:

   ```graphql
   {
     adventureList {
       items {
         adventureTitle
         adventureType
         adventurePrimaryImage {
           ...on ImageRef {
             _path
             mimeType
           }
         }
       }
     }
   }
   ```

   Você deve ver uma lista de **Aventuras** retornada. Sinta-se à vontade para experimentar adicionando campos adicionais ao query.

## Filtrar uma Lista de fragmentos de conteúdo {#filter-list-cf}

Em seguida, vamos observar como é possível filtrar os resultados para um subconjunto de Fragmentos de conteúdo com base em um valor de propriedade.

1. Digite o seguinte query na interface do usuário do GraphiQL:

   ```graphql
   {
   contributorList(filter: {
     occupation: {
       _expressions: {
         value: "Photographer"
         }
       }
     }) {
       items {
         _path
         fullName
         occupation
       }
     }
   }
   ```

   O query acima realiza uma pesquisa em relação a todos os contribuidores no sistema. O filtro adicionado ao início do query executará uma comparação no campo `occupation` e na string &quot;**Fotógrafo**&quot;.

1. Execute o query, espera-se que apenas um único **Contributor** seja retornado.
1. Digite o seguinte query para query de uma lista de **Aventuras** onde `adventureActivity` é **diferente** igual a **&quot;Surfing&quot;**:

   ```graphql
   {
     adventureList(filter: {
       adventureActivity: {
           _expressions: {
               _operator: EQUALS_NOT
               value: "Surfing"
           }
       }
   }) {
       items {
       _path
       adventureTitle
       adventureActivity
       }
     }
   }
   ```

1. Execute o query e inspecione os resultados. Observe que nenhum dos resultados inclui um `adventureType` igual a **&quot;Surfing&quot;**.

Há muitas outras opções para filtrar e criar query complexos, acima estão apenas alguns exemplos.

## Query de um único fragmento de conteúdo {#query-single-cf}

Também é possível query direto de um único fragmento de conteúdo. O conteúdo no AEM é armazenado de maneira hierárquica e o identificador exclusivo de um fragmento é baseado no caminho do fragmento. Se o objetivo for retornar dados sobre um único fragmento, é preferível usar o caminho e query o modelo diretamente. Usar essa sintaxe significa que a complexidade do query será muito baixa e gerará um resultado mais rápido.

1. Digite o seguinte query no editor do GraphiQL:

   ```graphql
   {
    contributorByPath(_path: "/content/dam/wknd/en/contributors/stacey-roswells") {
       item {
         _path
         fullName
         biography {
           html
         }
       }
     }
   }
   ```

1. Execute o query e observe que o único resultado do fragmento **Stacey Roswells** é retornado.

   No exercício anterior, você usava um filtro para restringir uma lista de resultados. Você pode usar uma sintaxe semelhante para filtrar por caminho, no entanto, a sintaxe acima é preferida por motivos de desempenho.

1. Lembre-se no capítulo [Criação de fragmentos de conteúdo](./author-content-fragments.md) de que uma variação **Summary** foi criada para **Stacey Roswells**. Atualize o query para retornar a variação **Summary**:

   ```graphql
   {
   contributorByPath
   (
       _path: "/content/dam/wknd/en/contributors/stacey-roswells"
       variation: "summary"
   ) {
       item {
         _path
         fullName
         biography {
           html
         }
       }
     }
   }
   ```

   Embora a variação tenha sido nomeada como **Summary**, as variações são mantidas em minúsculas e, portanto, `summary` é usado.

1. Execute o query e observe que o campo `biography` contém um resultado `html` muito menor.

## Query para vários modelos de fragmento de conteúdo {#query-multiple-models}

Também é possível combinar query separados em um único query. Isso é útil para minimizar o número de solicitações HTTP necessárias para alimentar o aplicativo. Por exemplo, a visualização *Home* de um aplicativo pode exibir conteúdo com base em **dois** diferentes Modelos de fragmento de conteúdo. Em vez de executar **dois** query separados, podemos combinar os query em uma única solicitação.

1. Digite o seguinte query no editor do GraphiQL:

   ```graphql
   {
     adventureList {
       items {
         _path
         adventureTitle
       }
     }
     contributorList {
       items {
         _path
         fullName
       }
     }
   }
   ```

1. Execute o query e observe que o conjunto de resultados contém dados de **Aventuras** e **Colaboradores**:

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
          "adventureTitle": "Bali Surf Camp"
        },
        {
          "_path": "/content/dam/wknd/en/adventures/beervana-portland/beervana-in-portland",
          "adventureTitle": "Beervana in Portland"
        },
        ...
      ]
    },
    "contributorList": {
      "items": [
        {
          "_path": "/content/dam/wknd/en/contributors/jacob-wester",
          "fullName": "Jacob Wester"
        },
        {
          "_path": "/content/dam/wknd/en/contributors/stacey-roswells",
          "fullName": "Stacey Roswells"
        }
      ]
    }
  }
}
```

## Recursos adicionais

Para obter mais exemplos de query GraphQL, consulte: [Aprendendo a usar o GraphQL com AEM - Conteúdo de amostra e Query](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/content-fragments-graphql-samples.html).

## Parabéns! {#congratulations}

Parabéns, você acabou de criar e executar vários query GraphQL!

## Próximas etapas {#next-steps}

No próximo capítulo, [Consultando AEM de um aplicativo React](./graphql-and-external-app.md), você explorará como um aplicativo externo pode query AEM pontos finais GraphQL. O aplicativo externo que modifica o aplicativo de amostra WKND GraphQL React para adicionar query GraphQL filtrados, permitindo que o usuário do aplicativo filtre aventuras por atividade. Você também será apresentado a alguma manipulação básica de erros.
