---
title: Personalizar a camada de dados do cliente Adobe com componentes AEM
description: Saiba como personalizar a Camada de dados do cliente Adobe com conteúdo de componentes AEM personalizados. Saiba como usar as APIs fornecidas por AEM Componentes principais para estender e personalizar a camada de dados.
feature: core-component
topics: integrations
audience: developer
doc-type: tutorial
activity: use
version: cloud-service
kt: 6265
thumbnail: KT-6265.jpg
translation-type: tm+mt
source-git-commit: 46936876de355de9923f7a755aa6915a13cca354
workflow-type: tm+mt
source-wordcount: '2027'
ht-degree: 3%

---


# Personalizar a camada de dados do cliente Adobe com AEM componentes {#customize-data-layer}

Saiba como personalizar a Camada de dados do cliente Adobe com conteúdo de componentes AEM personalizados. Saiba como usar as APIs fornecidas por [AEM Componentes principais para estender](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html) e personalizar a camada de dados.

## O que você vai criar

![Camada de dados de byline](assets/adobe-client-data-layer/byline-data-layer-html.png)

Neste tutorial, você explorará várias opções para estender a Camada de dados do cliente Adobe, atualizando o componente WKND [Byline](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html). Este é um componente personalizado e as lições aprendidas neste tutorial podem ser aplicadas a outros componentes personalizados.

### Objetivos {#objective}

1. Injete os dados do componente na camada de dados estendendo um Modelo Sling e um componente HTL
1. Usar utilitários de camada de dados do Componente Principal para reduzir o esforço
1. Usar atributos de dados do Componente Principal para conectar-se a eventos de camada de dados existentes

## Pré-requisitos {#prerequisites}

Um **ambiente de desenvolvimento local** é necessário para concluir este tutorial. Capturas de tela e vídeo são capturados usando o AEM como um SDK Cloud Service em execução em um macOS. Os comandos e códigos são independentes do sistema operacional local, a menos que seja observado o contrário.

**Novo no AEM as a Cloud Service?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).

**Novo no AEM 6.5?** Consulte o guia a  [seguir para configurar um ambiente](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) de desenvolvimento local.

## Baixe e implante o site de referência WKND {#set-up-wknd-site}

Este tutorial estende o componente Linha de identificação no site de referência WKND. Clonar e instalar a base de código WKND no seu ambiente local.

1. Start uma instância local do Quickstart **author** de AEM em execução em [http://localhost:4502](http://localhost:4502).
1. Abra uma janela de terminal e clone a base de código WKND usando Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Implante a base de código WKND para uma instância local do AEM:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se estiver usando o AEM 6.5 e o service pack mais recente, adicione o perfil `classic` ao comando Maven:
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. Abra uma nova janela do navegador e faça login no AEM. Abra uma página **Revista** como: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Componente de byline na página](assets/adobe-client-data-layer/byline-component-onpage.png)

   Você deve ver um exemplo do componente Linha de identificação que foi adicionado à página como parte de um Fragmento de experiência. Você pode visualização o Fragmento de experiência em [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)
1. Abra as ferramentas do desenvolvedor e digite o seguinte comando no **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect a resposta para ver o estado atual da camada de dados em um site AEM. Você deve ver informações sobre a página e os componentes individuais.

   ![Resposta da camada de dados de Adobe](assets/data-layer-state-response.png)

   Observe que o componente Linha de identificação não está listado na Camada de dados.

## Atualize o Modelo de divisão em linha de bytes {#sling-model}

Para injetar dados sobre o componente na camada de dados, primeiro é necessário atualizar o Modelo Sling do componente. Em seguida, atualize a interface Java do Byline e a implementação do Modelo Sling para adicionar um novo método `getData()`. Este método conterá as propriedades que queremos injetar na camada de dados.

1. No IDE de sua escolha, abra o projeto `aem-guides-wknd`. Navegue até o módulo `core`.
1. Abra o arquivo `Byline.java` em `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.

   ![Interface Java de byline](assets/adobe-client-data-layer/byline-java-interface.png)

1. Adicione um novo método à interface:

   ```java
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return String
        */
       String getData();
   }
   ```

1. Abra o arquivo `BylineImpl.java` em `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

   Esta é a implementação da interface `Byline` e é implementada como um Modelo Sling.

1. Adicione as seguintes declarações de importação ao início do arquivo:

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   As APIs `fasterxml.jackson` serão usadas para serializar os dados que queremos expor como JSON. O `ComponentUtils` dos Componentes principais AEM será usado para verificar se a camada de dados está ativada.

1. Adicione o método não implementado `getData()` a `BylineImple.java`:

   ```java
   public class BylineImpl implements Byline {
       ...
       @Override
       public String getData() {
           Resource bylineResource = this.request.getResource();
           // Use ComponentUtils to verify if the DataLayer is enabled
           if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
               //Create a map of properties we want to expose
               Map<String, Object> bylineProperties = new HashMap<String,Object>();
               bylineProperties.put("@type", bylineResource.getResourceType());
               bylineProperties.put("name", this.getName());
               bylineProperties.put("occupation", this.getOccupations());
               bylineProperties.put("fileReference", image.getFileReference());
   
               //Use AEM Core Component utils to get a unique identifier for the Byline component (in case multiple are on the page)
               String bylineComponentID = ComponentUtils.getId(bylineResource, this.currentPage, this.componentContext);
   
               // Return the bylineProperties as a JSON String with a key of the bylineResource's ID
               try {
                   return String.format("{\"%s\":%s}",
                       bylineComponentID,
                       // Use the ObjectMapper to serialize the bylineProperties to a JSON string
                       new ObjectMapper().writeValueAsString(bylineProperties));
               } catch (JsonProcessingException e) {
   
                   LOGGER.error("Unable to generate dataLayer JSON string", e);
               }
   
           }
           // return null if the Data Layer is not enabled
           return null;
       }
   }
   ```

   No método acima, um novo `HashMap` é usado para capturar as propriedades que queremos expor como JSON. Observe que métodos existentes, como `getName()` e `getOccupations()`, são usados. `@type` representa o tipo de recurso exclusivo do componente, permitindo que um cliente identifique facilmente eventos e/ou acionadores com base no tipo de componente.

   O `ObjectMapper` é usado para serializar as propriedades e retornar uma string JSON. Essa string JSON pode ser inserida na camada de dados.

1. Abra uma janela de terminal. Crie e implante apenas o módulo `core` usando suas habilidades Maven:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## Atualize o Byline HTL {#htl}

Em seguida, atualize `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl). HTL (Linguagem de modelo HTML) é o modelo usado para renderizar o HTML do componente.

Um atributo de dados especial `data-cmp-data-layer` em cada Componente AEM é usado para expor sua camada de dados.  O JavaScript fornecido pelos Componentes Principais AEM procura por esse atributo de dados, cujo valor será preenchido com a String JSON retornada pelo método `getData()` do Modelo de Sling Byline, e injeta os valores na camada de Dados do Cliente Adobe.

1. No IDE, abra o projeto `aem-guides-wknd`. Navegue até o módulo `ui.apps`.
1. Abra o arquivo `byline.html` em `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![Byline HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. Atualize `byline.html` para incluir o atributo `data-cmp-data-layer`:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   O valor de `data-cmp-data-layer` foi definido como `"${byline.data}"`, onde `byline` é o Modelo Sling atualizado anteriormente. `.data` é a notação padrão para chamar um método Java Getter em HTL de  `getData()` implementado no exercício anterior.

1. Abra uma janela de terminal. Crie e implante apenas o módulo `ui.apps` usando suas habilidades Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Retorne ao navegador e abra novamente a página com um componente de Linha de identificação: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Abra as ferramentas do desenvolvedor e inspecione a fonte HTML da página em busca do componente Linha de identificação:

   ![Camada de dados de byline](assets/adobe-client-data-layer/byline-data-layer-html.png)

   Você deve ver que `data-cmp-data-layer` foi preenchido com a string JSON do Modelo Sling.

1. Abra as ferramentas do desenvolvedor do navegador e digite o seguinte comando no **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Navegue abaixo da resposta em `component` para localizar a instância do componente `byline` foi adicionada à camada de dados:

   ![Ignorar parte da camada de dados](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   Você deve ver uma entrada como a seguinte:

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   Observe que as propriedades expostas são as mesmas adicionadas em `HashMap` no Modelo Sling.

## Adicionar um Evento Click {#click-event}

A Camada de dados do cliente Adobe é acionada por evento e um dos eventos mais comuns para acionar uma ação é o evento `cmp:click`. Os componentes principais do AEM facilitam o registro do componente com a ajuda do elemento de dados: `data-cmp-clickable`.

Os elementos clicáveis geralmente são um botão CTA ou um link de navegação. Infelizmente, o componente Byline não tem nenhum desses componentes, mas nós o registraremos de qualquer maneira, pois isso pode ser comum para outros componentes personalizados.

1. Abra o módulo `ui.apps` em seu IDE
1. Abra o arquivo `byline.html` em `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. Atualize `byline.html` para incluir o atributo `data-cmp-clickable` no elemento **name** da Linha de identificação:

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. Abra um novo terminal. Crie e implante apenas o módulo `ui.apps` usando suas habilidades Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Retorne ao navegador e abra novamente a página com o componente Linha de identificação adicionado: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   Para testar nosso evento, adicionaremos manualmente algum JavaScript usando o console do desenvolvedor. Consulte [Usando a camada de dados do cliente Adobe com AEM componentes principais](data-layer-overview.md) para obter um vídeo sobre como fazer isso.

1. Abra as ferramentas do desenvolvedor do navegador e insira o seguinte método no **Console**:

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   Esse método simples deve manipular o clique do nome do componente Linha de identificação.

1. Digite o seguinte método no **Console**:

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   O método acima coloca um ouvinte de evento na camada de dados para ouvir o evento `cmp:click` e chama o `bylineClickHandler`.

   >[!CAUTION]
   >
   > Será importante **e não** atualizar o navegador durante todo este exercício, caso contrário o JavaScript do console será perdido.

1. No navegador, com a **Console** aberta, clique no nome do autor no componente Linha de identificação:

   ![Componente de byline clicado](assets/adobe-client-data-layer/byline-component-clicked.png)

   Você deve ver a mensagem do console `Byline Clicked!` e o nome da Linha de identificação.

   O evento `cmp:click` é o mais fácil de se conectar. Para componentes mais complexos e para rastrear outros comportamentos, é possível adicionar javascript personalizado para adicionar e registrar novos eventos. Um ótimo exemplo é o componente carrossel, que aciona um evento `cmp:show` sempre que um slide é alternado. Consulte o código fonte [para obter mais detalhes](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219).

## Usar o utilitário DataLayerBuilder {#data-layer-builder}

Quando o Modelo Sling era [atualizado](#sling-model) no início do capítulo, optamos por criar a String JSON usando `HashMap` e definindo cada uma das propriedades manualmente. Esse método funciona bem para pequenos componentes únicos, no entanto, para componentes que estendem os Componentes principais AEM isso pode resultar em um monte de código extra.

Existe uma classe de utilitário, `DataLayerBuilder`, para executar a maioria do trabalho pesado. Isso permite que as implementações estendam apenas as propriedades desejadas. Vamos atualizar o Modelo Sling para usar o `DataLayerBuilder`.

1. Retorne ao IDE e navegue até o módulo `core`.
1. Abra o arquivo `Byline.java` em `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.
1. Modifique o método `getData()` para retornar um tipo de `ComponentData`

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   ...
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return ComponentData
        */
       ComponentData getData();
   }
   ```

   `ComponentData` é um objeto fornecido pelos Componentes principais AEM. Isso resulta em uma string JSON, assim como no exemplo anterior, mas também executa muito trabalho adicional.

1. Abra o arquivo `BylineImpl.java` em `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. Adicione as seguintes declarações de importação:

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. Substitua o método `getData()` pelo seguinte:

   ```java
   @Override
   public ComponentData getData() {
       Resource bylineResource = this.request.getResource();
       // Use ComponentUtils to verify if the DataLayer is enabled
       if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
           return DataLayerBuilder.extending(getImage().getData()).asImageComponent()
               .withTitle(this::getName)
               .build();
   
       }
       // return null if the Data Layer is not enabled
       return null;
   }
   ```

   O componente Linha de identificação reutiliza partes do Componente principal da imagem para exibir uma imagem representando o autor. No trecho acima, [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) é usado para estender a camada de dados do componente `Image`. Isso pré-preenche o objeto JSON com todos os dados sobre a imagem usada. Ela também executa algumas funções de rotina, como definir `@type` e o identificador exclusivo do componente. Notem que o método é realmente pequeno!

   A única propriedade estendida `withTitle` que é substituída pelo valor de `getName()`.

1. Abra uma janela de terminal. Crie e implante apenas o módulo `core` usando suas habilidades Maven:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. Retorne ao IDE e abra o arquivo `byline.html` em `ui.apps`
1. Atualize o HTL para usar `byline.data.json` para preencher o atributo `data-cmp-data-layer`:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   Lembre-se de que agora estamos retornando um objeto do tipo `ComponentData`. Este objeto inclui um método getter `getJson()` e é usado para preencher o atributo `data-cmp-data-layer`.

1. Abra uma janela de terminal. Crie e implante apenas o módulo `ui.apps` usando suas habilidades Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Retorne ao navegador e abra novamente a página com o componente Linha de identificação adicionado: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. Abra as ferramentas do desenvolvedor do navegador e digite o seguinte comando no **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Navegue abaixo da resposta em `component` para localizar a instância do componente `byline`:

   ![Camada de dados de byline atualizada](assets/adobe-client-data-layer/byline-data-layer-builder.png)

   Você deve ver uma entrada como a seguinte:

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       dc:title: "Stacey Roswells"
       image:
           @type: "image/jpeg"
           repo:id: "142658f8-4029-4299-9cd6-51afd52345c0"
           repo:modifyDate: "2019-10-25T23:49:51Z"
           repo:path: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
           xdm:tags: []
       parentId: "page-30d989b3f8"
       repo:modifyDate: "2019-10-18T20:17:24Z"
   ```

   Observe que agora há um objeto `image` na entrada do componente `byline`. Isso tem muito mais informações sobre o ativo no DAM. Observe também que `@type` e a id exclusiva (neste caso, `byline-136073cfcb`) foram preenchidas automaticamente, bem como o `repo:modifyDate` que indica quando o componente foi modificado.

## Exemplos adicionais {#additional-examples}

1. Outro exemplo de extensão da camada de dados pode ser visualizado inspecionando o componente `ImageList` na base de código WKND:
   * `ImageList.java` - interface Java no  `core` módulo.
   * `ImageListImpl.java` - Modelo Sling no  `core` módulo.
   * `image-list.html` - Modelo HTL no  `ui.apps` módulo.

   >[!NOTE]
   >
   > É um pouco mais difícil incluir propriedades personalizadas como `occupation` ao usar o [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html). Entretanto, se você estender um componente principal que inclui uma imagem ou página, o utilitário economiza muito tempo.

   >[!NOTE]
   >
   > Se estiver criando uma camada de dados avançada para objetos reutilizados em uma implementação, é recomendável extrair os elementos da camada de dados em seus próprios objetos Java específicos da camada de dados. Por exemplo, os Componentes principais de comércio adicionaram interfaces para `ProductData` e `CategoryData`, já que elas podem ser usadas em muitos componentes em uma implementação de Comércio. Consulte [o código no aem-CIF-core-components repo](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer) para obter mais detalhes.

## Parabéns! {#congratulations}

Você acabou de explorar algumas maneiras de estender e personalizar a Camada de dados do cliente Adobe com componentes AEM!

## Recursos adicionais {#additional-resources}

* [Documentação da camada de dados do cliente Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Integração da camada de dados com os componentes principais](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [Uso da camada de dados do cliente Adobe e da documentação dos componentes principais](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/developing/data-layer/overview.html)
