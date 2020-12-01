---
title: Introdução ao AEM Sites - Noções básicas sobre componentes
description: Entenda a tecnologia subjacente de um componente Adobe Experience Manager (AEM) Sites através de um exemplo simples "HelloWorld". Tópicos de HTL, Modelos Sling, bibliotecas do lado do cliente e caixas de diálogo do autor são explorados.
sub-product: sites
feature: components, sling-models, htl
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
translation-type: tm+mt
source-git-commit: 9825f6c82aac6d57477286f651da94f05a672ea8
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 2%

---


# Noções básicas sobre componentes {#component-basics}

Neste capítulo, exploraremos a tecnologia subjacente de um componente Adobe Experience Manager (AEM) Sites por meio de um exemplo simples `HelloWorld`. Pequenas modificações serão feitas em um componente existente, abrangendo tópicos de criação, HTL, Modelos Sling e bibliotecas do lado do cliente.

## Pré-requisitos {#prerequisites}

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

## Objetivo {#objective}

1. Saiba mais sobre a função dos modelos HTL e Sling Models para renderizar dinamicamente o HTML.
1. Entenda como as caixas de diálogo são usadas para facilitar a criação de conteúdo.
1. Saiba mais sobre as noções básicas das bibliotecas do lado do cliente para incluir CSS e JavaScript para suportar um componente.

## O que você vai criar {#what-you-will-build}

Neste capítulo, você fará várias modificações em um componente `HelloWorld` muito simples. No processo de fazer atualizações no componente `HelloWorld`, você aprenderá sobre as principais áreas AEM desenvolvimento de componentes.

## Projeto inicial do capítulo {#starter-project}

Este capítulo baseia-se em um projeto genérico gerado pelo [AEM Project Archetype](https://github.com/adobe/aem-project-archetype). Assista ao vídeo abaixo e reveja os [pré-requisitos](#prerequisites) para começar!

>[!VIDEO](https://video.tv.adobe.com/v/30154/?quality=12&learn=on)

Abra um novo terminal de linha de comando e execute as seguintes ações.

1. Em um diretório vazio, clone o repositório [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd):

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   Cloning into 'aem-guides-wknd'...
   ```

   >[!NOTE]
   >
   > Como opção, você pode baixar a ramificação [`component-basics/start`](https://github.com/adobe/aem-guides-wknd/archive/component-basics/start.zip) diretamente.

1. Navegue até o diretório `aem-guides-wknd`:

   ```shell
   $ cd aem-guides-wknd
   ```

1. Mude para a ramificação `component-basics/start`:

   ```shell
   $ git checkout component-basics/start
   Branch component-basics/start set up to track remote branch component-basics/start from origin.
   Switched to a new branch 'component-basics/start'
   ```

1. Crie e implante o projeto em uma instância local do AEM com o seguinte comando:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

1. Importe o projeto para seu IDE preferido seguindo as instruções para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

## Criação de componentes {#component-authoring}

Os componentes podem ser considerados pequenos blocos componentes modulares de uma página da Web. Para reutilizar componentes, eles devem ser configuráveis. Isso é feito por meio da caixa de diálogo do autor. Em seguida, criaremos um componente simples e verificaremos como os valores da caixa de diálogo são persistentes em AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30176/?quality=12&learn=on)

Abaixo estão as etapas de alto nível executadas no vídeo acima.

1. Crie uma nova página chamada **Informações básicas sobre componentes** abaixo de **Site WKND** `>` **US** `>` **en**.
1. Adicione o **Componente do mundo** à página recém-criada.
1. Abra a caixa de diálogo do componente e insira algum texto. Salve as alterações para ver a mensagem exibida na página.
1. Alterne para o modo desenvolvedor e visualização o Caminho do conteúdo no CRXDE-Lite e inspecione as propriedades da instância do componente.
1. Use CRXDE-Lite para visualização dos scripts `cq:dialog` e `helloworld.html` localizados em `/apps/wknd/components/content/helloworld`.

## Linguagem de Modelo HTML (HTL) {#htl-templates}

A Linguagem de modelo HTML ou [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html) é uma linguagem de modelagem do lado do servidor e de peso leve usada pelos componentes AEM para renderizar o conteúdo.

Em seguida, atualizaremos o script HTL `HelloWorld` para exibir uma saudação adicional antes da mensagem de texto.

>[!VIDEO](https://video.tv.adobe.com/v/30177/?quality=12&learn=on)

Abaixo estão as etapas de alto nível executadas no vídeo acima.

1. Mude para o IDE Eclipse e abra o projeto no módulo `ui.apps`.
1. Abra o arquivo `.content.xml` que define a caixa de diálogo para o componente `HelloWorld` em:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/_cq_dialog/.content.xml
   ```

1. Atualize a caixa de diálogo para adicionar um campo de texto adicional chamado **Saudação** com um nome de `./greeting`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Properties"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns">
           <items jcr:primaryType="nt:unstructured">
               <column
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/container">
                   <items jcr:primaryType="nt:unstructured">
                       <greeting
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Greeting"
                           name="./greeting"/>
                       <text
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Text"
                           name="./text"/>
                   </items>
               </column>
           </items>
       </content>
   </jcr:root>
   ```

1. Abra o arquivo `helloworld.html`, que representa o script HTL principal responsável pela renderização do componente `HelloWorld`, localizado em:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html
   ```

1. Atualize `helloworld.html` para renderizar o valor do campo de texto **Saudação** como parte de uma tag `H1`:

   ```html
   <h1 data-sly-test="${properties.text && properties.greeting}">${properties.greeting} ${properties.text}</h1>
   <pre data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
   HelloWorldModel says:
   ${hello.message}
   </pre>
   ```

1. Implante as alterações em uma instância local do AEM usando o plug-in do Eclipse Developer ou suas habilidades Maven.

## Modelos Sling {#sling-models}

Os modelos Sling são Java &quot;POJO&#39;s&quot; (objetos Java simples) orientados por anotações que facilitam o mapeamento de dados do JCR para variáveis Java e fornecem várias outras opções ao se desenvolver no contexto de AEM.

Em seguida, faremos algumas atualizações no `HelloWorldModel` Sling Model para aplicar alguma lógica comercial aos valores armazenados no JCR antes de colocá-los na página.

>[!VIDEO](https://video.tv.adobe.com/v/30189/?quality=12&learn=on)

1. Abra o arquivo `HelloWorldModel.java`, que é o Modelo Sling usado com o componente `HelloWorld`.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Adicione as seguintes linhas à classe `HelloWorldModel` para mapear os valores das propriedades JCR do componente `greeting` e `text` para variáveis Java:

   ```java
   ...
   @Model(adaptables = Resource.class)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String greeting;
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String text;
   
           @PostConstruct
           protected void init() {
               ...
   ```

1. Adicione o seguinte método `getGreeting()` à classe `HelloWorldModel`, que retorna o valor da propriedade chamada `greeting`. Este método adiciona a lógica adicional para retornar um valor String de &quot;Hello&quot; se a propriedade `greeting` for nula ou estiver em branco:

   ```java
   /***
   *
   * @return the value of greeting, if null or blank returns "Hello"
   */
   public String getGreeting() {
       return StringUtils.isNotBlank(this.greeting) ? this.greeting : "Hello";
   }
   ```

1. Adicione o seguinte método `getTextUpperCase()` à classe `HelloWorldModel`, que retorna o valor da propriedade chamada `text`. Esse método transforma a String em todos os caracteres de TopCase.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getTextUpperCase() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Atualize o arquivo `helloworld.html` em `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` para usar os métodos recém-criados do modelo `HelloWorld`:

   ```html
   <div class="cmp-helloworld" data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 data-sly-test="${hello.textUpperCase}">${hello.greeting} ${hello.textUpperCase}</h1>
       <pre>
       HelloWorldModel says:
       ${hello.message}
       </pre>
   </div>
   ```

1. Implante as alterações em uma instância local do AEM usando o plug-in do Eclipse Developer ou suas habilidades Maven.

## Bibliotecas do lado do cliente {#client-side-libraries}

Bibliotecas do lado do cliente, clientlibs for short, fornecem um mecanismo para organizar e gerenciar arquivos CSS e JavaScript necessários para uma implementação do AEM Sites. As bibliotecas do cliente são a maneira padrão de incluir CSS e JavaScript em uma página no AEM.

Em seguida, incluiremos alguns estilos CSS para o componente `HelloWorld` para entender as noções básicas das bibliotecas do cliente.

>[!VIDEO](https://video.tv.adobe.com/v/30190/?quality=12&learn=on)

Abaixo estão as etapas de alto nível executadas no vídeo acima.

1. em `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld`, crie um novo nó chamado `clientlibs` com um tipo de nó de `cq:ClientLibraryFolder`.
1. Crie uma pasta e uma estrutura de arquivos como a seguinte abaixo `clientlibs`

   ```plain
   /helloworld
           /clientlibs
               /css/helloworld.css
               /js/helloworld.js
               +js.txt
               +css.txt
   ```

1. Preencha `helloworld/clientlibs/css/helloworld.css` com o seguinte:

   ```css
   .cmp-helloworld h1 {
       color: red;
   }
   ```

1. Preencha `helloworld/clientlibs/css.txt` com o seguinte:

   ```plain
   #base=css
   helloworld.css
   ```

1. Preencha `helloworld/clientlibs/js/helloworld.js` com o seguinte:

   ```js
   console.log("hello world!");
   ```

1. Preencha `helloworld/clientlibs/js.txt` com o seguinte:

   ```plain
   #base=js
   helloworld.js
   ```

1. Atualize as propriedades do nó `clientlibs` para incluir as duas propriedades a seguir:

   | Nome | Tipo | Valor |
   |------|------|-------|
   | categorias | Sequência de caracteres | wknd.base |
   | allowProxy | Booleano | verdadeiro |

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root
       xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       categories="wknd.base"
       allowProxy="{Boolean}true"/>
   ```

1. Implante as alterações em uma instância local do AEM usando o plug-in do Eclipse Developer ou suas habilidades Maven.

## Parabéns! {#congratulations}

Parabéns, você acabou de aprender as noções básicas do desenvolvimento de componentes no Adobe Experience Manager!

### Próximas etapas {#next-steps}

Familiarize-se com as páginas e modelos do Adobe Experience Manager no próximo capítulo [Páginas e Modelos](pages-templates.md). Entenda como os Componentes principais são enviados em proxy no projeto e saiba mais sobre as configurações avançadas de política de modelos editáveis para criar um modelo bem estruturado de Página de artigo.

Visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente no bloco Git `component-basics/solution`.

1. Clique no repositório [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Confira a ramificação `component-basics/solution`
