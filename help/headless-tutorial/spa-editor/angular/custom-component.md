---
title: Criar um componente personalizado | Introdução ao AEM SPA Editor e Angular
description: Saiba como criar um componente personalizado a ser usado com o Editor SPA do AEM. Saiba como desenvolver caixas de diálogo de criação e Modelos Sling para estender o modelo JSON e preencher um componente personalizado.
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-5831
thumbnail: 5831-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 6c1c7f2b-f574-458c-b744-b92419c46f23
duration: 308
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '1321'
ht-degree: 0%

---

# Criar um componente personalizado {#custom-component}

{{spa-editor-deprecation}}

Saiba como criar um componente personalizado a ser usado com o Editor SPA do AEM. Saiba como desenvolver caixas de diálogo de criação e Modelos Sling para estender o modelo JSON e preencher um componente personalizado.

## Objetivo

1. Entenda a função dos Modelos do Sling na manipulação da API do modelo JSON fornecida pelo AEM.
2. Entenda como criar caixas de diálogo de componente do AEM.
3. Saiba como criar um Componente AEM **personalizado** que seja compatível com a estrutura do editor de SPA.

## O que você vai criar

O foco dos capítulos anteriores era desenvolver componentes de SPA e mapeá-los para *Componentes principais do AEM* existentes. Este capítulo se concentra em como criar e estender *novos* componentes do AEM e manipular o modelo JSON fornecido pelo AEM.

Um simples `Custom Component` ilustra as etapas necessárias para criar um novo componente do AEM.

![Mensagem exibida em Todas as Maiúsculas](assets/custom-component/message-displayed.png)

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Obter o código

1. Baixe o ponto de partida para este tutorial pelo Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. Implante a base de código em uma instância do AEM local usando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se estiver usando o [AEM 6.x](overview.md#compatibility), adicione o perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Instale o pacote concluído para o [site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) tradicional. As imagens fornecidas pelo [site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) são reutilizadas no SPA do WKND. O pacote pode ser instalado usando o [Gerenciador de Pacotes](http://localhost:4502/crx/packmgr/index.jsp) da AEM.

   ![Instalar wknd.all](./assets/map-components/package-manager-wknd-all.png) do Gerenciador de Pacotes

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) ou conferir o código localmente alternando para a ramificação `Angular/custom-component-solution`.

## Definir o componente do AEM

Um componente do AEM é definido como um nó e propriedades. No projeto, esses nós e propriedades são representados como arquivos XML no módulo `ui.apps`. Em seguida, crie o componente AEM no módulo `ui.apps`.

>[!NOTE]
>
> Uma atualização rápida das [noções básicas dos componentes do AEM pode ser útil](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html?lang=pt-BR).

1. Abra a pasta `ui.apps` no IDE de sua escolha.
2. Navegue até `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` e crie uma pasta chamada `custom-component`.
3. Crie um arquivo chamado `.content.xml` abaixo da pasta `custom-component`. Preencha o `custom-component/.content.xml` com o seguinte:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![Criar definição de componente personalizado](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - identifica que este nó é um componente do AEM.

   `jcr:title` é o valor exibido para os Autores de Conteúdo e `componentGroup` determina o agrupamento de componentes na interface de criação.

4. Abaixo da pasta `custom-component`, crie outra pasta chamada `_cq_dialog`.
5. Abaixo da pasta `_cq_dialog`, crie um arquivo chamado `.content.xml` e preencha-o com o seguinte:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Custom Component"
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
                                               <message
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldDescription="The text to display on the component."
                                                   fieldLabel="Message"
                                                   name="./message"/>
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

   O arquivo XML acima gera uma caixa de diálogo simples para `Custom Component`. A parte crítica do arquivo é o nó interno `<message>`. Esta caixa de diálogo contém um `textfield` simples chamado `Message` e mantém o valor do campo de texto para uma propriedade chamada `message`.

   Um Modelo Sling é criado próximo a expor o valor da propriedade `message` por meio do modelo JSON.

   >[!NOTE]
   >
   > Você pode exibir muito mais [exemplos de caixas de diálogo ao exibir as definições dos Componentes principais](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). Você também pode exibir campos de formulário adicionais, como `select`, `textarea`, `pathfield`, disponíveis em `/libs/granite/ui/components/coral/foundation/form` no [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   Com um componente tradicional do AEM, geralmente é necessário um script [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html?lang=pt-BR). Como o SPA renderiza o componente, nenhum script HTL é necessário.

## Criar o modelo do Sling

Os Modelos do Sling são objetos POJO (Plain Old Java™ Objects) do Java™ orientados por anotações que facilitam o mapeamento de dados do JCR para as variáveis do Java™. Os [Modelos do Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html?lang=pt-BR#sling-models) geralmente funcionam para encapsular uma lógica de negócios complexa do lado do servidor para Componentes do AEM.

No contexto do Editor SPA, os Modelos Sling expõem o conteúdo de um componente por meio do modelo JSON por meio de um recurso que usa o [Exportador de Modelo Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=pt-BR).

1. No IDE de sua escolha, abra o módulo `core`. `CustomComponent.java` e `CustomComponentImpl.java` já foram criados e extraídos como parte do código inicial do capítulo.

   >[!NOTE]
   >
   > Se estiver usando o Visual Studio Code IDE, pode ser útil instalar [extensões para o Java™](https://code.visualstudio.com/docs/java/extensions).

2. Abra a interface Java™ `CustomComponent.java` em `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`:

   ![Interface CustomComponent.java](assets/custom-component/custom-component-interface.png)

   Essa é a interface Java™ implementada pelo Modelo Sling.

3. Atualizar `CustomComponent.java` para que ele estenda a interface `ComponentExporter`:

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   A implementação da interface `ComponentExporter` é um requisito para que o Modelo Sling seja selecionado automaticamente pela API do modelo JSON.

   A interface `CustomComponent` inclui um único método getter `getMessage()`. Esse é o método que expõe o valor da caixa de diálogo do autor por meio do modelo JSON. Somente métodos Getter com parâmetros vazios `()` são exportados no modelo JSON.

4. Abrir `CustomComponentImpl.java` em `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   Esta é a implementação da interface `CustomComponent`. A anotação `@Model` identifica a classe Java™ como um Modelo Sling. A anotação `@Exporter` permite que a classe Java™ seja serializada e exportada por meio do Exportador de Modelo do Sling.

5. Atualize a variável estática `RESOURCE_TYPE` para apontar para o componente AEM `wknd-spa-angular/components/custom-component` criado no exercício anterior.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   O tipo de recurso do componente é o que vincula o Modelo do Sling ao componente do AEM e, por fim, mapeia para o componente do Angular.

6. Adicione o método `getExportedType()` à classe `CustomComponentImpl` para retornar o tipo de recurso do componente:

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   Este método é necessário ao implementar a interface `ComponentExporter` e expõe o tipo de recurso que permite o mapeamento para o componente do Angular.

7. Atualize o método `getMessage()` para retornar o valor da propriedade `message` persistida pela caixa de diálogo do autor. Use a anotação `@ValueMap` para mapear o valor JCR `message` para uma variável Java™:

   ```java
   import org.apache.commons.lang3.StringUtils;
   ...
   
   @ValueMapValue
   private String message;
   
   @Override
   public String getMessage() {
       return StringUtils.isNotBlank(message) ? message.toUpperCase() : null;
   }
   ```

   Alguma &quot;lógica de negócios&quot; adicional é adicionada para retornar o valor da mensagem como maiúsculas. Isso nos permite ver a diferença entre o valor bruto armazenado pela caixa de diálogo do autor e o valor exposto pelo Modelo do Sling.

   >[!NOTE]
   >
   > Você pode exibir o [CustomComponentImpl.java concluído aqui](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java).

## Atualizar o componente do Angular

O código Angular do componente personalizado já foi criado. Em seguida, faça algumas atualizações para mapear o componente Angular para o componente AEM.

1. No módulo `ui.frontend`, abra o arquivo `ui.frontend/src/app/components/custom/custom.component.ts`
2. Observe a linha `@Input() message: string;`. Espera-se que o valor em maiúsculas transformado seja mapeado para essa variável.
3. Importe o objeto `MapTo` do AEM SPA Editor JS SDK e use-o para mapear para o componente AEM:

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. Abra `cutom.component.html` e observe que o valor de `{{message}}` é exibido ao lado de uma tag `<h2>`.
5. Abra `custom.component.css` e adicione a seguinte regra:

   ```css
   :host-context {
       display: block;
   }
   ```

   Para que o Espaço Reservado do Editor do AEM seja exibido corretamente, quando o componente estiver vazio, o `:host-context` ou outro `<div>` precisa ser definido como `display: block;`.

6. Implante as atualizações em um ambiente local do AEM a partir da raiz do diretório do projeto, usando suas habilidades em Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Atualizar a política do modelo

Em seguida, navegue até o AEM para verificar as atualizações e permitir que o `Custom Component` seja adicionado ao SPA.

1. Verifique o registro do novo Modelo do Sling navegando até [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   Você deve ver as duas linhas acima que indicam que `CustomComponentImpl` está associado ao componente `wknd-spa-angular/components/custom-component` e que ele está registrado por meio do Exportador de Modelo do Sling.

2. Navegue até o Modelo de página do SPA em [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Atualize a política do Contêiner de Layout para adicionar o novo `Custom Component` como um componente permitido:

   ![Atualizar política de Contêiner de Layout](assets/custom-component/custom-component-allowed.png)

   Salve as alterações na política e observe o `Custom Component` como um componente permitido:

   ![Componente personalizado como um componente permitido](assets/custom-component/custom-component-allowed-layout-container.png)

## Criar o componente personalizado

Em seguida, crie o `Custom Component` usando o Editor SPA do AEM.

1. Navegue até [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. No modo `Edit`, adicione o `Custom Component` ao `Layout Container`:

   ![Inserir novo componente](assets/custom-component/insert-custom-component.png)

3. Abra a caixa de diálogo do componente e insira uma mensagem que contenha algumas letras minúsculas.

   ![Configurar o componente personalizado](assets/custom-component/enter-dialog-message.png)

   Esta é a caixa de diálogo que foi criada com base no arquivo XML mencionado anteriormente no capítulo.

4. Salve as alterações. Observe que a mensagem exibida está em maiúsculas.

   ![Mensagem exibida em Todas as Maiúsculas](assets/custom-component/message-displayed.png)

5. Exiba o modelo JSON navegando até [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Pesquisar por `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   Observe que o valor JSON está definido como todas as letras maiúsculas com base na lógica adicionada ao Modelo Sling.

## Parabéns! {#congratulations}

Parabéns, você aprendeu a criar um componente AEM personalizado e como os Modelos e caixas de diálogo do Sling funcionam com o modelo JSON.

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) ou conferir o código localmente alternando para a ramificação `Angular/custom-component-solution`.

### Próximas etapas {#next-steps}

[Estender um Componente Principal](extend-component.md) - Saiba como estender um Componente Principal existente para ser usado com o Editor SPA do AEM. Entender como adicionar propriedades e conteúdo a um componente existente é uma técnica poderosa para expandir os recursos de uma implementação do Editor SPA do AEM.
