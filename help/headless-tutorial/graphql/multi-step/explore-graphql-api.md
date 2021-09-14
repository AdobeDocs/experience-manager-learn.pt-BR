---
title: Explore APIs GraphQL - Introdução ao AEM Headless - GraphQL
description: Introdução à Adobe Experience Manager (AEM) e GraphQL. Explore AEM APIs GraphQL usando o GrapiQL IDE integrado. Saiba como o AEM gera automaticamente um esquema GraphQL com base em um modelo de Fragmento de conteúdo. Experimente construir consultas básicas usando a sintaxe GraphQL.
version: Cloud Service
mini-toc-levels: 1
kt: 6714
thumbnail: KT-6714.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 508b0211-fa21-4a73-b8b4-c6c34e3ba696
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1133'
ht-degree: 1%

---

# Explorar APIs GraphQL {#explore-graphql-apis}

A API GraphQL do AEM fornece uma linguagem de consulta avançada para expor os dados dos Fragmentos de conteúdo aos aplicativos de downstream. Os modelos de Fragmento de conteúdo definem o schema de dados usado pelos Fragmentos de conteúdo. Sempre que um Modelo de fragmento de conteúdo é criado ou atualizado, o esquema é traduzido e adicionado ao &quot;gráfico&quot; que compõe a API GraphQL.

Neste capítulo, exploraremos algumas consultas GraphQL comuns para coletar conteúdo usando um IDE chamado [GraphiQL](https://github.com/graphql/graphiql). O GraphiQL IDE permite testar e refinar rapidamente as consultas e os dados retornados. O GraphiQL também oferece acesso fácil à documentação, facilitando o aprendizado e a compreensão de quais métodos estão disponíveis.

## Pré-requisitos {#prerequisites}

Este é um tutorial de várias partes e presume-se que as etapas descritas em [Criação de fragmentos de conteúdo](./author-content-fragments.md) foram concluídas.

## Objetivos {#objectives}

* Saiba como usar a ferramenta GraphiQL para criar uma consulta usando a sintaxe GraphQL.
* Saiba como consultar uma lista de Fragmentos de conteúdo e um único Fragmento de conteúdo.
* Saiba como filtrar e solicitar atributos de dados específicos.
* Saiba como consultar uma variação de um Fragmento de conteúdo.
* Saiba como associar-se a uma consulta de vários modelos de Fragmento de conteúdo

## Instalação da ferramenta GraphiQL {#install-graphiql}

O GraphiQL IDE é uma ferramenta de desenvolvimento e é necessário apenas em ambientes de nível inferior, como uma instância de desenvolvimento ou local. Por conseguinte, não está incluído no projeto de AEM, mas constitui um pacote separado que pode ser instalado numa base ad hoc.

1. Navegue até o **[Portal de distribuição de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM como um Cloud Service**.
1. Procure por &quot;GraphiQL&quot; (certifique-se de incluir o **i** em **GraphiQL**.
1. Baixe o **Pacote de Conteúdo GraphiQL v.x.x** mais recente

   ![Download do pacote GraphiQL](assets/explore-graphql-api/software-distribution.png)

   O arquivo zip é um pacote AEM que pode ser instalado diretamente.

1. No menu **AEM Iniciar**, navegue até **Ferramentas** > **Implantação** > **Pacotes**.
1. Clique em **Upload Package** e escolha o pacote baixado na etapa anterior. Clique em **Install** para instalar o pacote.

   ![Instalar o pacote GraphiQL](assets/explore-graphql-api/install-graphiql-package.png)

## Consultar uma lista de fragmentos de conteúdo {#query-list-cf}

Um requisito comum será consultar vários Fragmentos de conteúdo.

1. Navegue até o GraphiQL IDE em [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html).
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

1. Pressione o botão **Play** no menu superior para executar a consulta. Você deve ver os resultados dos fragmentos de conteúdo dos Contribuidores do capítulo anterior:

   ![Resultados da lista de colaboradores](assets/explore-graphql-api/contributorlist-results.png)

1. Posicione o cursor abaixo do texto `_path` e insira **CTRL+Space** para acionar a dica de código. Adicione `fullName` e `occupation` à query.

   ![Atualizar consulta com hitação de código](assets/explore-graphql-api/update-query-codehinting.png)

1. Execute a consulta novamente pressionando o botão **Play** e você deverá ver que os resultados incluem as propriedades adicionais de `fullName` e `occupation`.

   ![Nome completo e resultados da atividade profissional](assets/explore-graphql-api/updated-query-fullname-occupation.png)

   `fullName` e  `occupation` são propriedades simples. Lembre-se do capítulo [Definindo Modelos de Fragmento de Conteúdo](./content-fragment-models.md) de que `fullName` e `occupation` são os valores usados ao definir o **Nome da Propriedade** dos respectivos campos.

1. `pictureReference` e  `biographyText` representam campos mais complexos. Atualize a consulta com o seguinte para retornar dados sobre os campos `pictureReference` e `biographyText`.

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

   `biographyText` é um campo de texto de várias linhas e a API GraphQL permite escolher diversos formatos para os resultados, como  `html`,  `markdown`ou  `json`   `plaintext`.

   `pictureReference` é uma referência de conteúdo e deve ser uma imagem, portanto,  `ImageRef` o objeto incorporado é usado. Isso nos permite solicitar dados adicionais sobre a imagem que está sendo referenciada, como `width` e `height`.

1. Em seguida, experimente consultar uma lista de **Aventuras**. Execute a seguinte consulta:

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

   Você deve ver uma lista de **Adventures** retornada. Você pode experimentar adicionando campos adicionais ao query.

## Filtrar uma lista de fragmentos de conteúdo {#filter-list-cf}

Em seguida, vamos examinar como é possível filtrar os resultados para um subconjunto de Fragmentos de conteúdo com base em um valor de propriedade.

1. Insira a seguinte query na interface do usuário GraphiQL:

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

   A consulta acima realiza uma pesquisa em relação a todos os contribuidores no sistema. O filtro adicionado ao início da consulta executará uma comparação no campo `occupation` e na string &quot;**Fotógrafo**&quot;.

1. Execute a query, espera-se que apenas um único **Contributor** seja retornado.
1. Insira a seguinte query para consultar uma lista de **Aventuras** onde `adventureActivity` é **não** igual a **&quot;Surfing&quot;**:

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

1. Execute a consulta e inspecione os resultados. Observe que nenhum dos resultados inclui um `adventureType` igual a **&quot;Navegação&quot;**.

Há muitas outras opções para filtrar e criar consultas complexas, acima estão apenas alguns exemplos.

## Consultar um único fragmento de conteúdo {#query-single-cf}

Também é possível consultar diretamente um único Fragmento de conteúdo. O conteúdo no AEM é armazenado de maneira hierárquica e o identificador exclusivo de um fragmento é baseado no caminho do fragmento. Se o objetivo for retornar dados sobre um único fragmento, é preferível usar o caminho e consultar o modelo diretamente. Usar essa sintaxe significa que a complexidade da consulta será muito baixa e gerará um resultado mais rápido.

1. Insira a seguinte query no editor GraphiQL:

   ```graphql
   {
    contributorByPath(_path: "/content/dam/wknd/en/contributors/stacey-roswells") {
       item {
         _path
         fullName
         biographyText {
           html
         }
       }
     }
   }
   ```

1. Execute a query e observe que o único resultado do fragmento **Stacey Roswells** é retornado.

   No exercício anterior, você usou um filtro para restringir uma lista de resultados. Você pode usar uma sintaxe semelhante para filtrar por caminho, no entanto, a sintaxe acima é preferida por motivos de desempenho.

1. Lembre-se no capítulo [Criação de fragmentos de conteúdo](./author-content-fragments.md) de que uma variação **Resumo** foi criada para **Stacey Roswells**. Atualize a consulta para retornar a variação **Summary**:

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
         biographyText {
           html
         }
       }
     }
   }
   ```

   Mesmo que a variação tenha sido nomeada como **Summary**, as variações são persistentes em minúsculas e, portanto, `summary` é usada.

1. Execute a query e observe que o campo `biography` contém um resultado `html` muito mais curto.

## Consulta para vários modelos de fragmento de conteúdo {#query-multiple-models}

Também é possível combinar consultas separadas em um único query. Isso é útil para minimizar o número de solicitações HTTP necessárias para potencializar o aplicativo. Por exemplo, a visualização *Início* de um aplicativo pode exibir conteúdo com base em **dois** Modelos de Fragmento de Conteúdo diferentes. Em vez de executar **dois** consultas separadas, podemos combinar as consultas em uma única solicitação.

1. Insira a seguinte query no editor GraphiQL:

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

1. Execute a query e observe que o conjunto de resultados contém dados de **Adventures** e **Contributors**:

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

Para obter mais exemplos de consultas GraphQL, consulte: [Saiba como usar GraphQL com AEM - Conteúdo de amostra e Consultas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/content-fragments-graphql-samples.html).

## Parabéns! {#congratulations}

Parabéns, você acabou de criar e executar várias consultas GraphQL!

## Próximas etapas {#next-steps}

No próximo capítulo, [Consultando AEM de um aplicativo React](./graphql-and-external-app.md), você explorará como um aplicativo externo pode consultar AEM pontos de extremidade GraphQL. O aplicativo externo que modifica o aplicativo WKND GraphQL React para adicionar consultas GraphQL de filtragem, permitindo que o usuário do aplicativo filtre aventuras por atividade. Você também será apresentado a algum tratamento básico de erros.
