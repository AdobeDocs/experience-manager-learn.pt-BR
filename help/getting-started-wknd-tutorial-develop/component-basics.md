---
title: Introdução ao AEM Sites - Noções básicas sobre componentes
description: Entenda a tecnologia subjacente de um Componente de sites do Adobe Experience Manager (AEM) por meio de um exemplo simples de "HelloWorld". Tópicos de HTL, Modelos do Sling, bibliotecas do lado do cliente e caixas de diálogo do autor são explorados.
sub-product: sites
feature: Componentes principais, Ferramentas do desenvolvedor
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
topic: Gerenciamento de conteúdo, desenvolvimento
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1154'
ht-degree: 1%

---


# Noções básicas sobre componentes {#component-basics}

Neste capítulo, exploraremos a tecnologia subjacente de um Componente de sites do Adobe Experience Manager (AEM) por meio de um exemplo simples `HelloWorld`. Pequenas modificações serão feitas em um componente existente, abrangendo tópicos de criação, HTL, Modelos do Sling, bibliotecas do lado do cliente.

## Pré-requisitos {#prerequisites}

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

O IDE usado nos vídeos é [Visual Studio Code](https://code.visualstudio.com/) e o plug-in [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync).

## Objetivo {#objective}

1. Saiba mais sobre a função dos modelos HTL e Modelos do Sling para renderizar dinamicamente o HTML.
1. Entenda como as caixas de diálogo são usadas para facilitar a criação de conteúdo.
1. Saiba mais sobre as noções básicas das bibliotecas do lado do cliente para incluir CSS e JavaScript para suportar um componente.

## O que você vai criar {#what-you-will-build}

Neste capítulo, você executará várias modificações em um componente `HelloWorld` muito simples. No processo de fazer atualizações no componente `HelloWorld`, você aprenderá sobre as principais áreas do desenvolvimento de componentes do AEM.

## Projeto inicial do capítulo {#starter-project}

Este capítulo baseia-se em um projeto genérico gerado pelo [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype). Assista ao vídeo abaixo e revise os [pré-requisitos](#prerequisites) para começar!

>[!NOTE]
>
> Se você concluiu o capítulo anterior com êxito, é possível reutilizar o projeto e ignorar as etapas para verificar o projeto inicial.

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
   > Se estiver usando o AEM 6.5 ou 6.4, anexe o perfil `classic` a qualquer comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Importe o projeto no IDE preferido seguindo as instruções para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

## Criação de componentes {#component-authoring}

Os componentes podem ser considerados como pequenos blocos componentes modulares de uma página da Web. Para reutilizar componentes, eles devem ser configuráveis. Isso é feito por meio da caixa de diálogo do autor. Em seguida, criaremos um componente simples e verificaremos como os valores da caixa de diálogo são mantidos no AEM.

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

Abaixo estão as etapas de alto nível executadas no vídeo acima.

1. Crie uma nova página chamada **Noções básicas do componente** abaixo de **Site WKND** `>` **US** `>` **en**.
1. Adicione o **Hello World Component** à página recém-criada.
1. Abra a caixa de diálogo do componente e insira algum texto. Salve as alterações para ver a mensagem exibida na página.
1. Alterne para o modo desenvolvedor e exiba o Caminho do conteúdo no CRXDE-Lite e inspecione as propriedades da instância do componente.
1. Use o CRXDE-Lite para exibir o script `cq:dialog` e `helloworld.html` localizado em `/apps/wknd/components/content/helloworld`.

## HTL (Linguagem de modelo HTML) e caixas de diálogo {#htl-dialogs}

Linguagem de modelo HTML ou **[HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)** é uma linguagem de modelo leve do lado do servidor usada pelos componentes do AEM para renderizar o conteúdo.

**** As caixas de diálogo definem as configurações disponíveis que podem ser feitas para um componente.

Em seguida, atualizaremos o script HTL `HelloWorld` para exibir uma saudação adicional antes da mensagem de texto.

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

Abaixo estão as etapas de alto nível executadas no vídeo acima.

1. Alterne para o IDE e abra o projeto para o módulo `ui.apps`.
1. Abra o arquivo `helloworld.html` e faça uma alteração na marcação HTML.
1. Use as ferramentas do IDE como [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) para sincronizar a alteração de arquivo com a instância do AEM local.
1. Retorne ao navegador e observe que a renderização do componente foi alterada.
1. Abra o arquivo `.content.xml` que define a caixa de diálogo do componente `HelloWorld` em:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Atualize a caixa de diálogo para adicionar um campo de texto adicional chamado **Title** com o nome `./title`:

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

1. Implante as alterações em uma instância local do AEM usando o plug-in do desenvolvedor ou usando suas habilidades Maven.

## Modelos sling {#sling-models}

Os Modelos do Sling são objetos Java &quot;POJO&quot; (Plain Old Java Objects) orientados por anotações que facilitam o mapeamento de dados do JCR para variáveis Java e fornecem várias outras sutilezas ao serem desenvolvidas no contexto do AEM.

Em seguida, faremos algumas atualizações no `HelloWorldModel` Modelo do Sling para aplicar alguma lógica de negócios aos valores armazenados no JCR antes de exibi-los na página.

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. Abra o arquivo `HelloWorldModel.java`, que é o Modelo do Sling usado com o componente `HelloWorld`.

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

1. Adicione as seguintes linhas à classe `HelloWorldModel` para mapear os valores das propriedades JCR do componente `title` e `text` para as variáveis Java:

   ```java
   ...
   @Model(adaptables = Resource.class,
   defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue
       private String title;
   
       @ValueMapValue
       private String text;
   
       @PostConstruct
       protected void init() {
           ...
   ```

1. Adicione o seguinte método `getTitle()` à classe `HelloWorldModel`, que retorna o valor da propriedade chamada `title`. Esse método adiciona a lógica adicional para retornar um valor de String de &quot;Valor padrão aqui!&quot; se a propriedade `title` for nula ou estiver em branco:

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. Adicione o seguinte método `getText()` à classe `HelloWorldModel`, que retorna o valor da propriedade chamada `text`. Esse método transforma a String em todos os caracteres em maiúsculas.

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
   > Se estiver usando o AEM 6.4/6.5, use `mvn clean install -PautoInstallBundle -Pclassic`

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

1. Implante as alterações em uma instância local do AEM usando o plug-in do desenvolvedor do Eclipse ou usando suas habilidades Maven.

## Bibliotecas do lado do cliente {#client-side-libraries}

Bibliotecas do lado do cliente, clientlibs, abreviando, fornecem um mecanismo para organizar e gerenciar arquivos CSS e JavaScript necessários para uma implementação do AEM Sites. As bibliotecas do lado do cliente são a maneira padrão de incluir CSS e JavaScript em uma página no AEM.

Em seguida, incluiremos alguns estilos de CSS para o componente `HelloWorld` a fim de entender as noções básicas das bibliotecas do lado do cliente.

>[!VIDEO](https://video.tv.adobe.com/v/330989/?quality=12&learn=on)

Abaixo estão as etapas de alto nível executadas no vídeo acima.

1. abaixo de `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs` crie uma nova pasta chamada `clientlib-helloworld`.
1. Crie uma pasta e uma estrutura de arquivo como a seguinte abaixo `clientlibs`

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

1. Implante as alterações em uma instância local do AEM usando o plug-in do desenvolvedor ou usando suas habilidades Maven.

   >[!NOTE]
   >
   > Por motivos de desempenho, o CSS e o JavaScript são frequentemente armazenados em cache pelo navegador. Se você não vir imediatamente a alteração da biblioteca do cliente, execute uma atualização rígida e limpe o cache do navegador. Pode ser útil usar uma janela incógnita para garantir um novo cache.

## Parabéns! {#congratulations}

Parabéns, você acabou de aprender as noções básicas do desenvolvimento de componentes no Adobe Experience Manager!

### Próximas etapas {#next-steps}

Familiarize-se com as páginas e modelos do Adobe Experience Manager no próximo capítulo [Páginas e Modelos](pages-templates.md). Entenda como os Componentes principais são transferidos por proxy para o projeto e aprenda as configurações de política avançadas dos modelos editáveis para criar um modelo de página de artigo bem estruturado.

Visualize o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente na ramificação Git `tutorial/component-basics-solution`.

