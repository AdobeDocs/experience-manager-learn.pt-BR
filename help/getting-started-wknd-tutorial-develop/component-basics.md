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
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '1066'
ht-degree: 1%

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

>[!NOTE]
>
> Se você tiver concluído com êxito o capítulo anterior, poderá reutilizar o projeto e ignorar as etapas para fazer check-out do projeto inicial.

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

Abra um novo terminal de linha de comando e execute as seguintes ações.

1. Em um diretório vazio, clone o repositório [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd):

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > Opcionalmente, você pode continuar usando o projeto gerado no capítulo anterior, [Configuração do projeto](./project-setup.md).

1. Navegue até a pasta `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Crie e implante o projeto em uma instância local do AEM com o seguinte comando:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se estiver usando AEM 6.5 ou 6.4, anexe o perfil `classic` a qualquer comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Importe o projeto para seu IDE preferido seguindo as instruções para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

## Criação de componentes {#component-authoring}

Os componentes podem ser considerados pequenos blocos componentes modulares de uma página da Web. Para reutilizar componentes, eles devem ser configuráveis. Isso é feito por meio da caixa de diálogo do autor. Em seguida, criaremos um componente simples e verificaremos como os valores da caixa de diálogo são persistentes em AEM.

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

Abaixo estão as etapas de alto nível executadas no vídeo acima.

1. Crie uma nova página chamada **Informações básicas sobre componentes** abaixo de **Site WKND** `>` **US** `>` **en**.
1. Adicione o **Componente do mundo** à página recém-criada.
1. Abra a caixa de diálogo do componente e insira algum texto. Salve as alterações para ver a mensagem exibida na página.
1. Alterne para o modo desenvolvedor e visualização o Caminho do conteúdo no CRXDE-Lite e inspecione as propriedades da instância do componente.
1. Use CRXDE-Lite para visualização dos scripts `cq:dialog` e `helloworld.html` localizados em `/apps/wknd/components/content/helloworld`.

## HTL (Linguagem de Modelo HTML) e Diálogos {#htl-dialogs}

A Linguagem de modelo HTML ou **[HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)** é uma linguagem de modelo do lado do servidor e de peso leve usada pelos componentes AEM para renderizar o conteúdo.

**As** caixas de diálogo definem as configurações disponíveis que podem ser feitas para um componente.

Em seguida, atualizaremos o script HTL `HelloWorld` para exibir uma saudação adicional antes da mensagem de texto.

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

Abaixo estão as etapas de alto nível executadas no vídeo acima.

1. Alterne para o IDE e abra o projeto no módulo `ui.apps`.
1. Abra o arquivo `helloworld.html` e faça uma alteração na marcação HTML.
1. Use as ferramentas IDE para sincronizar a alteração de arquivo com a instância AEM local.
1. Retorne ao navegador e observe que a renderização do componente foi alterada.
1. Abra o arquivo `.content.xml` que define a caixa de diálogo para o componente `HelloWorld` em:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Atualize a caixa de diálogo para adicionar um campo de texto adicional chamado **Title** com um nome de `./title`:

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
                       <title
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Title"
                           name="./title"/>
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

1. Reabra o arquivo `helloworld.html`, que representa o script HTL principal responsável pela renderização do componente `HelloWorld`, localizado em:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. Atualize `helloworld.html` para renderizar o valor do campo de texto **Saudação** como parte de uma tag `H1`:

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. Implante as alterações em uma instância local do AEM usando o plug-in do desenvolvedor ou suas habilidades Maven.

## Modelos Sling {#sling-models}

Os modelos Sling são Java &quot;POJO&#39;s&quot; (objetos Java simples) orientados por anotações que facilitam o mapeamento de dados do JCR para variáveis Java e fornecem várias outras opções ao se desenvolver no contexto de AEM.

Em seguida, faremos algumas atualizações no `HelloWorldModel` Sling Model para aplicar alguma lógica comercial aos valores armazenados no JCR antes de colocá-los na página.

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. Abra o arquivo `HelloWorldModel.java`, que é o Modelo Sling usado com o componente `HelloWorld`.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Adicione as seguintes declarações de importação:

   ```java
   import org.apache.commons.lang.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. Atualize a anotação `@Model` para usar um `DefaultInjectionStrategy`:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. Adicione as seguintes linhas à classe `HelloWorldModel` para mapear os valores das propriedades JCR do componente `title` e `text` para variáveis Java:

   ```java
   ...
   @Model(adaptables = Resource.class,
   defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue
       protected String title;
   
       @ValueMapValue
       protected String text;
   
           @PostConstruct
           protected void init() {
               ...
   ```

1. Adicione o seguinte método `getTitle()` à classe `HelloWorldModel`, que retorna o valor da propriedade chamada `title`. Este método adiciona a lógica adicional para retornar um valor String de &quot;Valor padrão aqui!&quot; se a propriedade `title` for nula ou estiver em branco:

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. Adicione o seguinte método `getText()` à classe `HelloWorldModel`, que retorna o valor da propriedade chamada `text`. Esse método transforma a string em todos os caracteres em maiúsculas.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Crie e implante o pacote a partir do módulo `core`:

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > Se estiver usando AEM 6.4/6.5, use `mvn clean install -PautoInstallBundle -Pclassic`

1. Atualize o arquivo `helloworld.html` em `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` para usar os métodos recém-criados do modelo `HelloWorld`:

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld"
   data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 class="cmp-helloworld__title">${model.title}</h1>
       <div class="cmp-helloworld__item" data-sly-test="${properties.text}">
           <p class="cmp-helloworld__item-label">Text property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.text}</pre>
       </div>
       <div class="cmp-helloworld__item" data-sly-test="${model.text}">
           <p class="cmp-helloworld__item-label">Sling Model getText() property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${model.text}</pre>
       </div>
       <div class="cmp-helloworld__item"  data-sly-test="${model.message}">
           <p class="cmp-helloworld__item-label">Model message:</p>
           <pre class="cmp-helloworld__item-output"data-cmp-hook-helloworld="model">${model.message}</pre>
       </div>
   </div>
   ```

1. Implante as alterações em uma instância local do AEM usando o plug-in do Eclipse Developer ou suas habilidades Maven.

## Bibliotecas do lado do cliente {#client-side-libraries}

Bibliotecas do lado do cliente, clientlibs for short, fornecem um mecanismo para organizar e gerenciar arquivos CSS e JavaScript necessários para uma implementação do AEM Sites. As bibliotecas do cliente são a maneira padrão de incluir CSS e JavaScript em uma página no AEM.

Em seguida, incluiremos alguns estilos CSS para o componente `HelloWorld` para entender as noções básicas das bibliotecas do cliente.

>[!VIDEO](https://video.tv.adobe.com/v/330989/?quality=12&learn=on)

Abaixo estão as etapas de alto nível executadas no vídeo acima.

1. em `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs`, crie uma nova pasta chamada `clientlib-helloworld`.
1. Crie uma pasta e uma estrutura de arquivos como a seguinte abaixo `clientlibs`

   ```plain
   /clientlib-helloworld
       /css/helloworld.css
       /js/helloworld.js
       +js.txt
       +css.txt
       +.content.xml
   ```

1. Preencha `helloworld.css` com o seguinte:

   ```css
   .cmp-helloworld .cmp-helloworld__title {
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
   console.log("Hello World from Javascript!");
   ```

1. Preencha `helloworld/clientlibs/js.txt` com o seguinte:

   ```plain
   #base=js
   helloworld.js
   ```

1. Atualize o arquivo `clientlib-helloworld/.conten.xml` para incluir as seguintes propriedades:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.helloworld]" />
   ```

1. Atualize o arquivo `clientlibs/clientlib-base/.content.xml` para **embed** a categoria `wknd.helloworld`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.base]"
       embed="[core.wcm.components.accordion.v1,core.wcm.components.tabs.v1,core.wcm.components.carousel.v1,core.wcm.components.image.v2,core.wcm.components.breadcrumb.v2,core.wcm.components.search.v1,core.wcm.components.form.text.v2,core.wcm.components.pdfviewer.v1,core.wcm.components.commons.datalayer.v1,wknd.grid,wknd.helloworld]"/>
   ```

1. Implante as alterações em uma instância local do AEM usando o plug-in do desenvolvedor ou suas habilidades Maven.

## Parabéns! {#congratulations}

Parabéns, você acabou de aprender as noções básicas do desenvolvimento de componentes no Adobe Experience Manager!

### Próximas etapas {#next-steps}

Familiarize-se com as páginas e modelos do Adobe Experience Manager no próximo capítulo [Páginas e Modelos](pages-templates.md). Entenda como os Componentes principais são enviados em proxy no projeto e saiba mais sobre as configurações avançadas de política de modelos editáveis para criar um modelo bem estruturado de Página de artigo.

Visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente no ramo Git `tutorial/component-basics-solution`.

