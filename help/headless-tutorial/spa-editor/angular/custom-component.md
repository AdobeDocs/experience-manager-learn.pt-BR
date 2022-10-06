---
title: Criar um componente personalizado | Introdução ao AEM SPA Editor e Angular
description: Saiba como criar um componente personalizado a ser usado com o Editor de SPA de AEM. Saiba como desenvolver caixas de diálogo do autor e Modelos do Sling para estender o modelo JSON para preencher um componente personalizado.
sub-product: sites
feature: SPA Editor
doc-type: tutorial
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 5831
thumbnail: 5831-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 6c1c7f2b-f574-458c-b744-b92419c46f23
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1471'
ht-degree: 3%

---

# Criar um componente personalizado {#custom-component}

Saiba como criar um componente personalizado a ser usado com o Editor de SPA de AEM. Saiba como desenvolver caixas de diálogo do autor e Modelos do Sling para estender o modelo JSON para preencher um componente personalizado.

## Objetivo

1. Entenda a função dos Modelos do Sling em manipular a API de modelo JSON fornecida pelo AEM.
2. Entenda como criar caixas de diálogo AEM componente.
3. Saiba como criar um **custom** AEM componente compatível com a estrutura do editor de SPA.

## O que você vai criar

O foco dos capítulos anteriores era desenvolver componentes SPA e mapeá-los para *existente* AEM Componentes principais. Este capítulo foca em como criar e estender *novo* AEM componentes e manipule o modelo JSON servido por AEM.

Um simples `Custom Component` ilustra as etapas necessárias para criar um novo componente de AEM.

![Mensagem exibida em todas as maiúsculas](assets/custom-component/message-displayed.png)

## Pré-requisitos

Revise as ferramentas necessárias e as instruções para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Obter o código

1. Baixe o ponto de partida para este tutorial via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. Implante a base de código em uma instância de AEM local usando o Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se estiver usando [AEM 6.x](overview.md#compatibility) adicione o `classic` perfil:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Instale o pacote concluído para o pacote tradicional [Site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). As imagens fornecidas por [Site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) são reutilizadas no SPA WKND. O pacote pode ser instalado usando [Gerenciador de pacotes de AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![O Gerenciador de Pacotes instala o wknd.all](./assets/map-components/package-manager-wknd-all.png)

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) ou faça check-out do código localmente, alternando para a ramificação `Angular/custom-component-solution`.

## Definir o componente AEM

Um componente AEM é definido como um nó e propriedades. No projeto, esses nós e propriedades são representados como arquivos XML no `ui.apps` módulo. Em seguida, crie o componente AEM no `ui.apps` módulo.

>[!NOTE]
>
> Uma atualização rápida sobre o [noções básicas dos componentes de AEM podem ser úteis](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. Abra o `ui.apps` no IDE de sua escolha.
2. Navegar para `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` e criar uma pasta chamada `custom-component`.
3. Crie um arquivo com o nome `.content.xml` abaixo do `custom-component` pasta. Preencha o `custom-component/.content.xml` com o seguinte:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![Criar definição de componente personalizado](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - identifica que este nó é um componente AEM.

   `jcr:title` é o valor exibido para Autores de conteúdo e o `componentGroup` determina o agrupamento de componentes na interface do usuário de criação.

4. Abaixo da `custom-component` pasta, crie outra pasta chamada `_cq_dialog`.
5. Abaixo da `_cq_dialog` pasta criar um arquivo chamado `.content.xml` e preencha com o seguinte:

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

   O arquivo XML acima gera uma caixa de diálogo simples para a variável `Custom Component`. A parte crítica do arquivo é a parte interna `<message>` nó . Esta caixa de diálogo contém um `textfield` nomeado `Message` e persiste o valor do campo de texto para uma propriedade chamada `message`.

   Um Modelo do Sling é criado ao lado da exposição do valor da variável `message` propriedade por meio do modelo JSON.

   >[!NOTE]
   >
   > Você pode visualizar muito mais [exemplos de caixas de diálogo ao exibir as definições dos Componentes principais](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). Também é possível exibir campos de formulário adicionais, como `select`, `textarea`, `pathfield`, disponível abaixo de `/libs/granite/ui/components/coral/foundation/form` em [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   Com um componente de AEM tradicional, uma [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html) geralmente é necessário. Como o SPA renderiza o componente, nenhum script HTL é necessário.

## Criar o Modelo do Sling

Os Modelos do Sling são objetos Java™ POJOs (Plain Old Java™ Objects) orientados por anotações que facilitam o mapeamento de dados do JCR para variáveis Java™. [Modelos Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html#sling-models) O geralmente funciona para encapsular a lógica comercial complexa do lado do servidor para componentes do AEM.

No contexto do Editor de SPA, os Modelos do Sling expõem o conteúdo de um componente por meio do modelo JSON por meio de um recurso usando o [Exportador de Modelo Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=pt-BR).

1. No IDE de sua escolha, abra o `core` módulo. `CustomComponent.java` e `CustomComponentImpl.java` já foram criadas e eliminadas como parte do código inicial do capítulo.

   >[!NOTE]
   >
   > Se estiver usando o Visual Studio Code IDE, pode ser útil instalar [extensões para Java™](https://code.visualstudio.com/docs/java/extensions).

2. Abra a interface do Java™ `CustomComponent.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`:

   ![Interface CustomComponent.java](assets/custom-component/custom-component-interface.png)

   Essa é a interface Java™ implementada pelo Modelo do Sling.

3. Atualizar `CustomComponent.java` de forma a estender a `ComponentExporter` interface:

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   Implementar o `ComponentExporter` interface é um requisito para que o Modelo do Sling seja selecionado automaticamente pela API do modelo JSON.

   O `CustomComponent` a interface inclui um único método getter `getMessage()`. Esse é o método que expõe o valor da caixa de diálogo do autor por meio do modelo JSON. Somente métodos get com parâmetros vazios `()` são exportadas no modelo JSON.

4. Abrir `CustomComponentImpl.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   Essa é a implementação da variável `CustomComponent` interface. O `@Model` A anotação identifica a classe Java™ como um Modelo do Sling. O `@Exporter` A anotação permite que a classe Java™ seja serializada e exportada por meio do Sling Model Exporter.

5. Atualizar a variável estática `RESOURCE_TYPE` apontar para o componente AEM `wknd-spa-angular/components/custom-component` criado no exercício anterior.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   O tipo de recurso do componente é o que vincula o Modelo do Sling ao componente do AEM e, em última análise, mapeia para o componente do Angular.

6. Adicione o `getExportedType()` para `CustomComponentImpl` classe para retornar o tipo de recurso do componente:

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   Esse método é necessário ao implementar o `ComponentExporter` e expõe o tipo de recurso que permite o mapeamento para o componente de Angular.

7. Atualize o `getMessage()` para retornar o valor da variável `message` propriedade mantida pela caixa de diálogo do autor. Use o `@ValueMap` a anotação é mapear o valor do JCR `message` para uma variável Java™:

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

   Alguns outros termos de &quot;lógica comercial&quot; são adicionados para retornar o valor da mensagem em maiúsculas. Isso nos permite ver a diferença entre o valor bruto armazenado pela caixa de diálogo do autor e o valor exposto pelo Modelo do Sling.

   >[!NOTE]
   Você pode visualizar o [concluído CustomComponentImpl.java aqui](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java).

## Atualizar o componente do Angular

O código de Angular do Componente personalizado já foi criado. Em seguida, faça algumas atualizações para mapear o componente Angular para o componente AEM.

1. No `ui.frontend` módulo abre o arquivo `ui.frontend/src/app/components/custom/custom.component.ts`
2. Observe a `@Input() message: string;` linha. Espera-se que o valor em maiúsculas transformado seja mapeado para essa variável.
3. Importe o `MapTo` objeto do AEM SPA Editor JS SDK e use-o para mapear para o componente AEM:

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. Abrir `cutom.component.html` e observe que o valor de `{{message}}` é exibido no lado de um `<h2>` .
5. Abrir `custom.component.css` e adicione a seguinte regra:

   ```css
   :host-context {
       display: block;
   }
   ```

   Para que o Espaço reservado do Editor de AEM seja exibido corretamente quando o componente estiver vazio, o `:host-context` ou outro `<div>` precisa ser definido como `display: block;`.

6. Implante as atualizações em um ambiente de AEM local na raiz do diretório do projeto, usando suas habilidades Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Atualizar a Política de Modelo

Em seguida, navegue até AEM para verificar as atualizações e permitir a `Custom Component` a ser adicionado ao SPA.

1. Verifique o registro do novo Modelo do Sling navegando até [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   Você deve ver as duas linhas acima que indicam a variável `CustomComponentImpl` está associada ao `wknd-spa-angular/components/custom-component` e que está registrado através do Sling Model Exporter.

2. Navegue até o modelo de página de SPA em [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Atualize a política do Contêiner de layout para adicionar o novo `Custom Component` como um componente permitido:

   ![Atualizar política do contêiner de layout](assets/custom-component/custom-component-allowed.png)

   Salve as alterações na política e observe o `Custom Component` como um componente permitido:

   ![Componente personalizado como um componente permitido](assets/custom-component/custom-component-allowed-layout-container.png)

## Crie o componente personalizado

Em seguida, crie o `Custom Component` usando o Editor de SPA AEM.

1. Navegar para [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. Em `Edit` , adicione o `Custom Component` para `Layout Container`:

   ![Inserir novo componente](assets/custom-component/insert-custom-component.png)

3. Abra a caixa de diálogo do componente e insira uma mensagem que contenha algumas letras minúsculas.

   ![Configurar o componente personalizado](assets/custom-component/enter-dialog-message.png)

   Esta é a caixa de diálogo que foi criada com base no arquivo XML anterior no capítulo.

4. Salve as alterações. Observe que a mensagem exibida está em todas as letras maiúsculas e minúsculas.

   ![Mensagem exibida em todas as maiúsculas](assets/custom-component/message-displayed.png)

5. Visualize o modelo JSON navegando até [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Pesquisar `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   Observe que o valor JSON é definido como todas as letras maiúsculas com base na lógica adicionada ao Modelo do Sling.

## Parabéns.  {#congratulations}

Parabéns, você aprendeu a criar um componente de AEM personalizado e como os Modelos e diálogos do Sling funcionam com o modelo JSON.

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) ou faça check-out do código localmente, alternando para a ramificação `Angular/custom-component-solution`.

### Próximas etapas {#next-steps}

[Estender um componente principal](extend-component.md) - Saiba como estender um Componente principal existente para ser usado com o Editor de SPA de AEM. Entender como adicionar propriedades e conteúdo a um componente existente é uma técnica avançada para expandir os recursos de uma implementação do Editor de SPA AEM.
