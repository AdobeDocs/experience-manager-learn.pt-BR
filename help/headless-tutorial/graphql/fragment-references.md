---
title: Modelagem de dados avançada com referências de fragmento - Introdução ao AEM Headless - GraphQL
description: Introdução ao Adobe Experience Manager (AEM) e GraphQL. Saiba como usar o recurso Referência de fragmento para modelagem de dados avançada e criar uma relação entre dois Fragmentos de conteúdo diferentes. Saiba como modificar uma consulta GraphQL para incluir um campo de um modelo referenciado.
sub-product: ativos
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6718
thumbnail: KT-6718.jpg
feature: Content Fragments, GraphQL APIs
topic: Headless, Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '857'
ht-degree: 1%

---


# Modelagem de dados avançada com referências de fragmento

É possível fazer referência a um Fragmento de conteúdo a partir de outro Fragmento de conteúdo. Isso permite que um usuário crie modelos de dados complexos com relações entre Fragmentos.

Neste capítulo, você atualizará o modelo do Adventure para incluir uma referência ao modelo do Contributor usando o campo **Referência do fragmento**. Você também aprenderá a modificar uma consulta GraphQL para incluir campos de um modelo referenciado.

## Pré-requisitos

Este é um tutorial com várias partes e presume-se que as etapas descritas nas partes anteriores foram concluídas.

## Objetivos

Neste capítulo, aprenderemos a:

* Atualize um Modelo de fragmento de conteúdo para usar o campo Referência de fragmento
* Criar uma consulta GraphQL que retorna campos de um modelo referenciado

## Adicionar uma referência de fragmento {#add-fragment-reference}

Atualize o Modelo de fragmento do conteúdo de empreendimento para adicionar uma referência ao modelo do contribuidor.

1. Abra um novo navegador e acesse o AEM.
1. No menu **Início do AEM**, navegue até **Ferramentas** > **Ativos** > **Modelos de Fragmento de Conteúdo** > **Site WKND**.
1. Abra o **Modelo de Fragmento de Conteúdo do Adventure**

   ![Abra o modelo de fragmento de conteúdo de empreendimento](assets/fragment-references/adventure-content-fragment-edit.png)

1. Em **Tipos de dados**, arraste e solte um campo **Referência de fragmento** no painel principal.

   ![Adicionar campo de referência do fragmento](assets/fragment-references/add-fragment-reference-field.png)

1. Atualize as **Propriedades** para este campo com o seguinte:

   * Renderizar como - `fragmentreference`
   * Rótulo de campo - **Colaborador de risco**
   * Nome da Propriedade - `adventureContributor`
   * Tipo de modelo - Selecione o modelo **Contributor**
   * Caminho raiz - `/content/dam/wknd`

   ![Propriedades de referência do fragmento](assets/fragment-references/fragment-reference-properties.png)

   O nome da propriedade `adventureContributor` agora pode ser usado para fazer referência a um Fragmento do conteúdo do colaborador.

1. Salve as alterações no modelo.

## Atribuir um contribuidor a uma empresa

Agora que o modelo de Fragmento de conteúdo de empreendimento foi atualizado, podemos editar um fragmento existente e fazer referência a um Contribuinte. Observe que a edição do modelo de Fragmento de conteúdo *afeta* qualquer Fragmento de conteúdo existente criado a partir dele.

1. Navegue até **Assets** > **Arquivos** > **Site WKND** > **Inglês** > **Aventuras** > **[Campo de Surf Bali](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**.

   ![Pasta Bali Surf Camp](assets/setup/bali-surf-camp-folder.png)

1. Clique no fragmento de conteúdo **Campo de navegação de bali** para abrir o Editor de fragmento de conteúdo.
1. Atualize o campo **Adventure Contributor** e selecione um Contribuidor clicando no ícone de pasta.

   ![Selecione Stacey Roswells como colaborador](assets/fragment-references/stacey-roswell-contributor.png)

   *Selecionar um caminho para um Fragmento do colaborador*

   ![caminho preenchido para colaborador](assets/fragment-references/populated-path.png)

   Observe que somente os fragmentos criados usando o modelo **Contributor** podem ser selecionados.

1. Salve as alterações no fragmento.

1. Repita as etapas acima para atribuir um colaborador a aventuras como [Yosemite Backpackaging](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking) e [Colorado Rock Climbing](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/colorado-rock-climbing/colorado-rock-climbing)

## Consultar fragmento de conteúdo aninhado com GraphiQL

Em seguida, execute uma consulta para uma Aventura e adicione propriedades aninhadas do modelo de contribuidor referenciado. Usaremos a ferramenta GraphiQL para verificar rapidamente a sintaxe do query.

1. Navegue até a ferramenta GraphiQL no AEM: [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html)

1. Insira a seguinte query:

   ```graphql
   {
     adventureByPath(_path:"/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp") {
        item {
          _path
          adventureTitle
          adventureContributor {
            fullName
            occupation
            pictureReference {
           ...on ImageRef {
             _path
           }
         }
       }
     }
    }
   }
   ```

   A consulta acima é para uma única Aventura pelo caminho. A propriedade `adventureContributor` faz referência ao modelo do Contribuidor e, em seguida, podemos solicitar propriedades do Fragmento de conteúdo aninhado.

1. Execute o query e você deverá obter um resultado como o seguinte:

   ```json
   {
     "data": {
       "adventureByPath": {
           "item": {
               "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
               "adventureTitle": "Bali Surf Camp",
               "adventureContributor": {
                   "fullName": "Stacey Roswells",
                   "occupation": "Photographer",
                   "pictureReference": {
                       "_path": "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
                   }
               }
           }
        }
     }
   }
   ```

1. Experimente com outras consultas, como `adventureList` e adicione propriedades para o Fragmento de conteúdo referenciado em `adventureContributor`.

## Atualize o aplicativo React para exibir o conteúdo do Contributor

Em seguida, atualize as consultas usadas pelo Aplicativo de Reação para incluir o novo Contribuidor e exibir informações sobre o Contribuidor como parte da visualização de detalhes da Aventura.

1. Abra o aplicativo WKND GraphQL React no IDE.

1. Abra o arquivo `src/components/AdventureDetail.js`.

   ![Componente de detalhes da empresa IDE](assets/fragment-references/adventure-detail-ide.png)

1. Encontre a função `adventureDetailQuery(_path)`. A função `adventureDetailQuery(..)` simplesmente envolve uma consulta GraphQL de filtragem, que usa a sintaxe `<modelName>ByPath` do AEM para consultar um Fragmento de conteúdo único identificado por seu caminho JCR.

1. Atualize a consulta para incluir informações sobre o Contribuidor referenciado:

   ```javascript
   function adventureDetailQuery(_path) {
       return `{
           adventureByPath (_path: "${_path}") {
           item {
               _path
               adventureTitle
               adventureActivity
               adventureType
               adventurePrice
               adventureTripLength
               adventureGroupSize
               adventureDifficulty
               adventurePrice
               adventurePrimaryImage {
                   ... on ImageRef {
                   _path
                   mimeType
                   width
                   height
                   }
               }
               adventureDescription {
                   html
               }
               adventureItinerary {
                   html
               }
               adventureContributor {
                   fullName
                   occupation
                   pictureReference {
                       ...on ImageRef {
                           _path
                       }
                   }
               }
             }
          }
        }
       `;
   }
   ```

   Com esta atualização, propriedades adicionais sobre `adventureContributor`, `fullName`, `occupation` e `pictureReference` serão incluídas na consulta.

1. Inspecione o componente `Contributor` incorporado no arquivo `AdventureDetail.js` em `function Contributor(...)`. Esse componente renderizará o nome, a ocupação e a imagem do Colaborador, se as propriedades existirem.

   O componente `Contributor` é referenciado no método `AdventureDetail(...)` `return`:

   ```javascript
   function AdventureDetail(props) {
       ...
       return (
           ...
            <h2>Itinerary</h2>
           <hr />
           <div className="adventure-detail-itinerary"
                dangerouslySetInnerHTML={{__html: adventureData.adventureItinerary.html}}></div>
           {/* Contributor component is instaniated and 
               is passed the adventureContributor object from the GraphQL Query results */}
           <Contributer {...adventureData.adventureContributor} />
           ...
       )
   }
   ```

1. Salve as alterações no arquivo.
1. Inicie o aplicativo React, se ainda não estiver em execução:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. Navegue até [http://localhost:3000](http://localhost:3000/) e clique em uma Aventura que tenha um Contribuidor referenciado. Agora, você deve ver as informações do Contribuidor listadas abaixo do **Itinerário**:

   ![Colaborador adicionado no aplicativo](assets/fragment-references/contributor-added-detail.png)

## Parabéns!{#congratulations}

Parabéns! Você atualizou um Modelo de fragmento de conteúdo existente para fazer referência a um Fragmento de conteúdo aninhado usando o campo **Referência do fragmento**. Você também aprendeu a modificar uma consulta GraphQL para incluir campos de um modelo referenciado.

## Próximas etapas {#next-steps}

No próximo capítulo, [Implantação de produção usando um ambiente AEM Publish](./production-deployment.md), saiba mais sobre os serviços AEM Author and Publish e o padrão de implantação recomendado para aplicativos sem periféricos. Você atualizará um aplicativo existente para usar variáveis de ambiente para alterar dinamicamente um ponto de extremidade GraphQL com base no ambiente de destino. Você também aprenderá a configurar corretamente o AEM para o CORS (Cross-Origin resource sharing).
