---
title: Estender um componente principal | Introdução ao Editor de SPA de AEM e React
description: Saiba como estender o Modelo JSON para um Componente principal existente a ser usado com o Editor de SPA AEM. Entender como adicionar propriedades e conteúdo a um componente existente é uma técnica avançada para expandir os recursos de uma implementação do Editor de SPA AEM. Saiba como usar o padrão de delegação para estender Modelos do Sling e recursos do Sling Resource Merger.
sub-product: sites
feature: Editor de SPA, Componentes principais
doc-type: tutorial
version: cloud-service
kt: 5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 24d70ebaa6a63cfd4a73f43188f25b375dc702ec
workflow-type: tm+mt
source-wordcount: '1097'
ht-degree: 1%

---


# Estender um componente principal {#extend-component}

Saiba como estender um Componente principal existente a ser usado com o Editor de SPA de AEM. Entender como estender um componente existente é uma técnica poderosa para personalizar e expandir os recursos de uma implementação AEM SPA Editor.

## Objetivo

1. Estenda um Componente principal existente com propriedades e conteúdo adicionais.
2. Entenda a base da Herança de componente com o uso de `sling:resourceSuperType`.
3. Saiba como aproveitar o [Padrão de delegação](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) para Modelos do Sling para reutilizar a lógica e funcionalidade existentes.

## O que você vai criar

Este capítulo ilustra o código adicional necessário para adicionar uma propriedade extra a um componente `Image` padrão para atender aos requisitos de um novo componente `Banner`. O componente `Banner` contém todas as mesmas propriedades que o componente `Image` padrão, mas inclui uma propriedade adicional para que os usuários preencham o **Texto do banner**.

![Componente de banner de criação final](assets/extend-component/final-author-banner-component.png)

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment). Pressupõe-se que neste ponto os usuários do tutorial tenham uma sólida compreensão do recurso Editor de SPA AEM.

## Herança com o Supertipo de Recurso Sling {#sling-resource-super-type}

Para estender um componente existente, defina uma propriedade chamada `sling:resourceSuperType` na definição do componente.  `sling:resourceSuperType`é uma  [](https://sling.apache.org/documentation/the-sling-engine/resources.html#resource-properties) propriedade que pode ser definida na definição de um componente AEM que aponta para outro componente. Isso define explicitamente o componente para herdar toda a funcionalidade do componente identificado como `sling:resourceSuperType`.

Se quiser estender o componente `Image` em `wknd-spa-react/components/image`, precisamos atualizar o código no módulo `ui.apps`.

1. Crie uma nova pasta abaixo do módulo `ui.apps` para `banner` em `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/banner`.
1. Abaixo de `banner` crie uma definição de Componente (`.content.xml`) da seguinte maneira:

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

O arquivo `_cq_editConfig.xml` determina o comportamento de arrastar e soltar na interface do usuário de criação de AEM. Ao estender o componente de Imagem , é importante que o tipo de recurso corresponda ao próprio componente.

1. No módulo `ui.apps`, crie outro arquivo sob `banner` chamado `_cq_editConfig.xml`.
1. Preencha `_cq_editConfig.xml` com o seguinte XML:

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

1. O aspecto exclusivo do arquivo é o nó `<parameters>` que define resourceType como `wknd-spa-react/components/banner`.

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

Nosso componente `Banner` requer um campo de texto extra na caixa de diálogo para capturar o `bannerText`. Como estamos usando a herança Sling, podemos usar os recursos do [Sling Resource Merger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html) para substituir ou estender partes do diálogo. Neste exemplo, uma nova guia foi adicionada à caixa de diálogo para capturar dados adicionais de um autor para preencher o Componente de cartão.

1. No módulo `ui.apps`, abaixo da pasta `banner`, crie uma pasta chamada `_cq_dialog`.
1. Abaixo de `_cq_dialog` crie um arquivo de definição da caixa de diálogo `.content.xml`. Preencha-o com o seguinte:

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

   A definição XML acima criará uma nova guia chamada **Text** e a ordenará *antes de* da guia **Asset** existente. Ele conterá um único campo **Texto do banner**.

1. A caixa de diálogo terá a seguinte aparência:

   ![Caixa de diálogo final do banner](assets/extend-component/banner-dialog.png)

   Observe que não precisávamos definir as guias para **Asset** ou **Metadados**. Eles são herdados por meio da propriedade `sling:resourceSuperType` .

   Antes de poder visualizar a caixa de diálogo, precisamos implementar o Componente de SPA e a função `MapTo`.

## Implementar SPA componente {#implement-spa-component}

Para usar o componente Banner com o Editor de SPA, um novo componente de SPA deve ser criado e mapeado para `wknd-spa-react/components/banner`. Isso será feito no módulo `ui.frontend`.

1. No módulo `ui.frontend` crie uma nova pasta para `Banner` em `ui.frontend/src/components/Banner`.
1. Crie um novo arquivo chamado `Banner.js` abaixo da pasta `Banner`. Preencha-o com o seguinte:

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

   Esse componente de SPA mapeia para o componente de AEM `wknd-spa-react/components/banner` criado anteriormente.

1. Atualize `import-components.js` em `ui.frontend/src/components/import-components.js` para incluir o novo componente de SPA `Banner`:

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

1. Atualize a política do Modelo de SPA para adicionar o componente `Banner` como um **componente permitido**.

1. Navegue até uma página de SPA e adicione o componente `Banner` a uma das páginas de SPA:

   ![Adicionar componente Banner](assets/extend-component/add-banner-component.png)

   >[!NOTE]
   >
   > A caixa de diálogo permitirá salvar um valor para **Banner Text**, mas esse valor não é refletido no componente SPA. Para habilitar, precisamos estender o Modelo do Sling para o componente.

## Adicionar interface Java {#java-interface}

Para expor os valores da caixa de diálogo do componente ao componente React , precisamos atualizar o Modelo do Sling que preenche o JSON para o componente `Banner`. Isso será feito no módulo `core` que contém todo o código Java do projeto SPA.

Primeiro, criaremos uma nova interface Java para `Banner` que estende a interface Java `Image`.

1. No módulo `core` crie um novo arquivo chamado `BannerModel.java` em `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. Preencha `BannerModel.java` com o seguinte:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.osgi.annotation.versioning.ProviderType;
   
   @ProviderType
   public interface BannerModel extends Image {
   
       public String getBannerText();
   
   }
   ```

   Isso herdará todos os métodos da interface do Componente principal `Image` e adicionará um novo método `getBannerText()`.

## Implementar o Modelo Sling {#sling-model}

Em seguida, implemente o Modelo do Sling para a interface `BannerModel`.

1. No módulo `core` crie um novo arquivo chamado `BannerModelImpl.java` em `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models/impl`.

1. Preencha `BannerModelImpl.java` com o seguinte:

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

   Observe a utilização das anotações `@Model` e `@Exporter` para garantir que o Modelo do Sling possa ser serializado como JSON por meio do Exportador de Modelo do Sling.

   `BannerModelImpl.java` O usa o padrão  [Delegação para ](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) Modelos do Sling para evitar a regravação de toda a lógica do componente principal Imagem .

1. Observe as seguintes linhas:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   A anotação acima instanciará um objeto de Imagem chamado `image` com base na herança `sling:resourceSuperType` do componente `Banner`.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   É possível simplesmente usar o objeto `image` para implementar métodos definidos pela interface `Image`, sem precisar gravar a lógica por conta própria. Essa técnica é usada para `getSrc()`, `getAlt()` e `getTitle()`.

1. Abra uma janela de terminal e implante apenas as atualizações do módulo `core` usando o perfil Maven `autoInstallBundle` do diretório `core`.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

## Colocando tudo junto {#put-together}

1. Retorne para AEM e abra a página SPA que tem o componente `Banner`.
1. Atualize o componente `Banner` para incluir **Texto do banner**:

   ![Texto do banner](assets/extend-component/banner-text-dialog.png)

1. Preencha o componente com uma imagem:

   ![Caixa de diálogo Adicionar imagem ao banner](assets/extend-component/banner-dialog-image.png)

   Salve as atualizações da caixa de diálogo.

1. Agora você deve ver o valor renderizado de **Texto do banner**:

   ![Texto do banner exibido](assets/extend-component/banner-text-displayed.png)

1. Visualize a resposta do modelo JSON em: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) e procure pelo `wknd-spa-react/components/card`:

   ```json
   "banner": {
       "bannerText": "My Banner Text",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/banner.coreimg.jpeg/1622167884688/sport-climbing.jpeg",
       "alt": "alt banner rock climber",
       ":type": "wknd-spa-react/components/banner"
    },
   ```

   Observe que o modelo JSON é atualizado com pares de chave/valor adicionais após a implementação do Modelo do Sling em `BannerModelImpl.java`.

## Parabéns! {#congratulations}

Parabéns, você aprendeu a estender um componente de AEM usando o e como os Modelos e diálogos do Sling funcionam com o modelo JSON.

