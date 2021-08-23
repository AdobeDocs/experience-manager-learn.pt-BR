---
title: Tutorial do mundo sobre desenvolvimento com o AEM SPA Editor - Hello
description: AEM Editor de SPA fornece suporte para a edição no contexto de um Aplicativo de página única ou SPA. Este tutorial é uma introdução SPA desenvolvimento a ser usado com AEM Editor JS SDK. O tutorial estenderá o aplicativo do We.Retail Journal adicionando um componente personalizado Hello World. Os usuários podem concluir o tutorial usando estruturas React ou Angular.
version: 6.3, 6.4, 6.5
topic: SPA
feature: Editor de SPA
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '3170'
ht-degree: 1%

---


# Tutorial do mundo sobre desenvolvimento com o AEM SPA Editor - Hello {#developing-with-the-aem-spa-editor-hello-world-tutorial}

>[!WARNING]
>
> Este tutorial é **obsoleto**. É recomendável seguir um destes procedimentos: [Introdução ao Editor de SPA de AEM e Angular](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/spa-editor/angular/overview.html) ou [Introdução ao Editor de SPA de AEM e React](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/spa-editor/react/overview.html)

AEM Editor de SPA fornece suporte para a edição no contexto de um Aplicativo de página única ou SPA. Este tutorial é uma introdução SPA desenvolvimento a ser usado com AEM Editor JS SDK. O tutorial estenderá o aplicativo do We.Retail Journal adicionando um componente personalizado Hello World. Os usuários podem concluir o tutorial usando estruturas React ou Angular.

>[!NOTE]
>
> O recurso Editor de aplicativo de página única (SPA) requer AEM 6.4 service pack 2 ou mais recente.
>
> O Editor de SPA é a solução recomendada para projetos que exigem renderização do lado do cliente baseada em SPA estrutura (por exemplo, Reagir ou Angular).

## Leitura de pré-requisito {#prereq}

Este tutorial destina-se a destacar as etapas necessárias para mapear um componente de SPA a um componente de AEM para ativar a edição no contexto. Os usuários que iniciam este tutorial devem conhecer os conceitos básicos de desenvolvimento com o Adobe Experience Manager, o AEM e o desenvolvimento com o React of Angular Framework. O tutorial aborda tarefas de desenvolvimento de back-end e de front-end.

Recomenda-se que os seguintes recursos sejam revisados antes de iniciar este tutorial:

* [Vídeo de recurso do editor de SPA](spa-editor-framework-feature-video-use.md)  - Uma visão geral em vídeo do editor de SPA e do aplicativo We.Retail Journal.
* [Tutorial do React.js](https://reactjs.org/tutorial/tutorial.html)  - uma introdução ao desenvolvimento com a estrutura React.
* [Tutorial do Angular](https://angular.io/tutorial)  - uma introdução ao desenvolvimento com o Angular

## Ambiente de desenvolvimento local {#local-dev}

Este tutorial foi projetado para:

[Adobe Experience Manager 6.5](https://helpx.adobe.com/experience-manager/6-5/release-notes.html) ou  [Adobe Experience Manager 6.4](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/technical-requirements.html) +  [Service Pack 5](https://helpx.adobe.com/br/experience-manager/6-4/release-notes/sp-release-notes.html)

Neste tutorial, as seguintes tecnologias e ferramentas devem ser instaladas:

1. [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
2. [Apache Maven - 3.3.1+](https://maven.apache.org/)
3. [Node.js - 8.11.1+](https://nodejs.org/en/) e npm 5.6.0+ (npm é instalado com node.js)

Verifique novamente a instalação das ferramentas acima, abrindo um novo terminal e executando o seguinte:

```shell
$ java -version
java version "11 +"

$ mvn -version
Apache Maven 3.3.9

$ node --version
v8.11.1

$ npm --version
6.1.0
```

## Visão geral {#overview}

O conceito básico é mapear um Componente de SPA para um Componente de AEM. AEM componentes, executando o lado do servidor, exporta conteúdo no formato JSON. O conteúdo JSON é consumido pelo SPA, executando no lado do cliente no navegador. Um mapeamento 1:1 entre SPA componentes e um componente AEM é criado.

![Mapeamento de componentes SPA](assets/spa-editor-helloworld-tutorial-use/mapto.png)

Estruturas populares [React JS](https://reactjs.org/) e [Angular](https://angular.io/) são suportadas imediatamente. Os usuários podem concluir este tutorial no Angular ou no React, qualquer estrutura com a qual estejam mais confortáveis.

## Configuração do projeto {#project-setup}

SPA desenvolvimento tem um pé AEM desenvolvimento, e o outro. O objetivo é permitir que SPA desenvolvimento ocorra de forma independente e (principalmente) agnóstico à AEM.

* SPA projetos podem funcionar independentemente do projeto AEM durante o desenvolvimento front-end.
* As ferramentas e tecnologias de build front-end como Webpack, NPM, [!DNL Grunt] e [!DNL Gulp]continuam a ser usadas.
* Para criar o para AEM, o projeto do SPA é compilado e automaticamente incluído no projeto do AEM.
* Pacotes de AEM padrão usados para implantar o SPA no AEM.

![Visão geral de artefatos e implantação](assets/spa-editor-helloworld-tutorial-use/spa-artifact-deployment.png)

*SPA desenvolvimento tem um pé no desenvolvimento AEM, e o outro fora - permitindo que o desenvolvimento SPA ocorra de forma independente e (principalmente) agnóstico à AEM.*

O objetivo deste tutorial é estender o aplicativo de diário We.Retail com um novo componente. Comece baixando o código-fonte do aplicativo We.Retail Journal e implantando em um AEM local.

1. **** Baixe o código do diário  [We.Retail mais recente em GitHub](https://github.com/adobe/aem-sample-we-retail-journal).

   Ou clone o repositório da linha de comando:

   ```shell
   $ git clone git@github.com:adobe/aem-sample-we-retail-journal.git
   ```

   >[!NOTE]
   >
   >O tutorial funcionará em relação à ramificação **principal** com a versão **1.2.1-SNAPSHOT** do projeto.

1. A seguinte estrutura deve ser visível:

   ![Estrutura da pasta do projeto](assets/spa-editor-helloworld-tutorial-use/folder-structure.png)

   O projeto contém os seguintes módulos maven:

   * `all`: Incorpora e instala o projeto inteiro em um único pacote.
   * `bundles`: Contém dois pacotes OSGi: commons e core que contêm  [!DNL Sling Models] e outro código Java.
   * `ui.apps`: contém as partes /apps do projeto, ou seja, clientlibs JS &amp; CSS, componentes, configurações específicas do modo de execução.
   * `ui.content`: contém conteúdo estrutural e configurações (`/content`,  `/conf`)
   * `react-app`: Aplicativo We.Retail Journal React. Este é um módulo Maven e um projeto do webpack.
   * `angular-app`: Aplicativo de Angular do diário We.Retail. Este é um módulo [!DNL Maven] e um projeto do webpack.

1. Abra uma nova janela de terminal e execute o seguinte comando para criar e implantar o aplicativo inteiro em uma instância de AEM local em execução em [http://localhost:4502](http://localhost:4502).

   ```shell
   $ cd <src>/aem-sample-we-retail-journal
   $ mvn -PautoInstallSinglePackage clean install
   ```

   >[!NOTE]
   >
   > Neste projeto, o perfil Maven para criar e empacotar o projeto inteiro é `autoInstallSinglePackage`

   >[!CAUTION]
   >
   > Se você receber um erro durante a criação, [verifique se o arquivo Maven settings.xml inclui o artefato Maven do repositório Maven](https://helpx.adobe.com/experience-manager/kb/SetUpTheAdobeMavenRepository.html).

1. Vá até:

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

   O aplicativo de diário We.Retail deve ser exibido no editor do AEM Sites.

1. No modo [!UICONTROL Editar], selecione um componente a ser editado e faça uma atualização do conteúdo.

   ![Editar um componente](assets/spa-editor-helloworld-tutorial-use/editcontent.png)

1. Selecione o ícone [!UICONTROL Propriedades da página] para abrir as [!UICONTROL Propriedades da página]. Selecione [!UICONTROL Editar Modelo] para abrir o modelo da página.

   ![Menu de propriedades da página](assets/spa-editor-helloworld-tutorial-use/page-properties.png)

1. Na versão mais recente do Editor de SPA, [Modelos editáveis](https://helpx.adobe.com/br/experience-manager/6-5/sites/developing/using/page-templates-editable.html) podem ser usados da mesma forma que com as implementações tradicionais de Sites. Isso será revisitado posteriormente com nosso componente personalizado.

   >[!NOTE]
   >
   > Somente o AEM 6.5 e o AEM 6.4 + **Service Pack 5** oferecem suporte para Modelos editáveis.

## Visão geral do desenvolvimento {#development-overview}

![Desenvolvimento de visão geral](assets/spa-editor-helloworld-tutorial-use/diagramv2.png)

SPA iterações de desenvolvimento ocorrem independentemente do AEM. Quando o SPA estiver pronto para ser implantado em AEM, as seguintes etapas de alto nível ocorrerão (como ilustrado acima).

1. A build AEM projeto é chamada, o que, por sua vez, aciona uma build do projeto SPA. O diário We.Retail usa o [**frontend-maven-plugin**](https://github.com/eirslett/frontend-maven-plugin).
1. O [**aem-clientlib-generator**](https://www.npmjs.com/package/aem-clientlib-generator) do projeto de SPA incorpora o SPA compilado como uma Biblioteca de Cliente AEM no projeto de AEM.
1. O projeto AEM gera um pacote AEM, incluindo o SPA compilado, além de qualquer outro código AEM de suporte.

## Criar AEM componente {#aem-component}

**Persona: Desenvolvedor de AEM**

Um componente de AEM será criado primeiro. O componente AEM é responsável pela renderização das propriedades JSON que são lidas pelo componente React . O componente de AEM também é responsável por fornecer uma caixa de diálogo para quaisquer propriedades editáveis do componente.

Usando [!DNL Eclipse] ou outro [!DNL IDE], importe o projeto We.Retail Journal Maven.

1. Atualize o reator **pom.xml** para remover o plug-in [!DNL Apache Rat]. Este plug-in verifica cada arquivo para garantir que haja um cabeçalho de licença. Para nossos propósitos, não precisamos nos preocupar com essa funcionalidade.

   Em **aem-sample-we-retail-journal/pom.xml** remova **apache-rate-plugin**:

   ```xml
   <!-- Remove apache-rat-plugin -->
   <plugin>
           <groupId>org.apache.rat</groupId>
           <artifactId>apache-rat-plugin</artifactId>
           <configuration>
               <excludes combine.children="append">
                   <exclude>*</exclude>
                       ...
               </excludes>
           </configuration>
           <executions>
                   <execution>
                       <phase>verify</phase>
                       <goals>
                           <goal>check</goal>
                       </goals>
               </execution>
           </executions>
       </plugin>
   ```

1. No módulo **we-retail-journal-content** (`<src>/aem-sample-we-retail-journal/ui.apps`) crie um novo nó sob `ui.apps/jcr_root/apps/we-retail-journal/components` chamado **helloworld** do tipo **cq:Component**.
1. Adicione as seguintes propriedades ao componente **helloworld**, representado em XML (`/helloworld/.content.xml`) abaixo:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:description="Hello World Component for We.Retail Journal"
       jcr:primaryType="cq:Component"
       jcr:title="Hello World"
       componentGroup="We.Retail Journal" />
   ```

   ![Componente Hello World](assets/spa-editor-helloworld-tutorial-use/hello-world-component.png)

   >[!NOTE]
   >
   > Para ilustrar o recurso Modelos editáveis, definimos propositalmente o `componentGroup="Custom Components"`. Em um projeto do mundo real, é melhor minimizar o número de grupos de componentes, de modo que um grupo melhor seria &quot;[!DNL We.Retail Journal]&quot; para corresponder aos outros componentes de conteúdo.
   >
   > Somente o AEM 6.5 e o AEM 6.4 + **Service Pack 5** oferecem suporte para modelos editáveis.

1. Em seguida, uma caixa de diálogo será criada para permitir que uma mensagem personalizada seja configurada para o componente **Hello World**. Abaixo de `/apps/we-retail-journal/components/helloworld` adicione um nome de nó **cq:dialog** de **nt:unstructured**.
1. O **cq:dialog** exibirá um único campo de texto que persiste texto em uma propriedade chamada **[!DNL message]**. Abaixo do recém-criado **cq:dialog** adicione os seguintes nós e propriedades, representados em XML abaixo (`helloworld/_cq_dialog/.content.xml`) :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="We.Retail Journal - Hello World"
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
                                                   fieldLabel="Message"
                                                   name="./message"
                                                   required="{Boolean}true"/>
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

   ![estrutura de arquivos](assets/spa-editor-helloworld-tutorial-use/updated-with-dialog.png)

   A definição de nó XML acima criará uma caixa de diálogo com um único campo de texto que permitirá que um usuário insira uma &quot;mensagem&quot;. Observe a propriedade `name="./message"` no nó `<message />`. Esse é o nome da propriedade que será armazenada no JCR no AEM.

1. Em seguida, uma caixa de diálogo de política vazia será criada (`cq:design_dialog`). A caixa de diálogo Política é necessária para ver o componente no Editor de modelo. Para esse caso de uso simples, uma caixa de diálogo ficará vazia.

   Abaixo de `/apps/we-retail-journal/components/helloworld` adicione um nome de nó `cq:design_dialog` de `nt:unstructured`.

   A configuração é representada no XML abaixo (`helloworld/_cq_design_dialog/.content.xml`)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
   jcr:primaryType="nt:unstructured" />
   ```

1. Implante a base de código no AEM a partir da linha de comando:

   ```shell
   $ cd <src>/aem-sample-we-retail-journal/content
   $ mvn -PautoInstallPackage clean install
   ```

   Em [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/global/components/helloworld), valide se o componente foi implantado inspecionando a pasta em `/apps/we-retail-journal/components:`

   ![Estrutura de componentes implantada no CRXDE Lite](assets/spa-editor-helloworld-tutorial-use/updated-component-withdialogs.png)

## Criar Modelo Sling {#create-sling-model}

**Persona: Desenvolvedor de AEM**

Em seguida, um [!DNL Sling Model] é criado para retornar o componente [!DNL Hello World]. Em um caso de uso tradicional do WCM, o [!DNL Sling Model] implementa qualquer lógica comercial e um script de renderização do lado do servidor (HTL) fará uma chamada para o [!DNL Sling Model]. Isso mantém o script de renderização relativamente simples.

[!DNL Sling Models] também são usadas no caso de uso de SPA para implementar a lógica de negócios do lado do servidor. A diferença é que no caso de uso [!DNL SPA], o [!DNL Sling Models] expõe seus métodos como JSON serializado.

>[!NOTE]
>
>Como prática recomendada, os desenvolvedores devem procurar usar [AEM Componentes principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR) sempre que possível. Entre outros recursos, os Componentes principais fornecem [!DNL Sling Models] uma saída JSON &quot;pronta para SPA&quot;, permitindo que os desenvolvedores se concentrem mais na apresentação front-end.

1. No editor de sua escolha, abra o projeto **we-retail-journal-commons** ( `<src>/aem-sample-we-retail-journal/bundles/commons`).
1. No pacote `com.adobe.cq.sample.spa.commons.impl.models`:
   * Crie uma nova classe chamada `HelloWorld`.
   * Adicionar uma interface de implementação para `com.adobe.cq.export.json.ComponentExporter.`

   ![Assistente de Nova Classe Java](assets/spa-editor-helloworld-tutorial-use/fig5.png)

   A interface `ComponentExporter` deve ser implementada para que [!DNL Sling Model] seja compatível com os Serviços de conteúdo AEM.

   ```java
    package com.adobe.cq.sample.spa.commons.impl.models;
   
    import com.adobe.cq.export.json.ComponentExporter;
   
    public class HelloWorld implements ComponentExporter {
   
        @Override
        public String getExportedType() {
            return null;
        }
    }
   ```

1. Adicione uma variável estática chamada `RESOURCE_TYPE` para identificar o tipo de recurso do componente [!DNL HelloWorld]:

   ```java
    ...
    public class HelloWorld implements ComponentExporter {
   
        static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
        ...
    }
   ```

1. Adicione as anotações OSGi para `@Model` e `@Exporter`. A anotação `@Model` registrará a classe como [!DNL Sling Model]. A anotação `@Exporter` exporá os métodos como JSON serializado usando a estrutura [!DNL Jackson Exporter].

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Exporter;
   import org.apache.sling.models.annotations.Model;
   import com.adobe.cq.export.json.ExporterConstants;
   ...
   
   @Model(
           adaptables = SlingHttpServletRequest.class,
           adapters = {ComponentExporter.class},
           resourceType = HelloWorld.RESOURCE_TYPE
   )
   @Exporter(
           name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
           extensions = ExporterConstants.SLING_MODEL_EXTENSION
   )
   public class HelloWorld implements ComponentExporter {
   
   ...
   ```

1. Implemente o método `getDisplayMessage()` para retornar a propriedade JCR `message`. Use a anotação [!DNL Sling Model] de `@ValueMapValue` para facilitar a recuperação da propriedade `message` armazenada abaixo do componente. A anotação `@Optional` é importante, pois quando o componente é adicionado pela primeira vez à página, `message` não será preenchido.

   Como parte da lógica de negócios, uma string, &quot;**Hello**&quot;, será anexada à mensagem.

   ```java
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.Optional;
   
   ...
   
   public class HelloWorld implements ComponentExporter {
   
      static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
      private static final String PREPEND_MSG = "Hello";
   
       @ValueMapValue @Optional
       private String message;
   
       public String getDisplayMessage() {
           if(message != null && message.length() > 0) {
               return PREPEND_MSG + " "  + message;
           }
           return null;
       }
   
   ...
   ```

   >[!NOTE]
   >
   > O nome do método `getDisplayMessage` é importante. Quando o [!DNL Sling Model] for serializado com o [!DNL Jackson Exporter], ele será exposto como uma propriedade JSON: `displayMessage`. O [!DNL Jackson Exporter] serializará e exporá todos os métodos `getter` que não utilizam um parâmetro (a menos que estejam marcados explicitamente para ignorar). Posteriormente, no aplicativo React/Angular, lemos esse valor de propriedade e o exibiremos como parte do aplicativo.

   O método `getExportedType` também é importante. O valor do componente `resourceType` será usado para &quot;mapear&quot; os dados JSON para o componente front-end (Angular / React). Vamos explorar isso na próxima seção.

1. Implemente o método `getExportedType()` para retornar o tipo de recurso do componente `HelloWorld`.

   ```java
    @Override
       public String getExportedType() {
           return RESOURCE_TYPE;
       }
   ```

   O código completo para [**HelloWorld.java** pode ser encontrado aqui.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/bundles/commons/HelloWorld.java)

1. Implante o código para AEM usando o Apache Maven:

   ```shell
   $ cd <src>/sample-we-retail-spa-content/bundles/commons
   $ mvn -PautoInstallPackage clean install
   ```

   Verifique a implantação e o registro do [!DNL Sling Model] navegando até [[!UICONTROL Status] > [!UICONTROL Sling Models]](http://localhost:4502/system/console/status-slingmodels) no console OSGi.

   Você deve ver que o `HelloWorld` Modelo do Sling está vinculado ao `we-retail-journal/components/helloworld` tipo de recurso do Sling e está registrado como [!DNL Sling Model Exporter Servlet]:

   ```shell
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld - we-retail-journal/components/helloworld
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld exports 'we-retail-journal/components/helloworld' with selector 'model' and extension '[Ljava.lang.String;@6480f3e5' with exporter 'jackson'
   ```

## Criar componente de reação {#react-component}

**Persona: Desenvolvedor front-end**

Em seguida, o componente React será criado. Abra o módulo **response-app** ( `<src>/aem-sample-we-retail-journal/react-app`) usando o editor de sua escolha.

>[!NOTE]
>
> Ignore esta seção se você estiver interessado apenas em [Angular development](#angular-component).

1. Dentro da pasta `react-app`, navegue até a pasta src. Expanda a pasta de componentes para exibir os arquivos de componentes React existentes.

   ![reagir à estrutura do arquivo de componentes](assets/spa-editor-helloworld-tutorial-use/react-components.png)

1. Adicione um novo arquivo abaixo da pasta de componentes chamada `HelloWorld.js`.
1. Abrir `HelloWorld.js`. Adicione uma declaração de importação para importar a biblioteca do componente React . Adicione uma segunda declaração de importação para importar o auxiliar `MapTo` fornecido pelo Adobe. A ajuda `MapTo` fornece um mapeamento do componente React para o JSON do componente AEM.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   ```

1. Abaixo das importações, crie uma nova classe chamada `HelloWorld` que estende a interface React `Component`. Adicione o método `render()` necessário à classe `HelloWorld`.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   class HelloWorld extends Component {
   
       render() {
   
       }
   }
   ```

1. A ajuda `MapTo` inclui automaticamente um objeto chamado `cqModel` como parte das props do componente React. O `cqModel` inclui todas as propriedades expostas pelo [!DNL Sling Model].

   Lembre-se de que o [!DNL Sling Model] criado anteriormente contém um método `getDisplayMessage()`. `getDisplayMessage()` é traduzido como uma chave JSON chamada  `displayMessage` when output.

   Implemente o método `render()` para gerar uma tag `h1` que contenha o valor de `displayMessage`. [JSX](https://reactjs.org/docs/introducing-jsx.html), uma extensão de sintaxe para JavaScript, é usada para retornar a marcação final do componente.

   ```js
   ...
   
   class HelloWorld extends Component {
       render() {
   
           if(this.props.displayMessage) {
               return (
                   <div className="cmp-helloworld">
                       <h1 className="cmp-helloworld_message">{this.props.displayMessage}</h1>
                   </div>
               );
           }
           return null;
       }
   }
   ```

1. Implemente um método de configuração de edição. Esse método é passado por meio da ajuda `MapTo` e fornece ao editor de AEM informações para exibir um espaço reservado, caso o componente esteja vazio. Isso ocorre quando o componente é adicionado ao SPA, mas ainda não foi criado. Adicione o seguinte abaixo da classe `HelloWorld`:

   ```js
   ...
   
   class HelloWorld extends Component {
       ...
   }
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   ```

1. No final do arquivo, chame o auxiliar `MapTo`, transmitindo a classe `HelloWorld` e `HelloWorldEditConfig`. Isso mapeará o Componente de reação para o componente de AEM com base no tipo de recurso do Componente de AEM: `we-retail-journal/components/helloworld`.

   ```js
   MapTo('we-retail-journal/components/helloworld')(HelloWorld, HelloWorldEditConfig);
   ```

   O código concluído para [**HelloWorld.js** pode ser encontrado aqui.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/react-app/components/HelloWorld.js)

1. Abra o arquivo `ImportComponents.js`. Ele pode ser encontrado em `<src>/aem-sample-we-retail-journal/react-app/src/ImportComponents.js`.

   Adicione uma linha para exigir o `HelloWorld.js` com os outros componentes no pacote JavaScript compilado:

   ```js
   ...
     require('./components/Text');
     require('./components/Image');
     require('./components/HelloWorld');
   ...
   ```

1. Na pasta `components` crie um novo arquivo chamado `HelloWorld.css` como um irmão de `HelloWorld.js.` Preencha o arquivo com o seguinte para criar um estilo básico para o componente `HelloWorld`:

   ```css
   /* HelloWorld.css to style HelloWorld component */
   
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. Abra novamente `HelloWorld.js` e atualize abaixo das instruções de importação para exigir `HelloWorld.css`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   require('./HelloWorld.css');
   
   ...
   ```

1. Implante o código para AEM usando o Apache Maven:

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. Em [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js) abra `/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js`. Execute uma pesquisa rápida para HelloWorld em app.js para verificar se o componente React foi incluído no aplicativo compilado.

   >[!NOTE]
   >
   > **app.** jis é o aplicativo React fornecido. O código não é mais legível por humanos. O comando `npm run build` acionou uma build otimizada que gera JavaScript compilado que pode ser interpretado pelos navegadores modernos.


## Criar componente de Angular {#angular-component}

**Persona: Desenvolvedor front-end**

>[!NOTE]
>
> Ignore esta seção se você estiver interessado apenas no desenvolvimento do React.

Em seguida, o componente Angular será criado. Abra o módulo **angular-app** (`<src>/aem-sample-we-retail-journal/angular-app`) usando o editor de sua escolha.

1. Dentro da pasta `angular-app`, navegue até a pasta `src` correspondente. Expanda a pasta de componentes para exibir os arquivos de componentes do Angular existentes.

   ![Estrutura do arquivo de angular](assets/spa-editor-helloworld-tutorial-use/angular-file-structure.png)

1. Adicione uma nova pasta abaixo da pasta de componentes chamada `helloworld`. Abaixo da pasta `helloworld` adicione novos arquivos chamados `helloworld.component.css, helloworld.component.html, helloworld.component.ts`.

   ```plain
   /angular-app
       /src
           /app
               /components
   +                /helloworld
   +                    helloworld.component.css
   +                    helloworld.component.html
   +                    helloworld.component.ts
   ```

1. Abrir `helloworld.component.ts`. Adicione uma instrução import para importar as classes Angular `Component` e `Input`. Crie um novo componente, apontando `styleUrls` e `templateUrl` para `helloworld.component.css` e `helloworld.component.html`. Por fim, exporte a classe `HelloWorldComponent` com a entrada esperada de `displayMessage`.

   ```js
   //helloworld.component.ts
   
   import { Component, Input } from '@angular/core';
   
   @Component({
     selector: 'app-helloworld',
     host: { 'class': 'cmp-helloworld' },
     styleUrls:['./helloworld.component.css'],
     templateUrl: './helloworld.component.html',
   })
   
   export class HelloWorldComponent {
     @Input() displayMessage: string;
   }
   ```

   >[!NOTE]
   >
   > Se você lembrar o [!DNL Sling Model] criado anteriormente, havia um método **getDisplayMessage()**. O JSON serializado deste método será **displayMessage**, que agora estamos lendo no aplicativo Angular.

1. Abra `helloworld.component.html` para incluir uma tag `h1` que imprimirá a propriedade `displayMessage`:

   ```html
   <h1 *ngIf="displayMessage" class="cmp-helloworld_message">
       {{displayMessage}}
   </h1>
   ```

1. Atualize `helloworld.component.css` para incluir alguns estilos básicos do componente.

   ```css
   :host-context {
       display: block;
   };
   
   .cmp-helloworld {
       display:block;
   }
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. Atualize `helloworld.component.spec.ts` com a seguinte base de teste:

   ```js
   import { async, ComponentFixture, TestBed } from '@angular/core/testing';
   
   import { HelloWorldComponent } from './helloworld.component';
   
       describe('HelloWorld', () => {
       let component: HelloWorldComponent;
       let fixture: ComponentFixture<HelloWorldComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           declarations: [ HelloWorldComponent ]
           })
           .compileComponents();
       }));
   
       beforeEach(() => {
           fixture = TestBed.createComponent(HelloWorldComponent);
           component = fixture.componentInstance;
           fixture.detectChanges();
       });
   
       it('should create', () => {
           expect(component).toBeTruthy();
       });
   });
   ```

1. Próxima atualização `src/components/mapping.ts` para incluir o `HelloWorldComponent`. Adicione um `HelloWorldEditConfig` que marcará o espaço reservado no editor de AEM antes que o componente seja configurado. Por fim, adicione uma linha para mapear o componente AEM para o componente Angular com o auxiliar `MapTo`.

   ```js
   // src/components/mapping.ts
   
   import { HelloWorldComponent } from "./helloworld/helloworld.component";
   
   ...
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   
   MapTo('we-retail-journal/components/helloworld')(HelloWorldComponent, HelloWorldEditConfig);
   ```

   O código completo para [**mapping.ts** pode ser encontrado aqui.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/mapping.ts)

1. Atualize `src/app.module.ts` para atualizar o **NgModule**. Adicione o **`HelloWorldComponent`** como uma **declaração** que pertence ao **AppModule**. Adicione também o `HelloWorldComponent` como um **entryComponent** para que ele seja compilado e incluído dinamicamente no aplicativo conforme o modelo JSON é processado.

   ```js
   import { HelloWorldComponent } from './components/helloworld/helloworld.component';
   
   ...
   
   @NgModule({
     imports: [BrowserModule.withServerTransition({ appId: 'we-retail-sample-angular' }),
       SpaAngularEditableComponentsModule,
     AngularWeatherWidgetModule.forRoot({
       key: "37375c33ca925949d7ba331e52da661a",
       name: WeatherApiName.OPEN_WEATHER_MAP,
       baseUrl: 'http://api.openweathermap.org/data/2.5'
     }),
       AppRoutingModule,
       BrowserTransferStateModule],
     providers: [ModelManagerService,
       { provide: APP_BASE_HREF, useValue: '/' }],
     declarations: [AppComponent,
       TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MenuComponent,
       MainContentComponent,
       HelloWorldComponent],
     entryComponents: [TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MainContentComponent,
       HelloWorldComponent],
     bootstrap: [AppComponent]
    })
   ```

   O código concluído para [**app.module.ts** pode ser encontrado aqui.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/app.module.ts)

1. Implante o código para AEM usando o Maven:

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. Em [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js) abra `/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js`. Execute uma pesquisa rápida para **HelloWorld** em `main.js` para verificar se o componente Angular foi incluído.

   >[!NOTE]
   >
   > **main.** jsis o aplicativo Angular fornecido. O código não é mais legível por humanos. O comando npm run build acionou uma build otimizada que gera JavaScript compilado que pode ser interpretado pelos navegadores modernos.

## Atualização do modelo {#template-update}

1. Navegue até o Modelo editável para as versões React e/ou Angular:

   * (Angular) [http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html)
   * (React) [http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html)

1. Selecione o [!UICONTROL Contêiner de layout] principal e selecione o ícone [!UICONTROL Política] para abrir sua política:

   ![Selecionar política de layout](assets/spa-editor-helloworld-tutorial-use/select-page-policy.png)

   Em **[!UICONTROL Propriedades]** > **[!UICONTROL Componentes permitidos]**, faça uma pesquisa por **[!DNL Custom Components]**. Você deve ver o componente **[!DNL Hello World]**, selecione-o. Salve as alterações clicando na caixa de seleção no canto superior direito.

   ![Configuração da política do contêiner de layout](assets/spa-editor-helloworld-tutorial-use/layoutcontainer-update.png)

1. Depois de salvar, você deve ver o componente **[!DNL HelloWorld]** como um componente permitido no [!UICONTROL Contêiner de layout].

   ![Componentes permitidos atualizados](assets/spa-editor-helloworld-tutorial-use/allowed-components.png)

   >[!NOTE]
   >
   > Somente o AEM 6.5 e o AEM 6.4.5 são compatíveis com o recurso Modelo editável do Editor de SPA. Se estiver usando o AEM 6.4, será necessário configurar manualmente a política para Componentes permitidos via CRXDE Lite: `/conf/we-retail-journal/react/settings/wcm/policies/wcm/foundation/components/responsivegrid/default` ou `/conf/we-retail-journal/angular/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`

   CRXDE Lite mostrando as configurações de política atualizadas para [!UICONTROL Componentes permitidos] no [!UICONTROL Contêiner de layout]:

   ![CRXDE Lite mostrando as configurações de política atualizadas para Componentes permitidos no Contêiner de layout](assets/spa-editor-helloworld-tutorial-use/editable-template-policy.png)

## Colocando tudo junto {#putting-together}

1. Navegue até as páginas Angular ou React :

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

1. Encontre o componente **[!DNL Hello World]** e arraste e solte o componente **[!DNL Hello World]** na página.

   ![hello world arrastar e soltar](assets/spa-editor-helloworld-tutorial-use/fig7.png)

   O espaço reservado deve aparecer.

   ![Olá, empresário mundial](assets/spa-editor-helloworld-tutorial-use/fig10.png)

1. Selecione o componente e adicione uma mensagem na caixa de diálogo, ou seja, &quot;Mundo&quot; ou &quot;Seu nome&quot;. Salve as alterações.

   ![componente renderizado](assets/spa-editor-helloworld-tutorial-use/fig11.png)

   Observe que a string &quot;Hello&quot; sempre está anexada à mensagem. Isso é resultado da lógica no `HelloWorld.java` [!DNL Sling Model].

## Próximas etapas {#next-steps}

[Solução concluída para o componente HelloWorld](assets/spa-editor-helloworld-tutorial-use/aem-sample-we-retail-journal-HelloWorldSolution.zip)

* Código fonte completo para [[!DNL We.Retail Journal] no GitHub](https://github.com/adobe/aem-sample-we-retail-journal)
* Veja um tutorial mais detalhado sobre como desenvolver o React com [[!DNL Getting Started with the AEM SPA Editor - WKND Tutorial]](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html)

## Resolução de problemas {#troubleshooting}

### Não é possível criar o projeto no Eclipse {#unable-to-build-project-in-eclipse}

**Erro:** um erro ao importar o  [!DNL We.Retail Journal] projeto para o Eclipse para execuções de meta não reconhecidas:

`Execution npm install, Execution npm run build, Execution default-analyze-classes*`

![assistente de erro de eclipse](assets/spa-editor-helloworld-tutorial-use/fig9.png)

**Resolução**: Clique em Finish para resolvê-los mais tarde. Isso não deve impedir a conclusão do tutorial.

**Erro**: O módulo React  `react-app`, não é criado com êxito durante uma build Maven.

**Solução:** Tente excluir a  `node_modules` pasta abaixo do aplicativo de  **reação**. Execute novamente o comando Apache Maven `mvn  clean install -PautoInstallSinglePackage` a partir da raiz do projeto.

### Dependências não satisfeitas em AEM {#unsatisfied-dependencies-in-aem}

![Erro de dependência do gerenciador de pacotes](assets/spa-editor-helloworld-tutorial-use/we-retail-journal-package-dependency.png)

Se uma dependência de AEM não for atendida, no **[!UICONTROL AEM Gerenciador de Pacotes]** ou no **[!UICONTROL AEM Web Console]** (Felix Console), isso indica que SPA Recurso do Editor não está disponível.

### O componente não é exibido

**Erro**: Mesmo depois de uma implantação bem-sucedida e verificar se as versões compiladas dos aplicativos React/Angular têm o  `helloworld` componente atualizado, meu componente não é exibido quando eu o arrasto na página. Posso ver o componente na interface do usuário do AEM.

**Resolução**: Limpe o histórico/cache do seu navegador e/ou abra um novo navegador ou use o modo incógnito. Se isso não funcionar, invalide o cache da biblioteca do cliente na instância de AEM local. AEM tenta armazenar em cache bibliotecas de grandes clientes para que sejam eficientes. Às vezes, a invalidação manual do cache é necessária para corrigir problemas em que o código desatualizado é armazenado em cache.

Navegue até: [http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) e clique em Invalidar cache. Retorne à página Reagir/Angular e atualize a página.

![Recriar biblioteca do cliente](assets/spa-editor-helloworld-tutorial-use/invalidatecache.png)
