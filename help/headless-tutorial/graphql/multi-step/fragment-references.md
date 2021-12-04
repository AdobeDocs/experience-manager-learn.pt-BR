---
title: Modelagem de dados avançada com referências de fragmento - Introdução AEM sem cabeçalho - GraphQL
description: Introdução à Adobe Experience Manager (AEM) e GraphQL. Saiba como usar o recurso Referência de fragmento para modelagem de dados avançada e criar uma relação entre dois Fragmentos de conteúdo diferentes. Saiba como modificar uma consulta GraphQL para incluir um campo de um modelo referenciado.
version: Cloud Service
mini-toc-levels: 1
kt: 6718
thumbnail: KT-6718.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d85b7ac3-42c1-4655-9394-29a797c0e1d7
source-git-commit: 0dae6243f2a30147bed7079ad06144ad35b781d8
workflow-type: tm+mt
source-wordcount: '847'
ht-degree: 1%

---

# Modelagem de dados avançada com referências de fragmento

É possível fazer referência a um Fragmento de conteúdo a partir de outro Fragmento de conteúdo. Isso permite que um usuário crie modelos de dados complexos com relações entre Fragmentos.

Neste capítulo, você atualizará o modelo de Aventura para incluir uma referência ao modelo de Contribuidor usando o **Referência do fragmento** campo. Você também aprenderá a modificar uma consulta GraphQL para incluir campos de um modelo referenciado.

## Pré-requisitos

Este é um tutorial com várias partes e presume-se que as etapas descritas nas partes anteriores foram concluídas.

## Objetivos

Neste capítulo, aprenderemos a:

* Atualize um Modelo de fragmento de conteúdo para usar o campo Referência de fragmento
* Criar uma consulta GraphQL que retorna campos de um modelo referenciado

## Adicionar uma referência de fragmento {#add-fragment-reference}

Atualize o Modelo de fragmento do conteúdo de empreendimento para adicionar uma referência ao modelo do contribuidor.

1. Abra um novo navegador e acesse AEM.
1. No **Início do AEM** navegue até **Ferramentas** > **Ativos** > **Modelos de fragmentos do conteúdo** > **Site WKND**.
1. Abra o **Aventura** Modelo de fragmento de conteúdo

   ![Abra o modelo de fragmento de conteúdo de empreendimento](assets/fragment-references/adventure-content-fragment-edit.png)

1. Em **Tipos de dados**, arraste e solte uma **Referência do fragmento** no painel principal.

   ![Adicionar campo de referência do fragmento](assets/fragment-references/add-fragment-reference-field.png)

1. Atualize o **Propriedades** para este campo com o seguinte:

   * Renderizar como - `fragmentreference`
   * Rótulo do campo - **Colaborador de risco**
   * Nome da Propriedade - `adventureContributor`
   * Tipo de modelo - Selecione o **Colaborador** modelo
   * Caminho raiz - `/content/dam/wknd`

   ![Propriedades de referência do fragmento](assets/fragment-references/fragment-reference-properties.png)

   O nome da propriedade `adventureContributor` agora pode ser usado para fazer referência a um Fragmento do conteúdo do colaborador.

1. Salve as alterações no modelo.

## Atribuir um contribuidor a uma empresa

Agora que o modelo de Fragmento de conteúdo de empreendimento foi atualizado, podemos editar um fragmento existente e fazer referência a um Contribuinte. Observe que a edição do modelo de Fragmento de conteúdo *efeitos* qualquer Fragmento de conteúdo existente criado a partir dele.

1. Navegar para **Ativos** > **Arquivos** > **Site WKND** > **Inglês** > **Aventuras** > **[Campo de Surf de Bali](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**.

   ![Pasta Bali Surf Camp](../quick-setup/assets/setup/bali-surf-camp-folder.png)

1. Clique no botão **Campo de Surf de Bali** fragmento de conteúdo para abrir o Editor de fragmento de conteúdo.
1. Atualize o **Colaborador de risco** e selecione um Colaborador clicando no ícone de pasta.

   ![Selecione Stacey Roswells como colaborador](assets/fragment-references/stacey-roswell-contributor.png)

   *Selecionar um caminho para um Fragmento do colaborador*

   ![caminho preenchido para colaborador](assets/fragment-references/populated-path.png)

   Observe que somente os fragmentos criados usando o **Colaborador** podem ser selecionados.

1. Salve as alterações no fragmento.

1. Repita as etapas acima para atribuir um colaborador a aventuras como [Embalagem de origem Yosemite](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking) e [Colorado Rock Climbing](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/colorado-rock-climbing/colorado-rock-climbing)

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

   A consulta acima é para uma única Aventura pelo caminho. O `adventureContributor` faz referência ao modelo do Contribuidor e, em seguida, podemos solicitar propriedades do Fragmento de conteúdo aninhado.

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

1. Experimente com outras consultas como `adventureList` e adicione as propriedades para o Fragmento do conteúdo referenciado em `adventureContributor`.

## Atualize o aplicativo React para exibir o conteúdo do Contributor

Em seguida, atualize as consultas usadas pelo Aplicativo de Reação para incluir o novo Contribuidor e exibir informações sobre o Contribuidor como parte da visualização de detalhes da Aventura.

1. Abra o aplicativo WKND GraphQL React no IDE.

1. Abra o arquivo `src/components/AdventureDetail.js`.

   ![Componente de detalhes da empresa IDE](assets/fragment-references/adventure-detail-ide.png)

1. Encontre a função `adventureDetailQuery(_path)`. O `adventureDetailQuery(..)` simplesmente envolve uma consulta GraphQL de filtragem, que usa AEM `<modelName>ByPath` para consultar um único Fragmento de conteúdo identificado por seu caminho JCR.

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

   Com esta atualização, outras propriedades sobre o `adventureContributor`, `fullName`, `occupation`e `pictureReference` serão incluídas no query.

1. A Inspect `Contributor` componente incorporado no `AdventureDetail.js` arquivo em `function Contributor(...)`. Esse componente renderizará o nome, a ocupação e a imagem do Colaborador, se as propriedades existirem.

   O `Contributor` é referenciado no `AdventureDetail(...)` `return` método :

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

1. Navegar para [http://localhost:3000](http://localhost:3000/) e clique em uma Aventura que tenha um contribuidor referenciado. Agora, você deve ver as informações do Colaborador listadas abaixo da variável **Itinerário**:

   ![Colaborador adicionado no aplicativo](assets/fragment-references/contributor-added-detail.png)

## Parabéns!{#congratulations}

Parabéns! Você atualizou um Modelo de fragmento de conteúdo existente para fazer referência a um Fragmento de conteúdo aninhado usando o **Referência do fragmento** campo. Você também aprendeu a modificar uma consulta GraphQL para incluir campos de um modelo referenciado.

## Próximas etapas {#next-steps}

No próximo capítulo, [Implantação de produção usando um ambiente de publicação do AEM](./production-deployment.md)Saiba mais sobre os serviços de Autor e Publicação do AEM e o padrão de implantação recomendado para aplicativos sem cabeçalho. Você atualizará um aplicativo existente para usar variáveis de ambiente para alterar dinamicamente um ponto de extremidade GraphQL com base no ambiente de destino. Você também aprenderá a configurar corretamente o AEM para o CORS (Cross-Origin resource sharing).
