---
title: Criar um componente climático personalizado | Introdução ao Editor de SPA de AEM e React
description: Saiba como criar um componente meteorológico personalizado para ser usado com o Editor de SPA de AEM. Saiba como desenvolver caixas de diálogo do autor e Modelos do Sling para estender o modelo JSON para preencher um componente personalizado. A API Open Weather e os componentes React Open Weather são usados.
sub-product: sites
feature: Editor de SPA
doc-type: tutorial
topics: development
version: cloud-service
kt: 5878
thumbnail: 5878-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 24d70ebaa6a63cfd4a73f43188f25b375dc702ec
workflow-type: tm+mt
source-wordcount: '1230'
ht-degree: 2%

---


# Criar um componente de tempo personalizado {#custom-component}

Saiba como criar um componente meteorológico personalizado para ser usado com o Editor de SPA de AEM. Saiba como desenvolver caixas de diálogo do autor e Modelos do Sling para estender o modelo JSON para preencher um componente personalizado. São usados o [Open Weather API](https://openweathermap.org) e o [React Open Weather component](https://www.npmjs.com/package/react-open-weather).

## Objetivo

1. Entenda a função dos Modelos do Sling em manipular a API de modelo JSON fornecida pelo AEM.
2. Entenda como criar novas caixas de diálogo do componente AEM.
3. Saiba como criar um Componente de AEM **personalizado** que será compatível com a estrutura do editor de SPA.

## O que você vai criar

Um componente meteorológico simples será criado. Esse componente poderá ser adicionado ao SPA pelos autores de conteúdo. Usando uma caixa de diálogo de AEM, os autores podem definir o local onde o tempo será exibido.  A implementação deste componente ilustra as etapas necessárias para criar um novo componente de AEM que seja compatível com a estrutura AEM SPA Editor.

![Configurar o componente de tempo aberto](assets/custom-component/enter-dialog.png)

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment). Este capítulo é uma continuação do capítulo [Navegação e Roteamento](navigation-routing.md). No entanto, para seguir em frente, tudo o que você precisa é de um projeto AEM habilitado para SPA implantado em uma instância de AEM local.

### Chave da API do tempo aberta

Uma chave de API de [Abrir tempo](https://openweathermap.org/) é necessária para acompanhar o tutorial. [A inscrição é ](https://home.openweathermap.org/users/sign_up) gratuita por uma quantidade limitada de chamadas de API.

## Definir o componente AEM

Um componente AEM é definido como um nó e propriedades. No projeto, esses nós e propriedades são representados como arquivos XML no módulo `ui.apps`. Em seguida, crie o componente AEM no módulo `ui.apps`.

>[!NOTE]
>
> Uma atualização rápida das [noções básicas dos componentes AEM pode ser útil](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. No IDE de sua escolha, abra a pasta `ui.apps` .
2. Navegue até `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components` e crie uma nova pasta chamada `open-weather`.
3. Crie um novo arquivo chamado `.content.xml` abaixo da pasta `open-weather`. Preencha o `open-weather/.content.xml` com o seguinte:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Open Weather"
       componentGroup="WKND SPA React - Content"/>
   ```

   ![Criar definição de componente personalizado](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - identifica que este nó será um componente AEM.

   `jcr:title` é o valor que será exibido para Autores de conteúdo e o  `componentGroup` determina o agrupamento de componentes na interface do usuário de criação.

4. Abaixo da pasta `custom-component`, crie outra pasta chamada `_cq_dialog`.
5. Abaixo da pasta `_cq_dialog` crie um novo arquivo chamado `.content.xml` e preencha com o seguinte:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Open Weather"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/tabs"
                   maximized="{Boolean}true">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Properties"
                           sling:resourceType="granite/ui/components/coral/foundation/container"
                           margin="{Boolean}true">
                           <items jcr:primaryType="nt:unstructured">
                               <columns
                                   jcr:primaryType="nt:unstructured"
                                   sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                   margin="{Boolean}true">
                                   <items jcr:primaryType="nt:unstructured">
                                       <column
                                           jcr:primaryType="nt:unstructured"
                                           sling:resourceType="granite/ui/components/coral/foundation/container">
                                           <items jcr:primaryType="nt:unstructured">
                                               <label
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldDescription="The label to display for the component"
                                                   fieldLabel="Label"
                                                   name="./label"/>
                                               <lat
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/numberfield"
                                                   fieldDescription="The latitude of the location."
                                                   fieldLabel="Latitude"
                                                   step="any"
                                                   name="./lat" />
                                               <lon
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/numberfield"
                                                   fieldDescription="The longitude of the location."
                                                   fieldLabel="Longitude"
                                                   step="any"
                                                   name="./lon"/>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </properties>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   ![Definição de componente personalizado](assets/custom-component/dialog-custom-component-defintion.png)

   O arquivo XML acima gera uma caixa de diálogo muito simples para o `Weather Component`. A parte crítica do arquivo são os nós internos `<label>`, `<lat>` e `<lon>`. Esta caixa de diálogo conterá dois `numberfield`s e um `textfield` que permitirá que um usuário configure o tempo a ser exibido.

   Um Modelo do Sling será criado ao lado de expor o valor das propriedades `label`,`lat` e `long` por meio do modelo JSON.

   >[!NOTE]
   >
   > Você pode visualizar muito mais [exemplos de caixas de diálogo ao visualizar as definições dos Componentes principais](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). Também é possível exibir campos de formulário adicionais, como `select`, `textarea`, `pathfield`, disponíveis abaixo de `/libs/granite/ui/components/coral/foundation/form` em [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   Com um componente de AEM tradicional, um script [HTL](https://docs.adobe.com/content/help/pt-BR/experience-manager-htl/using/overview.html) normalmente é necessário. Como o SPA renderizará o componente, nenhum script HTL é necessário.

## Criar o Modelo do Sling

Os Modelos do Sling são objetos Java &quot;POJO&quot; (Plain Old Java Objects) orientados por anotações que facilitam o mapeamento de dados do JCR para variáveis Java. [O Sling ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html?lang=en#sling-models) Modelstypicamente funciona para encapsular a lógica de negócios complexa do lado do servidor para componentes do AEM.

No contexto do Editor de SPA, os Modelos do Sling expõem o conteúdo de um componente por meio do modelo JSON por meio de um recurso usando o [Exportador de Modelo do Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html).

1. No IDE de sua escolha, abra o módulo `core` em `aem-guides-wknd-spa.react/core`.
1. Crie um arquivo chamado em `OpenWeatherModel.java` em `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. Preencha `OpenWeather.java` com o seguinte:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.export.json.ComponentExporter;
   
   // Sling Models intended to be used with SPA Editor must extend ComponentExporter interface
   public interface OpenWeatherModel extends ComponentExporter {
   
       public String getLabel();
   
       public double getLat();
   
       public double getLon();
   
   }
   ```

   Essa é a interface Java do nosso componente. Para que nosso Modelo do Sling seja compatível com a estrutura do Editor de SPA, ele deve estender a classe `ComponentExporter`.

1. Crie uma pasta chamada `impl` abaixo de `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. Crie um arquivo com o nome `OpenWeatherModelImpl.java` abaixo de `impl` e preencha com o seguinte:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models.impl;
   
   import org.apache.sling.models.annotations.*;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import com.adobe.cq.export.json.ComponentExporter;
   import com.adobe.cq.export.json.ExporterConstants;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import com.adobe.aem.guides.wkndspa.react.core.models.OpenWeatherModel;
   
   // Sling Model annotation
   @Model(
       adaptables = SlingHttpServletRequest.class, 
       adapters = { OpenWeatherModel.class, ComponentExporter.class }, 
       resourceType = OpenWeatherModelImpl.RESOURCE_TYPE, 
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
       )
   @Exporter( //Exporter annotation that serializes the modoel as JSON
       name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
       extensions = ExporterConstants.SLING_MODEL_EXTENSION
       )
   public class OpenWeatherModelImpl implements OpenWeatherModel {
   
       @ValueMapValue
       private String label; //maps variable to jcr property named "label" persisted by Dialog
   
       @ValueMapValue
       private double lat; //maps variable to jcr property named "lat"
   
       @ValueMapValue
       private double lon; //maps variable to jcr property named "lon"
   
       // points to AEM component definition in ui.apps
       static final String RESOURCE_TYPE = "wknd-spa-react/components/open-weather";
   
       // public getter method to expose value of private variable `label`
       // adds additional logic to default the label to "(Default)" if not set.
       @Override
       public String getLabel() {
           return StringUtils.isNotBlank(label) ? label : "(Default)";
       }
   
       // public getter method to expose value of private variable `lat`
       @Override
       public double getLat() {
           return lat;
       }
   
       // public getter method to expose value of private variable `lon`
       @Override
       public double getLon() {
           return lon;
       }
   
       // method required by `ComponentExporter` interface
       // exposes a JSON property named `:type` with a value of `wknd-spa-react/components/open-weather`
       // required to map the JSON export to the SPA component props via the `MapTo`
       @Override
       public String getExportedType() {
           return OpenWeatherModelImpl.RESOURCE_TYPE;
       }
   } 
   ```

   A variável estática `RESOURCE_TYPE` deve apontar para o caminho em `ui.apps` do componente. O `getExportedType()` é usado para mapear as propriedades JSON para o componente SPA via `MapTo`. `@ValueMapValue` é uma anotação que lê a propriedade jcr salva pela caixa de diálogo.

## Atualize o SPA

Em seguida, atualize o código React para incluir o [React Open Weather component](https://www.npmjs.com/package/react-open-weather) e faça com que ele mapeie para o componente AEM criado nas etapas anteriores.

1. Instale o componente React Open Weather como uma dependência **npm**:

   ```shell
   $ cd aem-guides-wknd-spa.react/ui.frontend
   $ npm i react-open-weather
   ```

1. Crie uma nova pasta chamada `OpenWeather` em `ui.frontend/src/components/OpenWeather`.
1. Adicione um arquivo chamado `OpenWeather.js` e preencha com o seguinte:

   ```js
   import React from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   import ReactWeather, { useOpenWeather } from 'react-open-weather';
   
   // Open weather API Key
   // For simplicity it is hard coded in the file, ideally this is extracted in to an environment variable
   const API_KEY = 'YOUR_API_KEY';
   
   // Logic to render placeholder or component
   const OpenWeatherEditConfig = {
   
       emptyLabel: 'Weather',
       isEmpty: function(props) {
           return !props || !props.lat || !props.lon || !props.label;
       }
   };
   
   // Wrapper function that includes react-open-weather component
   function ReactWeatherWrapper(props) {
       const { data, isLoading, errorMessage } = useOpenWeather({
           key: API_KEY,
           lat: props.lat, // passed in from AEM JSON
           lon: props.lon, // passed in from AEM JSON
           lang: 'en',
           unit: 'imperial', // values are (metric, standard, imperial)
       });
   
       return (
           <div className="cmp-open-weather">
               <ReactWeather
                   isLoading={isLoading}
                   errorMessage={errorMessage}
                   data={data}
                   lang="en"
                   locationLabel={props.label} // passed in from AEM JSON
                   unitsLabels={{ temperature: 'F', windSpeed: 'mph' }}
                   showForecast={false}
                 />
           </div>
       );
   }
   
   export default function OpenWeather(props) {
   
           // render nothing if component not configured
           if(OpenWeatherEditConfig.isEmpty(props)) {
               return null;
           }
   
           // render ReactWeather component if component configured
           // pass props to ReactWeatherWrapper. These props include the mapped properties from AEM JSON
           return ReactWeatherWrapper(props);
   
   }
   
   // Map OpenWeather to AEM component
   MapTo('wknd-spa-react/components/open-weather')(OpenWeather, OpenWeatherEditConfig);
   ```

1. Atualize `import-components.js` em `ui.frontend/src/components/import-components.js` para incluir o componente `OpenWeather`:

   ```diff
     // import-component.js
     import './Container/Container';
     import './ExperienceFragment/ExperienceFragment';
   + import './OpenWeather/OpenWeather';
   ```

1. Implante todas as atualizações em um ambiente de AEM local a partir da raiz do diretório do projeto, usando suas habilidades Maven:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Atualizar a Política de Modelo

Em seguida, navegue até AEM para verificar as atualizações e permitir que o componente `OpenWeather` seja adicionado ao SPA.

1. Verifique o registro do novo Modelo do Sling navegando até [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl - wknd-spa-react/components/open-weather
   
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl exports 'wknd-spa-react/components/open-weather' with selector 'model' and extension '[Ljava.lang.String;@2fd80fc5' with exporter 'jackson'
   ```

   Você deve ver as duas linhas acima que indicam que `OpenWeatherModelImpl` está associado ao componente `wknd-spa-react/components/open-weather` e que está registrado por meio do Exportador de Modelo do Sling.

1. Navegue até o Modelo de página de SPA em [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Atualize a política do Contêiner de layout para adicionar o novo `Open Weather` como um componente permitido:

   ![Atualizar política do contêiner de layout](assets/custom-component/custom-component-allowed.png)

   Salve as alterações na política e observe o `Open Weather` como um componente permitido:

   ![Componente personalizado como um componente permitido](assets/custom-component/custom-component-allowed-layout-container.png)

## Crie o componente Tempo aberto

Em seguida, crie o componente `Open Weather` usando o Editor de SPA AEM.

1. Navegue até [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).
1. No modo `Edit`, adicione o `Open Weather` ao `Layout Container`:

   ![Inserir novo componente](assets/custom-component/insert-custom-component.png)

1. Abra a caixa de diálogo do componente e insira um **Label**, **Latitude** e **Longitude**. Por exemplo **San Diego**, **32.7157** e **-117.1611**. Os números do hemisfério ocidental e do hemisfério sul são representados como números negativos com a API Open Weather

   ![Configurar o componente de tempo aberto](assets/custom-component/enter-dialog.png)

   Esta é a caixa de diálogo que foi criada com base no arquivo XML anterior no capítulo.

1. Salve as alterações. Observe que o tempo para **San Diego** agora é exibido:

   ![Componente meteorológico atualizado](assets/custom-component/weather-updated.png)

1. Visualize o modelo JSON navegando até [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Pesquisar `wknd-spa-react/components/open-weather`:

   ```json
   "open_weather": {
       "label": "San Diego",
       "lat": 32.7157,
       "lon": -117.1611,
       ":type": "wknd-spa-react/components/open-weather"
   }
   ```

   Os valores JSON são gerados pelo Modelo do Sling. Esses valores JSON são passados para o componente React como props.

## Parabéns! {#congratulations}

Parabéns, você aprendeu a criar um componente de AEM personalizado para ser usado com o Editor de SPA. Você também aprendeu como caixas de diálogo, propriedades JCR e Modelos do Sling interagem para produzir o modelo JSON.

### Próximas etapas {#next-steps}

[Estender um componente principal](extend-component.md)  - saiba como estender um componente principal de AEM existente para ser usado com o AEM Editor SPA. Entender como adicionar propriedades e conteúdo a um componente existente é uma técnica avançada para expandir os recursos de uma implementação do Editor de SPA AEM.
