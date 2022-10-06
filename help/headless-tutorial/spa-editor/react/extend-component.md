---
title: Estender um componente principal | Introdução ao Editor de SPA de AEM e React
description: Saiba como estender o Modelo JSON para um Componente principal existente a ser usado com o Editor de SPA AEM. Entender como adicionar propriedades e conteúdo a um componente existente é uma técnica avançada para expandir os recursos de uma implementação do Editor de SPA AEM. Saiba como usar o padrão de delegação para estender Modelos do Sling e recursos do Sling Resource Merger.
sub-product: sites
feature: SPA Editor, Core Components
doc-type: tutorial
version: Cloud Service
kt: 5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 44433595-08bc-4a82-9232-49d46c31b07b
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 2%

---

# Estender um componente principal {#extend-component}

Saiba como estender um Componente principal existente a ser usado com o Editor de SPA de AEM. Entender como estender um componente existente é uma técnica poderosa para personalizar e expandir os recursos de uma implementação AEM SPA Editor.

## Objetivo

1. Estenda um Componente principal existente com propriedades e conteúdo adicionais.
2. Entenda os fundamentos da Herança de componentes com o uso de `sling:resourceSuperType`.
3. Saiba como aproveitar o [Padrão de delegação](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) para Modelos do Sling para reutilizar a lógica e funcionalidade existentes.

## O que você vai criar

Este capítulo ilustra o código adicional necessário para adicionar uma propriedade extra a um padrão `Image` para cumprir os requisitos de um novo `Banner` componente. O `Banner` o componente contém todas as mesmas propriedades que o padrão `Image` , mas inclui uma propriedade adicional para os usuários preencherem a variável **Texto do banner**.

![Componente de banner de criação final](assets/extend-component/final-author-banner-component.png)

## Pré-requisitos

Revise as ferramentas necessárias e as instruções para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment). Pressupõe-se que neste ponto os usuários do tutorial tenham uma sólida compreensão do recurso Editor de SPA AEM.

## Herança com o Supertipo de Recurso Sling {#sling-resource-super-type}

Para estender um componente existente, defina uma propriedade chamada `sling:resourceSuperType` na definição do seu componente.  `sling:resourceSuperType`é um [propriedade](https://sling.apache.org/documentation/the-sling-engine/resources.html#resource-properties) que pode ser definido na definição de um componente AEM que aponta para outro componente. Isso define explicitamente o componente para herdar toda a funcionalidade do componente identificado como o `sling:resourceSuperType`.

Se queremos estender o `Image` componente em `wknd-spa-react/components/image` precisamos atualizar o código no `ui.apps` módulo.

1. Crie uma nova pasta abaixo do `ui.apps` módulo para `banner` at `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/banner`.
1. Beneath `banner` crie uma definição de Componente (`.content.xml`) como o seguinte:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Banner"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   Isso define `wknd-spa-react/components/banner` para herdar toda a funcionalidade de `wknd-spa-react/components/image`.

## cq:editConfig {#cq-edit-config}

O `_cq_editConfig.xml` O arquivo determina o comportamento de arrastar e soltar na interface do usuário de criação do AEM. Ao estender o componente de Imagem , é importante que o tipo de recurso corresponda ao próprio componente.

1. No `ui.apps` módulo criar outro arquivo abaixo `banner` nomeado `_cq_editConfig.xml`.
1. Preencher `_cq_editConfig.xml` com o seguinte XML:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="cq:EditConfig">
       <cq:dropTargets jcr:primaryType="nt:unstructured">
           <image
               jcr:primaryType="cq:DropTargetConfig"
               accept="[image/gif,image/jpeg,image/png,image/webp,image/tiff,image/svg\\+xml]"
               groups="[media]"
               propertyName="./fileReference">
               <parameters
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wknd-spa-react/components/banner"
                   imageCrop=""
                   imageMap=""
                   imageRotate=""/>
           </image>
       </cq:dropTargets>
       <cq:inplaceEditing
           jcr:primaryType="cq:InplaceEditingConfig"
           active="{Boolean}true"
           editorType="image">
           <inplaceEditingConfig jcr:primaryType="nt:unstructured">
               <plugins jcr:primaryType="nt:unstructured">
                   <crop
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*">
                       <aspectRatios jcr:primaryType="nt:unstructured">
                           <wideLandscape
                               jcr:primaryType="nt:unstructured"
                               name="Wide Landscape"
                               ratio="0.6180"/>
                           <landscape
                               jcr:primaryType="nt:unstructured"
                               name="Landscape"
                               ratio="0.8284"/>
                           <square
                               jcr:primaryType="nt:unstructured"
                               name="Square"
                               ratio="1"/>
                           <portrait
                               jcr:primaryType="nt:unstructured"
                               name="Portrait"
                               ratio="1.6180"/>
                       </aspectRatios>
                   </crop>
                   <flip
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="-"/>
                   <map
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff,image/svg+xml]"
                       features="*"/>
                   <rotate
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*"/>
                   <zoom
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*"/>
               </plugins>
               <ui jcr:primaryType="nt:unstructured">
                   <inline
                       jcr:primaryType="nt:unstructured"
                       toolbar="[crop#launch,rotate#right,history#undo,history#redo,fullscreen#fullscreen,control#close,control#finish]">
                       <replacementToolbars
                           jcr:primaryType="nt:unstructured"
                           crop="[crop#identifier,crop#unlaunch,crop#confirm]"/>
                   </inline>
                   <fullscreen jcr:primaryType="nt:unstructured">
                       <toolbar
                           jcr:primaryType="nt:unstructured"
                           left="[crop#launchwithratio,rotate#right,flip#horizontal,flip#vertical,zoom#reset100,zoom#popupslider]"
                           right="[history#undo,history#redo,fullscreen#fullscreenexit]"/>
                       <replacementToolbars jcr:primaryType="nt:unstructured">
                           <crop
                               jcr:primaryType="nt:unstructured"
                               left="[crop#identifier]"
                               right="[crop#unlaunch,crop#confirm]"/>
                           <map
                               jcr:primaryType="nt:unstructured"
                               left="[map#rectangle,map#circle,map#polygon]"
                               right="[map#unlaunch,map#confirm]"/>
                       </replacementToolbars>
                   </fullscreen>
               </ui>
           </inplaceEditingConfig>
       </cq:inplaceEditing>
   </jcr:root>
   ```

1. O aspecto exclusivo do arquivo é o `<parameters>` nó que define resourceType como `wknd-spa-react/components/banner`.

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/banner"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   A maioria dos componentes não requer um `_cq_editConfig`. Os componentes da imagem e os descendentes são a exceção.

## Estender a caixa de diálogo {#extend-dialog}

Nosso `Banner` O componente requer um campo de texto extra na caixa de diálogo para capturar a variável `bannerText`. Como estamos usando a herança Sling, podemos usar os recursos do [Fusão de Recursos Sling](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html?lang=pt-BR) para substituir ou estender partes da caixa de diálogo. Neste exemplo, uma nova guia foi adicionada à caixa de diálogo para capturar dados adicionais de um autor para preencher o Componente de cartão.

1. No `ui.apps` , abaixo do `banner` criar uma pasta chamada `_cq_dialog`.
1. Beneath `_cq_dialog` criar um arquivo de definição de caixa de diálogo `.content.xml`. Preencha-o com o seguinte:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Banner"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <text
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Text"
                           sling:orderBefore="asset"
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
                                               <textGroup
                                                   granite:hide="${cqDesign.titleHidden}"
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/well">
                                                   <items jcr:primaryType="nt:unstructured">
                                                       <bannerText
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           fieldDescription="Text to display on top of the banner."
                                                           fieldLabel="Banner Text"
                                                           name="./bannerText"/>
                                                   </items>
                                               </textGroup>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </text>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   A definição de XML acima criará uma nova guia chamada **Texto** e solicite *before* as **Ativo** guia . Ele conterá um único campo **Texto do banner**.

1. A caixa de diálogo terá a seguinte aparência:

   ![Caixa de diálogo final do banner](assets/extend-component/banner-dialog.png)

   Observe que não precisávamos definir as guias para **Ativo** ou **Metadados**. Eles são herdados por meio do `sling:resourceSuperType` propriedade.

   Antes de pré-visualizar o diálogo, precisamos implementar o Componente de SPA e o `MapTo` .

## Implementar SPA componente {#implement-spa-component}

Para usar o componente Banner com o Editor de SPA, um novo componente de SPA que será mapeado para `wknd-spa-react/components/banner`. Isso é feito no `ui.frontend` módulo.

1. No `ui.frontend` módulo crie uma nova pasta para `Banner` at `ui.frontend/src/components/Banner`.
1. Crie um novo arquivo com o nome `Banner.js` abaixo do `Banner` pasta. Preencha-o com o seguinte:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   export const BannerEditConfig = {
   
       emptyLabel: 'Banner',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   
   export default class Banner extends Component {
   
       get content() {
           return <img     
                   className="Image-src"
                   src={this.props.src}
                   alt={this.props.alt}
                   title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       // display our custom bannerText property!
       get bannerText() {
           if(this.props.bannerText) {
               return <h4>{this.props.bannerText}</h4>;
           }
   
           return null;
       }
   
       render() {
           if(BannerEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
               <div className="Banner">
                   {this.bannerText}
                   <div className="BannerImage">{this.content}</div>
               </div>
           );
       }
   }
   
   MapTo('wknd-spa-react/components/banner')(Banner, BannerEditConfig);
   ```

   Esse componente SPA mapeia para o componente AEM `wknd-spa-react/components/banner` criado anteriormente.

1. Atualizar `import-components.js` at `ui.frontend/src/components/import-components.js` para incluir o novo `Banner` SPA componente:

   ```diff
     import './ExperienceFragment/ExperienceFragment';
     import './OpenWeather/OpenWeather';
   + import './Banner/Banner';
   ```

1. Nesse momento, o projeto pode ser implantado em AEM e a caixa de diálogo pode ser testada. Implante o projeto usando suas habilidades Maven:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Atualize a política do Modelo de SPA para adicionar o `Banner` componente como um **componente permitido**.

1. Navegue até uma página de SPA e adicione o `Banner` para uma das páginas SPA:

   ![Adicionar componente Banner](assets/extend-component/add-banner-component.png)

   >[!NOTE]
   >
   > A caixa de diálogo permitirá salvar um valor para **Texto do banner** mas esse valor não é refletido no componente SPA. Para habilitar, precisamos estender o Modelo do Sling para o componente.

## Adicionar interface Java {#java-interface}

Para expor os valores da caixa de diálogo do componente ao componente React , precisamos atualizar o Modelo do Sling que preenche o JSON para a variável `Banner` componente. Isso é feito no `core` que contém todo o código Java do projeto do SPA.

Primeiro, criaremos uma nova interface Java para `Banner` que estende a `Image` Interface Java.

1. No `core` módulo criar um novo arquivo chamado `BannerModel.java` at `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. Preencher `BannerModel.java` com o seguinte:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.osgi.annotation.versioning.ProviderType;
   
   @ProviderType
   public interface BannerModel extends Image {
   
       public String getBannerText();
   
   }
   ```

   Isso herdará todos os métodos do Componente principal `Image` e adicione um novo método `getBannerText()`.

## Implementar o modelo Sling {#sling-model}

Em seguida, implemente o Modelo do Sling para o `BannerModel` interface.

1. No `core` módulo criar um novo arquivo chamado `BannerModelImpl.java` at `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models/impl`.

1. Preencher `BannerModelImpl.java` com o seguinte:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models.impl;
   
   import com.adobe.aem.guides.wkndspa.react.core.models.BannerModel;
   import com.adobe.cq.export.json.ComponentExporter;
   import com.adobe.cq.export.json.ExporterConstants;
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.apache.sling.models.annotations.*;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.via.ResourceSuperType;
   
   @Model(
       adaptables = SlingHttpServletRequest.class, 
       adapters = { BannerModel.class,ComponentExporter.class}, 
       resourceType = BannerModelImpl.RESOURCE_TYPE, 
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
       )
   @Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)
   public class BannerModelImpl implements BannerModel {
   
       // points to the the component resource path in ui.apps
       static final String RESOURCE_TYPE = "wknd-spa-react/components/banner";
   
       @Self
       private SlingHttpServletRequest request;
   
       // With sling inheritance (sling:resourceSuperType) we can adapt the current resource to the Image class
       // this allows us to re-use all of the functionality of the Image class, without having to implement it ourself
       // see https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models
       @Self
       @Via(type = ResourceSuperType.class)
       private Image image;
   
       // map the property saved by the dialog to a variable named `bannerText`
       @ValueMapValue
       private String bannerText;
   
       // public getter to expose the value of `bannerText` via the Sling Model and JSON output
       @Override
       public String getBannerText() {
           return bannerText;
       }
   
       // Re-use the Image class for all other methods:
   
       @Override
       public String getSrc() {
           return null != image ? image.getSrc() : null;
       }
   
       @Override
       public String getAlt() {
           return null != image ? image.getAlt() : null;
       }
   
       @Override
       public String getTitle() {
           return null != image ? image.getTitle() : null;
       }
   
   
       // method required by `ComponentExporter` interface
       // exposes a JSON property named `:type` with a value of `wknd-spa-react/components/banner`
       // required to map the JSON export to the SPA component props via the `MapTo`
       @Override
       public String getExportedType() {
           return BannerModelImpl.RESOURCE_TYPE;
       }
   
   }
   ```

   Observe a utilização do `@Model` e `@Exporter` Anotações para garantir que o Modelo do Sling possa ser serializado como JSON por meio do Exportador de Modelo do Sling.

   `BannerModelImpl.java` usa a variável [Padrão de delegação para modelos Sling](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) para evitar a regravação de toda a lógica do componente principal Imagem .

1. Observe as seguintes linhas:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   A anotação acima instanciará um objeto de Imagem chamado `image` com base no `sling:resourceSuperType` herança da `Banner` componente.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   Assim, é possível simplesmente usar a variável `image` objeto para implementar métodos definidos pela variável `Image` interface, sem ter que escrever a lógica. Essa técnica é usada para `getSrc()`, `getAlt()` e `getTitle()`.

1. Abra uma janela de terminal e implante apenas as atualizações na `core` módulo que usa o Maven `autoInstallBundle` do `core` diretório.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

## Tudo junto na prática {#put-together}

1. Retorne para AEM e abra a página SPA que tem a variável `Banner` componente.
1. Atualize o `Banner` componente para incluir **Texto do banner**:

   ![Texto do banner](assets/extend-component/banner-text-dialog.png)

1. Preencha o componente com uma imagem:

   ![Caixa de diálogo Adicionar imagem ao banner](assets/extend-component/banner-dialog-image.png)

   Salve as atualizações da caixa de diálogo.

1. Agora você deve ver o valor renderizado de **Texto do banner**:

   ![Texto do banner exibido](assets/extend-component/banner-text-displayed.png)

1. Visualize a resposta do modelo JSON em: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) e pesquise a `wknd-spa-react/components/card`:

   ```json
   "banner": {
       "bannerText": "My Banner Text",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/banner.coreimg.jpeg/1622167884688/sport-climbing.jpeg",
       "alt": "alt banner rock climber",
       ":type": "wknd-spa-react/components/banner"
    },
   ```

   Observe que o modelo JSON é atualizado com pares de chave/valor adicionais após a implementação do Modelo do Sling em `BannerModelImpl.java`.

## Parabéns.  {#congratulations}

Parabéns, você aprendeu a estender um componente de AEM usando o e como os Modelos e diálogos do Sling funcionam com o modelo JSON.
