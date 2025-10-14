---
title: Personalizar a camada de dados do cliente da Adobe com componentes do AEM
description: Saiba como personalizar a Camada de dados de clientes Adobe com conteúdo de Componentes AEM personalizados. Saiba como usar APIs fornecidas pelos Componentes principais do AEM para estender e personalizar a camada de dados.
version: Experience Manager as a Cloud Service
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate, Experienced
jira: KT-6265
thumbnail: KT-6265.jpg
last-substantial-update: 2022-09-20T00:00:00Z
doc-type: Tutorial
exl-id: 80e4cf2e-dff6-41e8-b09b-187cf2e18e00
duration: 452
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1813'
ht-degree: 1%

---

# Personalizar a camada de dados do cliente da Adobe com componentes do AEM {#customize-data-layer}

Saiba como personalizar a Camada de dados de clientes Adobe com conteúdo de Componentes AEM personalizados. Saiba como usar APIs fornecidas pelos [Componentes principais do AEM para estender](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html?lang=pt-BR) e personalizar a camada de dados.

## O que você vai criar

![Camada de dados de byline](assets/adobe-client-data-layer/byline-data-layer-html.png)

Neste tutorial, vamos explorar várias opções para estender a Camada de dados de clientes Adobe atualizando o [componente de linha de guia](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html?lang=pt-BR) da WKND. O componente _Subtítulo_ é um **componente personalizado** e as lições aprendidas neste tutorial podem ser aplicadas a outros componentes personalizados.

### Objetivos {#objective}

1. Insira dados do componente na camada de dados estendendo um Modelo Sling e o componente HTL
1. Usar utilitários de camada de dados dos Componentes principais para reduzir o esforço
1. Usar atributos de dados do Componente principal para conectar-se aos eventos existentes da camada de dados

## Pré-requisitos {#prerequisites}

É necessário um **ambiente de desenvolvimento local** para concluir este tutorial. Capturas de tela e vídeos são capturados usando o AEM as a Cloud Service SDK em execução em uma macOS. Os comandos e o código são independentes do sistema operacional local, a menos que indicado de outra forma.

**Novo no AEM as a Cloud Service?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local usando o SDK do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=pt-BR).

**Novo no AEM 6.5?** Consulte o [guia a seguir para configurar um ambiente de desenvolvimento local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=pt-BR).

## Baixar e implantar o site de referência WKND {#set-up-wknd-site}

Este tutorial estende o componente Subtítulo no site de referência WKND. Clonar e instalar a base de código WKND em seu ambiente local.

1. Inicie uma instância do AEM Quickstart **author** local em execução em [http://localhost:4502](http://localhost:4502).
1. Abra uma janela de terminal e clone a base de código WKND usando Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Implante a base de código WKND em uma instância local do AEM:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Para o AEM 6.5 e o pacote de serviços mais recente, adicione o perfil `classic` ao comando Maven:
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. Abra uma nova janela do navegador e faça logon no AEM. Abra uma página de **Revista** como: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Componente de Subtítulo na Página](assets/adobe-client-data-layer/byline-component-onpage.png)

   Você deve ver um exemplo do componente Subtítulo que foi adicionado à página como parte de um Fragmento de experiência. Você pode exibir o Fragmento de experiência em [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)
1. Abra as ferramentas do desenvolvedor e digite o seguinte comando no **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Para ver o estado atual da camada de dados em um site do AEM, inspecione a resposta. Você deve ver informações sobre a página e os componentes individuais.

   ![Resposta da Camada de Dados da Adobe](assets/data-layer-state-response.png)

   Observe que o componente Subtítulo não está listado na Camada de dados.

## Atualizar o modelo Sling de byline {#sling-model}

Para inserir dados sobre o componente na camada de dados, primeiro vamos atualizar o Modelo Sling do componente. Em seguida, atualize a interface Java™ do Byline e a implementação do Modelo Sling para ter um novo método `getData()`. Esse método contém as propriedades a serem inseridas na camada de dados.

1. Abra o projeto `aem-guides-wknd` no IDE de sua escolha. Navegue até o módulo `core`.
1. Abra o arquivo `Byline.java` em `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.

   ![Byline Java Interface](assets/adobe-client-data-layer/byline-java-interface.png)

1. Adicionar o método abaixo à interface:

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

1. Abra o arquivo `BylineImpl.java` em `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`. É a implementação da interface `Byline` e é implementada como um Modelo Sling.

1. Adicione as seguintes instruções de importação no início do arquivo:

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   As APIs `fasterxml.jackson` são usadas para serializar os dados a serem expostos como JSON. O `ComponentUtils` dos Componentes principais do AEM são usados para verificar se a camada de dados está habilitada.

1. Adicionar o método não implementado `getData()` a `BylineImple.java`:

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

   No método acima, um novo `HashMap` é usado para capturar as propriedades a serem expostas como JSON. Observe que métodos existentes como `getName()` e `getOccupations()` são usados. O `@type` representa o tipo de recurso exclusivo do componente. Ele permite que um cliente identifique eventos e/ou acionadores facilmente com base no tipo de componente.

   O `ObjectMapper` é usado para serializar as propriedades e retornar uma cadeia de caracteres JSON. Essa cadeia de caracteres JSON pode ser inserida na camada de dados.

1. Abra uma janela de terminal. Crie e implante apenas o módulo `core` usando suas habilidades Maven:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## Atualizar o HTL do byline {#htl}

Em seguida, atualize o `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/specification.html?lang=pt-BR). HTL (HTML Template Language) é o modelo usado para renderizar o HTML do componente.

Um atributo de dados especial `data-cmp-data-layer` em cada componente do AEM é usado para expor sua camada de dados. O JavaScript fornecido pelos Componentes principais do AEM procura esse atributo de dados. O valor desse atributo de dados é preenchido com a Cadeia de Caracteres JSON retornada pelo método `getData()` do Modelo Sling de byline e inserida na camada de Dados do cliente do Adobe.

1. Abra o projeto `aem-guides-wknd` no IDE. Navegue até o módulo `ui.apps`.
1. Abra o arquivo `byline.html` em `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![HTML de Linha &#x200B;](assets/adobe-client-data-layer/byline-html-template.png)

1. Atualize `byline.html` para incluir o atributo `data-cmp-data-layer`:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   O valor de `data-cmp-data-layer` foi definido como `"${byline.data}"`, onde `byline` é o Modelo Sling atualizado anteriormente. `.data` é a notação padrão para chamar um método Getter Java™ no HTL de `getData()` implementado no exercício anterior.

1. Abra uma janela de terminal. Crie e implante apenas o módulo `ui.apps` usando suas habilidades Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Retorne ao navegador e reabra a página com um componente Subtítulo: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Abra as ferramentas do desenvolvedor e inspecione o código-fonte HTML da página para o componente Subtítulo:

   ![Camada de dados de byline](assets/adobe-client-data-layer/byline-data-layer-html.png)

   Você deve ver que `data-cmp-data-layer` foi preenchido com a sequência JSON do modelo Sling.

1. Abra as ferramentas de desenvolvedor do navegador e digite o seguinte comando no **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Navegue abaixo da resposta em `component` para descobrir se a instância do componente `byline` foi adicionada à camada de dados:

   ![Parte da sublinha da camada de dados](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

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

## Adicionar um evento de clique {#click-event}

A Camada de Dados de Clientes Adobe é orientada por eventos e um dos eventos mais comuns para acionar uma ação é o evento `cmp:click`. Os Componentes principais do AEM facilitam o registro do componente com a ajuda do elemento de dados: `data-cmp-clickable`.

Os elementos clicáveis geralmente são um botão CTA ou um link de navegação. Infelizmente, o componente Subtítulo não tem nenhum desses, mas vamos registrá-lo de qualquer maneira, pois isso pode ser comum para outros componentes personalizados.

1. Abra o módulo `ui.apps` no IDE
1. Abra o arquivo `byline.html` em `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. Atualize `byline.html` para incluir o atributo `data-cmp-clickable` no elemento **name** da linha de dados:

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

1. Retorne ao navegador e reabra a página com o componente Subtítulo adicionado: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   Para testar nosso evento, adicionaremos manualmente alguns JavaScript usando o console do desenvolvedor. Consulte [Usando a Camada de Dados de Clientes Adobe com Componentes Principais do AEM](data-layer-overview.md) para ver um vídeo sobre como fazer isso.

1. Abra as ferramentas de desenvolvedor do navegador e insira o seguinte método no **Console**:

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   Este método simples deve lidar com o clique do nome do componente Subtítulo.

1. Digite o seguinte método no **Console**:

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   O método acima envia um ouvinte de eventos para a camada de dados para ouvir o evento `cmp:click` e chama `bylineClickHandler`.

   >[!CAUTION]
   >
   > É importante **não** atualizar o navegador durante todo este exercício, caso contrário, o JavaScript do console será perdido.

1. No navegador, com o **Console** aberto, clique no nome do autor no componente Subtítulo:

   ![Componente de byline clicado](assets/adobe-client-data-layer/byline-component-clicked.png)

   Você deve ver a mensagem de console `Byline Clicked!` e o nome de Subtítulo.

   O evento `cmp:click` é o mais fácil de conectar. Para componentes mais complexos e rastrear outros comportamentos, é possível adicionar um JavaScript personalizado para adicionar e registrar novos eventos. Um ótimo exemplo é o componente Carrossel, que aciona um evento `cmp:show` sempre que um slide é alternado. Consulte o [código-fonte para obter mais detalhes](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js).

## Usar o utilitário DataLayerBuilder {#data-layer-builder}

Quando o Modelo Sling foi [atualizado](#sling-model) anteriormente no capítulo, optamos por criar a Cadeia de caracteres JSON usando um `HashMap` e definindo cada uma das propriedades manualmente. Esse método funciona bem para componentes pequenos e únicos, no entanto, para componentes que estendem os Componentes principais do AEM, isso pode resultar em muito código extra.

Uma classe de utilitário, `DataLayerBuilder`, existe para executar a maior parte do trabalho pesado. Isso permite que as implementações estendam apenas as propriedades que desejam. Vamos atualizar o Modelo do Sling para usar o `DataLayerBuilder`.

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

   `ComponentData` é um objeto fornecido pelos Componentes Principais do AEM. Ele resulta em uma sequência JSON, como no exemplo anterior, mas também executa muito trabalho adicional.

1. Abra o arquivo `BylineImpl.java` em `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. Adicione as seguintes instruções de importação:

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

   O componente Subtítulo reutiliza partes do Componente principal de imagem para exibir uma imagem representando o autor. No trecho acima, o [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) é usado para estender a camada de dados do componente `Image`. Isso preenche o objeto JSON previamente com todos os dados sobre a imagem usada. Também executa algumas funções de rotina, como definir o `@type` e o identificador exclusivo do componente. Observe que o método é pequeno!

   A única propriedade estendeu o `withTitle`, que é substituído pelo valor de `getName()`.

1. Abra uma janela de terminal. Crie e implante apenas o módulo `core` usando suas habilidades Maven:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. Retorne ao IDE e abra o arquivo `byline.html` em `ui.apps`
1. Atualize o HTL para usar `byline.data.json` para popular o atributo `data-cmp-data-layer`:

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

1. Retorne ao navegador e reabra a página com o componente Subtítulo adicionado: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. Abra as ferramentas de desenvolvedor do navegador e digite o seguinte comando no **Console**:

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

   Observe que agora há um objeto `image` dentro da entrada de componente `byline`. Há muito mais informações sobre o ativo no DAM. Observe também que `@type` e a ID exclusiva (neste caso, `byline-136073cfcb`) foram automaticamente preenchidas, e a `repo:modifyDate` que indica quando o componente foi modificado.

## Exemplos adicionais {#additional-examples}

1. Outro exemplo de extensão da camada de dados pode ser visualizado ao inspecionar o componente `ImageList` na base de código WKND:
   * `ImageList.java` - Interface Java no módulo `core`.
   * `ImageListImpl.java` - Modelo Sling no módulo `core`.
   * `image-list.html` - Modelo HTL no módulo `ui.apps`.

   >[!NOTE]
   >
   > É um pouco mais difícil incluir propriedades personalizadas como `occupation` ao usar o [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html). No entanto, se estender um Componente principal que inclua uma Imagem ou Página, o utilitário economiza muito tempo.

   >[!NOTE]
   >
   > Se estiver criando uma Camada de dados avançada para objetos reutilizados em uma implementação, é recomendável extrair os elementos da Camada de dados em seus próprios objetos Java™ específicos da camada de dados. Por exemplo, os Componentes Principais do Commerce adicionaram interfaces para `ProductData` e `CategoryData` já que eles podem ser usados em muitos componentes dentro de uma implementação do Commerce. Revise [o código no repositório aem-CIF-core-components](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer) para obter mais detalhes.

## Parabéns. {#congratulations}

Você acabou de explorar algumas maneiras de estender e personalizar a Camada de dados do cliente da Adobe com componentes do AEM!

## Recursos adicionais {#additional-resources}

* [Documentação da Camada de Dados de Clientes Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Integração da Camada de Dados com os Componentes Principais](https://github.com/adobe/aem-core-wcm-components/blob/main/DATA_LAYER_INTEGRATION.md)
* [Usando a Camada de Dados de Clientes Adobe e a Documentação de Componentes Principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=pt-BR)
