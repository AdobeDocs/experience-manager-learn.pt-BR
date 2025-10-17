---
title: Componente personalizado
description: Aborda a criação de ponta a ponta de um componente de linha de identificação personalizado que exibe o conteúdo criado. Inclui o desenvolvimento de um modelo do Sling para encapsular a lógica de negócios a fim de preencher o componente de linha de identificação e o HTL correspondente para renderizar o componente.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, APIs
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4072
mini-toc-levels: 1
thumbnail: 30181.jpg
doc-type: Tutorial
exl-id: f54f3dc9-6ec6-4e55-9043-7a006840c905
duration: 1039
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '3883'
ht-degree: 100%

---

# Componente personalizado {#custom-component}

Este tutorial aborda a criação de ponta a ponta de um componente `Byline` personalizado do AEM que exibe o conteúdo criado em uma caixa de diálogo e apresenta o desenvolvimento de um modelo do Sling para encapsular a lógica de negócios a fim de preencher o HTL do componente.

## Pré-requisitos {#prerequisites}

Consulte as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Projeto inicial

>[!NOTE]
>
> Se você tiver concluído com sucesso o capítulo anterior, poderá reutilizar o projeto e ignorar as etapas para conferir o projeto inicial.

Confira o código de linha de base no qual o tutorial se baseia:

1. Confira a ramificação `tutorial/custom-component-start` do [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
   ```

1. Implante a base de código em uma instância do AEM local, usando as suas habilidades do Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se estiver usando o AEM 6.5 ou 6.4, anexe o perfil `classic` a todos os comandos do Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

É possível ver o código concluído no [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) ou conferir o código localmente a qualquer momento, alternando para a ramificação `tutorial/custom-component-solution`.

## Objetivo

1. Entender como criar um componente personalizado do AEM
1. Aprender a encapsular a lógica de negócios com modelos do Sling
1. Aprender a usar um modelo do Sling de dentro de um script HTL

## O que você criará {#what-build}

Nesta parte do tutorial do WKND, criamos um componente de linha de identificação usado para exibir informações criadas sobre o colaborador de um artigo.

![exemplo de componente de linha de identificação](assets/custom-component/byline-design.png)

*Componente de linha de identificação*

A implementação do componente de linha de identificação inclui uma caixa de diálogo que coleta o conteúdo da linha de identificação e um modelo do Sling personalizado que recupera detalhes como:

* Nome
* Imagem
* Profissões

## Criar componente de linha de identificação {#create-byline-component}

Primeiro, crie a estrutura do nó do componente de linha de identificação e defina uma caixa de diálogo. Representa o componente no AEM e define implicitamente o tipo de recurso do componente por sua localização no JCR.

A caixa de diálogo expõe a interface que os criadores de conteúdo podem fornecer. Para esta implementação, o componente de **Imagem** do componente principal de WCM do AEM é usado para tratar a criação e a renderização da imagem da linha de identificação; portanto, ele deve ser definido como `sling:resourceSuperType` deste componente.

### Criar definição do componente {#create-component-definition}

1. No módulo **ui.apps**, navegue até `/apps/wknd/components` e crie uma pasta chamada `byline`.
1. Dentro da pasta `byline`, adicione um arquivo chamado `.content.xml`.

   ![caixa de diálogo de criação de nó](assets/custom-component/byline-node-creation.png)

1. Preencha o arquivo `.content.xml` com o seguinte:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   O arquivo XML acima fornece a definição do componente, incluindo o título, a descrição e o grupo. `sling:resourceSuperType` aponta para `core/wcm/components/image/v2/image`, que é o [Componente de imagem principal](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/image.html?lang=pt-BR).

### Criar o script HTL {#create-the-htl-script}

1. Na pasta `byline`, adicione um arquivo `byline.html`, responsável pela apresentação do componente em HTML. É importante nomear o arquivo de forma idêntica à pasta, pois ele se torna o script padrão que o Sling usa para renderizar esse tipo de recurso.

1. Adicione o código a seguir ao `byline.html`.

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

O `byline.html` é [revisitado mais tarde](#byline-htl), depois que o modelo do Sling é criado. O estado atual do arquivo HTL permite que o componente seja exibido em um estado vazio no editor de páginas do AEM Sites quando ele é arrastado e solto na página.

### Criar a definição da caixa de diálogo {#create-the-dialog-definition}

Em seguida, defina uma caixa de diálogo para o componente de linha de identificação com os seguintes campos:

* **Nome**: um campo de texto que representa o nome do colaborador.
* **Imagem**: uma referência à imagem biográfica do colaborador.
* **Profissões**: uma lista de profissões atribuídas ao colaborador. As profissões devem ser classificadas alfabeticamente em ordem crescente (de A a Z).

1. Dentro da pasta `byline`, crie uma pasta chamada `_cq_dialog`.
1. Dentro do `byline/_cq_dialog`, adicione um arquivo chamado `.content.xml`. Essa é a definição XML da caixa de diálogo. Adicione o seguinte XML:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="nt:unstructured"
           jcr:title="Byline"
           sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured"
               sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="granite/ui/components/coral/foundation/tabs"
                       maximized="{Boolean}false">
                   <items jcr:primaryType="nt:unstructured">
                       <asset
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}false"/>
                       <metadata
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}true"/>
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
                                               <name
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                       emptyText="Enter the contributor's name to display."
                                                       fieldDescription="The contributor's name to display."
                                                       fieldLabel="Name"
                                                       name="./name"
                                                       required="{Boolean}true"/>
                                               <occupations
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/multifield"
                                                       fieldDescription="A list of the contributor's occupations."
                                                       fieldLabel="Occupations"
                                                       required="{Boolean}false">
                                                   <field
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           emptyText="Enter an occupation"
                                                           name="./occupations"/>
                                               </occupations>
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

   Estas definições de nó da caixa de diálogo usam o [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) para controlar quais guias da caixa de diálogo são herdadas do componente de `sling:resourceSuperType`, neste caso, o **Componente de imagem dos componentes principais**.

   ![caixa de diálogo concluída para a linha de identificação](assets/custom-component/byline-dialog-created.png)

### Criar a caixa de diálogo de política {#create-the-policy-dialog}

Seguindo a mesma abordagem da criação da caixa de diálogo, crie uma caixa de diálogo de política (anteriormente conhecida como caixa de diálogo de design) para ocultar campos indesejados na configuração de política herdada do componente de imagem dos componentes principais.

1. Dentro da pasta `byline`, crie uma pasta chamada `_cq_design_dialog`.
1. Dentro de `byline/_cq_design_dialog`, crie um arquivo chamado `.content.xml`. Atualize o arquivo com o XML a seguir. É mais fácil abrir o `.content.xml` e copiar/colar o XML abaixo nele.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Byline"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <decorative
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <altValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <titleValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <displayCaptionPopup
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <disableUuidTracking
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                   </items>
                               </content>
                           </items>
                       </properties>
                       <features
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <accordion
                                               jcr:primaryType="nt:unstructured">
                                           <items jcr:primaryType="nt:unstructured">
                                               <orientation
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                               <crop
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                           </items>
                                       </accordion>
                                   </items>
                               </content>
                           </items>
                       </features>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   A base do XML **da caixa de diálogo de política** anterior foi obtida do [Componente de imagem dos componentes principais](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   Como na configuração da caixa de diálogo, o [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) é usado para ocultar campos irrelevantes que, de outra forma, seriam herdados do `sling:resourceSuperType`, conforme visto pelas definições de nó com a propriedade `sling:hideResource="{Boolean}true"`.

### Implantar o código {#deploy-the-code}

1. Sincronize as alterações em `ui.apps` com o seu IDE ou usando as suas habilidades do Maven.

   ![Exportar para o componente de linha de identificação do servidor do AEM](assets/custom-component/export-byline-component-aem.png)

## Adicionar o componente a uma página {#add-the-component-to-a-page}

Para simplificar as coisas e manter o foco no desenvolvimento de componentes do AEM, vamos adicionar o componente de linha de identificação em seu estado atual a uma página de artigo para verificar se a definição do nó `cq:Component` está correta. E também para verificar se o AEM reconhece a nova definição de componente e se a caixa de diálogo do componente está funcionando para criação.

### Adicionar uma imagem ao AEM Assets

Primeiro, carregue uma amostra de foto de perfil no AEM Assets a ser usada para preencher a imagem no componente de linha de identificação.

1. Navegue até a pasta “LA Skateparks” no AEM Assets: [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. Carregue a foto de perfil **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)** na pasta.

   ![Foto de perfil carregada no AEM Assets](assets/custom-component/stacey-roswell-headshot-assets.png)

### Criar o componente {#author-the-component}

Em seguida, adicione o componente de linha de identificação a uma página no AEM. Como o componente de linha de identificação é adicionado ao **Projeto de sites da WKND: grupo de componentes de conteúdo**, por meio da definição `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml`, ele fica automaticamente disponível para qualquer **Container** cuja **Política** permita o **Projeto de sites da WKND: grupo de componentes de conteúdo**. Assim, ele está disponível no container de layout da página de artigo.

1. Navegue até o artigo “LA Skatepark” em: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. Na barra lateral esquerda, arraste e solte um **componente de linha de identificação** na **parte inferior** do container de layout da página de artigo aberta.

   ![adicionar componente de linha de identificação à página](assets/custom-component/add-to-page.png)

1. Verifique se a barra lateral esquerda está aberta **e visível, e se o** Localizador de ativos** está selecionado.

1. Selecione o espaço reservado do componente de **Linha de identificação**, que, por sua vez, exibe a barra de ação, e toque no ícone de **chave inglesa** para abrir a caixa de diálogo.

1. Com a caixa de diálogo aberta e a primeira guia (Ativo) ativa, abra a barra lateral esquerda e, no localizador de ativos, arraste uma imagem para a área de pouso de imagens. Procure por “stacey” para encontrar a imagem biográfica de Stacey Roswells fornecida no pacote ui.content da WKND.

   ![adicionar imagem à caixa de diálogo](assets/custom-component/add-image.png)

1. Depois de adicionar uma imagem, clique na guia **Propriedades** para inserir o **Nome** e as **Profissões**.

   Ao inserir profissões, insira-as em **ordem alfabética inversa**, para que a lógica de negócios de alfabetização implementada no modelo do Sling seja verificada.

   Toque no botão **Concluído**, na parte inferior direita, para salvar as alterações.

   ![preencher propriedades do componente de linha de identificação](assets/custom-component/add-properties.png)

   Os criadores do AEM configuram e criam componentes por meio das caixas de diálogo. Neste ponto, no desenvolvimento do componente de linha de identificação, as caixas de diálogo são incluídas para coletar os dados, mas a lógica para renderizar o conteúdo criado ainda não foi adicionada. Portanto, somente o espaço reservado é exibido.

1. Depois de salvar a caixa de diálogo, navegue até [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) e confira como o conteúdo do componente é armazenado no nó do conteúdo do componente de linha de identificação na página do AEM.

   Localize o nó de conteúdo do componente de linha de identificação abaixo da página “LA Skate Parks”, ou seja, `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`.

   Observe que os nomes das propriedades `name`, `occupations` e `fileReference` estão armazenados no **nó da linha de identificação**.

   Além disso, observe que o `sling:resourceType` do nó está definido como `wknd/components/content/byline`, que é o que vincula esse nó de conteúdo à implementação do componente de linha de identificação.

   ![propriedades da linha de identificação no CRXDE](assets/custom-component/byline-properties-crxde.png)

## Criar modelo do Sling de linha de identificação {#create-sling-model}

Em seguida, vamos criar um modelo do Sling que atuará como modelo de dados e hospedará a lógica de negócios do componente de linha de identificação.

Os modelos do Sling são POJOs (Plain Old Java™ Objects) de Java™ orientados por anotações que facilitam o mapeamento de dados do JCR para variáveis de Java™ e aumentam a eficiência ao desenvolver no contexto do AEM.

### Revisar dependências do Maven {#maven-dependency}

O modelo do Sling de linha de identificação depende de várias APIs de Java™ fornecidas pelo AEM. Essas APIs são disponibilizadas através do `dependencies` listado no arquivo POM do módulo `core`. O projeto usado neste tutorial foi criado para o AEM as a Cloud Service. No entanto, ele é diferente, pois é compatível com versões anteriores do AEM 6.5/6.4. Portanto, tanto a dependência do Cloud Service quanto a do AEM 6.x estão incluídas.

1. Abra o arquivo `pom.xml` abaixo de `<src>/aem-guides-wknd/core/pom.xml`.
1. Localizar a dependência de `aem-sdk-api`: **Somente AEM as a Cloud Service**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   A [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=pt-br) contém todas as APIs de Java™ públicas expostas pelo AEM. O `aem-sdk-api` é usado por padrão ao compilar este projeto. A versão é mantida no POM do reator primário da raiz do projeto em `aem-guides-wknd/pom.xml`.

1. Localizar a dependência de `uber-jar`: **Somente AEM 6.5/6.4**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   O `uber-jar` é incluído somente quando o perfil `classic` é chamado, ou seja, `mvn clean install -PautoInstallSinglePackage -Pclassic`. Novamente, isso é exclusivo deste projeto. Em um projeto real, gerado a partir do arquétipo de projeto do AEM, o `uber-jar` será o padrão, se a versão do AEM especificada for 6.5 ou 6.4.

   O [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html?lang=pt-BR#experience-manager-api-dependencies) contém todas as APIs de Java™ públicas expostas pelo AEM 6.x. A versão é mantida no POM do reator primário da raiz do projeto `aem-guides-wknd/pom.xml`.

1. Localizar a dependência de `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   Estas são as APIs de Java™ públicas completas expostas pelos componentes principais do AEM. Os componentes principais do AEM são um projeto mantido fora do AEM e, portanto, têm um ciclo de lançamento separado. Por esse motivo, é uma dependência que precisa ser incluída separadamente, **não** incluída com `uber-jar` ou `aem-sdk-api`.

   Assim como o uber-jar, a versão dessa dependência é mantida no arquivo POM do reator primário de `aem-guides-wknd/pom.xml`.

   Posteriormente neste tutorial, a classe “Imagem” do componente principal será usada para exibir a imagem no componente de linha de identificação. É necessário contar com a dependência do componente principal para criar e compilar o modelo do Sling.

### Interface da linha de identificação {#byline-interface}

Crie uma interface Java™ pública para a linha de identificação. O `Byline.java` define os métodos públicos necessários para direcionar o script HTL `byline.html`.

1. Lá dentro, o módulo `core` na pasta `core/src/main/java/com/adobe/aem/guides/wknd/core/models` cria um arquivo chamado `Byline.java`

   ![criar interface da linha de identificação](assets/custom-component/create-byline-interface.png)

1. Atualize `Byline.java` com os seguintes métodos:

   ```java
   package com.adobe.aem.guides.wknd.core.models;
   
   import java.util.List;
   
   /**
   * Represents the Byline AEM Component for the WKND Site project.
   **/
   public interface Byline {
       /***
       * @return a string to display as the name.
       */
       String getName();
   
       /***
       * Occupations are to be sorted alphabetically in a descending order.
       *
       * @return a list of occupations.
       */
       List<String> getOccupations();
   
       /***
       * @return a boolean if the component has enough content to display.
       */
       boolean isEmpty();
   }
   ```

   Os dois primeiros métodos expõem os valores de **nome** e **profissões** do componente de linha de identificação.

   O método `isEmpty()` é usado para determinar se o componente tem algum conteúdo para renderizar ou se está aguardando para ser configurado.

   Observe que não há um método para a imagem; [isso será revisado mais tarde](#tackling-the-image-problem).

1. Os pacotes de Java™ que contêm classes públicas de Java™, neste caso, um modelo do Sling, precisam contar com a versão criada por meio do arquivo `package-info.java` do pacote.

   Como o pacote de Java™ `com.adobe.aem.guides.wknd.core.models` da origem da WKND declara a versão de `1.0.0`, e uma interface pública e métodos não separáveis estão sendo adicionados, a versão precisa ser aumentada para `1.1.0`. Abra o arquivo em `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` e atualize `@Version("1.0.0")` para `@Version("2.1.0")`.

   ```
   @Version("2.1.0")
   package com.adobe.aem.guides.wknd.core.models;
   
   import org.osgi.annotation.versioning.Version;
   ```

Sempre que forem feitas alterações nos arquivos deste pacote, a [versão do pacote precisará ser ajustada semanticamente](https://semver.org/). Caso contrário, o [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd) do projeto do Maven detectará uma versão do pacote inválida e interromperá a compilação. Felizmente, em caso de falha, o plug-in do Maven relata a versão inválida do pacote de Java™ e a versão que deveria ter. Atualize a declaração `@Version("...")` no `package-info.java` do pacote de Java™ violador para a versão recomendada pelo plug-in para corrigir.

### Implementação da linha de identificação {#byline-implementation}

O `BylineImpl.java` é a implementação do modelo do Sling que implementa a interface `Byline.java` definida anteriormente. O código completo para `BylineImpl.java` pode ser encontrado na parte inferior desta seção.

1. Crie uma pasta chamada `impl` abaixo de `core/src/main/java/com/adobe/aem/guides/core/models`.
1. Na pasta `impl`, crie um arquivo `BylineImpl.java`.

   ![Arquivo de implementação da linha de identificação](assets/custom-component/byline-impl-file.png)

1. Abra `BylineImpl.java`. Especifique que ele implementa a interface `Byline`. Use os recursos de preenchimento automático do IDE ou atualize manualmente o arquivo para incluir os métodos necessários para implementar a interface `Byline`:

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   import java.util.List;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   
   public class BylineImpl implements Byline {
   
       @Override
       public String getName() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public List<String> getOccupations() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public boolean isEmpty() {
           // TODO Auto-generated method stub
           return false;
       }
   }
   ```

1. Adicione as anotações do modelo do Sling, atualizando `BylineImpl.java` com as anotações na camada das classes a seguir. Essa `@Model(..)`anotação é o que transforma a classe em um modelo do Sling.

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ...
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
       ...
   }
   ```

   Vamos revisar essa anotação e seus parâmetros:

   * A anotação `@Model` registra BylineImpl como um modelo do Sling quando implantada no AEM.
   * O parâmetro `adaptables` especifica que esse modelo pode ser adaptado pela solicitação.
   * O parâmetro `adapters` permite que a classe de implementação seja registrada na interface da linha de identificação. Isso permite que o script HTL chame o modelo do Sling pela interface (em vez da implementação diretamente). [Mais detalhes sobre os adaptadores podem ser encontrados aqui](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * O `resourceType` aponta para o tipo de recurso do componente de linha de identificação (criado anteriormente) e ajuda a resolver o modelo correto, caso haja várias implementações. [Mais detalhes sobre como associar uma classe de modelo a um tipo de recurso podem ser encontrados aqui](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130).

### Implementação dos métodos do modelo do Sling {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

O primeiro método implementado é `getName()`, que simplesmente retorna o valor armazenado ao nó de conteúdo JCR da linha de identificação na propriedade `name`.

Para isso, a anotação do modelo do Sling `@ValueMapValue` é usada para injetar o valor em um campo de Java™ usando o ValueMap do recurso da solicitação.


```java
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private String name;

    ...
    @Override
    public String getName() {
        return name;
    }
    ...
}
```

Como a propriedade JCR compartilha o nome como o campo de Java™ (ambos são “nome”), `@ValueMapValue` resolve automaticamente essa associação e injeta o valor da propriedade no campo de Java™.

#### getOccupations() {#implementing-get-occupations}

O próximo método a implementar é `getOccupations()`. Este método carrega as profissões armazenadas na propriedade JCR `occupations` e retorna uma coleção classificada (alfabeticamente) dessas profissões.

Usando a mesma técnica abordada em `getName()`, o valor da propriedade pode ser inserido no campo do modelo do Sling.

Quando os valores da propriedade JCR estiverem disponíveis no modelo do Sling por meio do campo de Java™ `occupations` inserido, a lógica de negócios da classificação poderá ser aplicada no método `getOccupations()`.


```java
import java.util.ArrayList;
import java.util.Collections;
  ...

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private List<String> occupations;
    ...
    @Override
    public List<String> getOccupations() {
        if (occupations != null) {
            Collections.sort(occupations);
            return new ArrayList<String>(occupations);
        } else {
            return Collections.emptyList();
        }
    }
    ...
}
  ...
```


#### isEmpty() {#implementing-is-empty}

O último método público é `isEmpty()`, que determina quando o componente deve ser considerado “criado o suficiente” para ser renderizado.

Para esse componente, o requisito comercial inclui todos os três campos; `name, image and occupations` deve ser preenchido *antes* que o componente possa ser renderizado.


```java
import org.apache.commons.lang3.StringUtils;
  ...
public class BylineImpl implements Byline {
    ...
    @Override
    public boolean isEmpty() {
        if (StringUtils.isBlank(name)) {
            // Name is missing, but required
            return true;
        } else if (occupations == null || occupations.isEmpty()) {
            // At least one occupation is required
            return true;
        } else if (/* image is not null, logic to be determined */) {
            // A valid image is required
            return true;
        } else {
            // Everything is populated, so this component is not considered empty
            return false;
        }
    }
    ...
}
```


#### Como lidar com o “problema da imagem” {#tackling-the-image-problem}

Verificar as condições de nome e profissão é trivial, e o Apache Commons Lang3 fornece a classe [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) útil. No entanto, não está claro como a **presença da imagem** pode ser validada, pois o componente de imagem dos componentes principais é usado para trazer a imagem à tona.

Há duas maneiras de contornar isso:

Verifique se a propriedade JCR `fileReference` está sendo resolvida como um ativo. *OU* Converta estse recurso em um modelo do Sling de imagem dos componentes principais e confirme que o método `getSrc()` não está vazio.

Vamos usar a **segunda** abordagem. A primeira abordagem provavelmente é suficiente, mas, neste tutorial, a última é usada para permitir explorar outros recursos dos modelos do Sling.

1. Crie um método privado que obtenha a imagem. Esse método é deixado como privado, porque não há necessidade de expor o objeto de imagem no próprio HTL, e ele é usado apenas para a unidade `isEmpty().`

   Adicione o seguinte método privado para `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   Como observado acima, há mais duas abordagens para obter o **modelo do Sling de imagem**:

   O primeiro usa a anotação `@Self` para adaptar automaticamente a solicitação atual ao `Image.class` do componente principal.

   O segundo usa o serviço [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) da OSGi, que é um serviço prático e nos ajuda a criar modelos do Sling de outros tipos no código Java™.

   Vamos usar a segunda abordagem.

   >[!NOTE]
   >
   >Em uma implementação real, a abordagem “Um”, que usa `@Self`, é preferível, pois é a solução mais simples e elegante. Neste tutorial, a segunda abordagem é usada, pois requer a exploração de mais facetas dos modelos do Sling que são úteis com componentes mais complexos.

   Como os modelos do Sling são POJOs de Java™, não serviços da OSGi, as anotações de injeção da OSGi comuns `@Reference` **não podem** ser usadas; em vez delas, os modelos do Sling fornecem uma anotação especial **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)**, que oferece uma funcionalidade semelhante.

1. Atualize `BylineImpl.java` para incluir a anotação `OSGiService` para inserir o `ModelFactory`:

   ```java
   import org.apache.sling.models.factory.ModelFactory;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   ...
   public class BylineImpl implements Byline {
       ...
       @OSGiService
       private ModelFactory modelFactory;
   }
   ```

   Com o `ModelFactory` disponível, um modelo do Sling de imagem dos componentes principais pode ser criado com:

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   No entanto, esse método requer uma solicitação e um recurso, o que ainda não está disponível no modelo do Sling. Para obtê-los, mais anotações do modelo do Sling são usadas.

   Para obter a solicitação atual, a anotação **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** pode ser usada para injetar o `adaptable` (que é definido no `@Model(..)` como `SlingHttpServletRequest.class`, em um campo de classe de Java™.

1. Adicione a anotação **@Self** para obter a **solicitação SlingHttpServletRequest**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   Lembre-se de que usar `@Self Image image` para inserir o modelo do Sling de imagem dos componentes principais foi uma opção acima; a anotação `@Self` tenta inserir o objeto adaptável (neste caso, uma SlingHttpServletRequest) e adaptar-se ao tipo de campo de anotação. Como o modelo do Sling de imagem dos componentes principais é adaptável a partir de objetos SlingHttpServletRequest, isso teria funcionado, e envolve menos código que a abordagem `modelFactory`, que é mais exploratória.

   Agora, as variáveis necessárias para instanciar o modelo de imagem por meio da API ModelFactory são inseridas. Vamos usar a anotação **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** do modelo do Sling para obter esse objeto após a instanciação do modelo do Sling.

   `@PostConstruct` é incrivelmente útil e atua com uma capacidade semelhante a um construtor, mas é chamado depois que a classe é instanciada e todos os campos de Java™ anotados são inseridos. Enquanto outras anotações do modelo do Sling anotam campos de classe de Java™ (variáveis), `@PostConstruct` anota um método de parâmetro nulo, zero, normalmente chamado de `init()` (mas pode ser chamado de qualquer coisa).

1. Adicionar o método **@PostConstruct**:

   ```java
   import javax.annotation.PostConstruct;
   ...
   public class BylineImpl implements Byline {
       ...
       private Image image;
   
       @PostConstruct
       private void init() {
           image = modelFactory.getModelFromWrappedRequest(request,
                                                           request.getResource(),
                                                           Image.class);
       }
       ...
   }
   ```

   Lembre-se de que os modelos do Sling **NÃO** são serviços da OSGi; portanto, é seguro manter o estado da classe. Muitas vezes, `@PostConstruct` deriva e configura o estado da classe do modelo do Sling para uso posterior, semelhante ao que um construtor simples faz.

   Se o método `@PostConstruct` acionar uma exceção, o modelo do Sling não será instanciado e será nulo.

1. **getImage()** agora pode ser atualizado para simplesmente retornar o objeto de imagem.

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. Vamos voltar a `isEmpty()` e concluir a implementação:

   ```java
   @Override
   public boolean isEmpty() {
      final Image componentImage = getImage();
   
       if (StringUtils.isBlank(name)) {
           // Name is missing, but required
           return true;
       } else if (occupations == null || occupations.isEmpty()) {
           // At least one occupation is required
           return true;
       } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
           // A valid image is required
           return true;
       } else {
           // Everything is populated, so this component is not considered empty
           return false;
       }
   }
   ```

   Observe que várias chamadas para `getImage()` não são problemáticas, pois retornam a variável de classe `image` inicializada e não invocam `modelFactory.getModelFromWrappedRequest(...)`, o que não é muito oneroso, mas vale a pena evitar chamar desnecessariamente.

1. O `BylineImpl.java` final deve ficar semelhante a:


   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import java.util.ArrayList;
   import java.util.Collections;
   import java.util.List;
   import javax.annotation.PostConstruct;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.factory.ModelFactory;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       /**
       * @PostConstruct is immediately called after the class has been initialized
       * but BEFORE any of the other public methods. 
       * It is a good method to initialize variables that is used by methods in the rest of the model
       *
       */
       @PostConstruct
       private void init() {
           // set the image object
           image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
       }
   
       @Override
       public String getName() {
           return name;
       }
   
       @Override
       public List<String> getOccupations() {
           if (occupations != null) {
               Collections.sort(occupations);
               return new ArrayList<String>(occupations);
           } else {
               return Collections.emptyList();
           }
       }
   
       @Override
       public boolean isEmpty() {
           final Image componentImage = getImage();
   
           if (StringUtils.isBlank(name)) {
               // Name is missing, but required
               return true;
           } else if (occupations == null || occupations.isEmpty()) {
               // At least one occupation is required
               return true;
           } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
               // A valid image is required
               return true;
           } else {
               // Everything is populated, so this component is not considered empty
               return false;
           }
       }
   
       /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
       */
       private Image getImage() {
           return image;
       }
   }
   ```


## Linha de identificação HTL {#byline-htl}

No módulo `ui.apps`, abra `/apps/wknd/components/byline/byline.html` criado na configuração anterior do componente do AEM.

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

Vamos analisar o que esse script HTL faz até agora:

* O `placeholderTemplate` aponta para o espaço reservado dos componentes principais, que é exibido quando o componente não está totalmente configurado. Isso é renderizado no editor de páginas do AEM Sites como uma caixa com o título do componente, conforme definido acima na propriedade `jcr:title` de `cq:Component`.

* O `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` carrega o `placeholderTemplate` definido acima e passa um valor booleano (atualmente embutido no código de `false`) para o modelo de espaço reservado. Quando `isEmpty` é verdadeiro, o modelo de espaço reservado renderiza a caixa cinza; caso contrário, não renderiza nada.

### Atualizar HTL da linha de identificação

1. Atualize **byline.html** com a seguinte estrutura de HTML estrutural:

   ```html
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!--/* Include the Core Components Image Component */-->
           </div>
           <h2 class="cmp-byline__name"><!--/* Include the name */--></h2>
           <p class="cmp-byline__occupations"><!--/* Include the occupations */--></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   Observe que as classes de CSS seguem a [convenção de nomenclatura do BEM](https://getbem.com/naming/). Embora o uso das convenções do BEM não seja obrigatório, o BEM é recomendado, pois é usado nas classes de CSS dos componentes principais e geralmente resulta em regras de CSS limpas e legíveis.

### Instanciação de objetos do modelo do Sling em HTL {#instantiating-sling-model-objects-in-htl}

A [declaração de bloco de uso](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) é usada para instanciar objetos de modelo do Sling no script HTL e atribuí-los a uma variável HTL.

O `data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` usa a interface de linha de identificação (com.adobe.aem.guides.wknd.models.Byline) implementada por BylineImpl e adapta a SlingHttpServletRequest atual a ele, e o resultado é armazenado em uma linha de identificação de nome de variável de HTL (`data-sly-use.<variable-name>`).

1. Atualize o `div` externo para fazer referência ao modelo do Sling de **Linha de identificação** por sua interface pública:

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### Acessar os métodos do modelo do Sling {#accessing-sling-model-methods}

O HTL pega emprestado do JSTL e usa o mesmo encurtamento de nomes dos métodos de obtenção do Java™.

Por exemplo, a invocação do método `getName()` do modelo do Sling de linha de identificação pode ser encurtada para `byline.name`; da mesma forma, em vez de `byline.isEmpty`, ela pode ser encurtada para `byline.empty`. O uso de nomes completos de métodos, `byline.getName` ou `byline.isEmpty`, também funciona. Observe que `()` nunca são usados para invocar métodos em HTL (semelhante ao JSTL).

Métodos de Java™ que exigem um parâmetro **não podem** ser usados em HTL. A intenção disso é manter a lógica do HTL simples.

1. O nome da linha de identificação pode ser adicionado ao componente, chamando-se o método `getName()` no modelo de Sling de linha de identificação ou no HTL: `${byline.name}`.

   Atualizar a tag `h2`:

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### Usar opções de expressão em HTL {#using-htl-expression-options}

[As opções de expressão em HTL](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) atuam como modificadores no conteúdo em HTL e variam de formatação da data à tradução de i18n. As expressões também podem ser usadas para unir listas ou matrizes de valores, que são necessárias para exibir as profissões em um formato delimitado por vírgulas.

As expressões são adicionadas por meio do operador `@` na expressão em HTL.

1. Para entrar na lista de profissões com “, ”, utiliza-se o seguinte código:

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### Exibição condicional do espaço reservado {#conditionally-displaying-the-placeholder}

A maioria dos scripts em HTL para componentes do AEM usa o **paradigma de espaço reservado** para fornecer uma dica visual aos criadores **, indicando que um componente foi criado incorretamente e não é exibido na publicação do AEM**. A convenção para conduzir essa decisão é implementar um método no modelo do Sling de apoio ao componente, neste caso: `Byline.isEmpty()`.

O método `isEmpty()` é chamado no modelo do Sling de linha de identificação, e o resultado (ou melhor, seu negativo, por meio do operador `!`) é salvo em uma variável de HTL chamada `hasContent`:

1. Atualizar o `div` externo para salvar uma variável de HTL chamada `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   Observe o uso de `data-sly-test`; o bloco de HTL `test` é fundamental, pois define uma variável de HTL e renderiza/não renderiza o elemento de HTML no qual se encontra. Tem como base o resultado da avaliação da expressão em HTL. Se for “verdadeiro”, o elemento de HTML será renderizado; caso contrário, não será renderizado.

   Essa variável de HTL `hasContent` agora pode ser reutilizada para mostrar/ocultar condicionalmente o espaço reservado.

1. Atualize a chamada condicional para `placeholderTemplate` na parte inferior do arquivo com o seguinte:

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### Exibir a imagem com os componentes principais {#using-the-core-components-image}

O script HTL para `byline.html` agora está quase concluído; só falta a imagem.

À medida que o `sling:resourceSuperType` aponta para o componente de imagem dos componentes principais para criar a imagem, o componente de imagem dos componentes principais pode ser usado para renderizar a imagem.

Para isso, vamos incluir o recurso de linha de identificação atual, mas forçar o tipo de recurso do componente de imagem dos componentes principais, usando o tipo de recurso `core/wcm/components/image/v2/image`. Trata-se de um padrão potente para reutilização de componentes. Para isso, o bloco `data-sly-resource` do HTL é usado.

1. Substitua o `div` por uma classe de `cmp-byline__image` com o seguinte:

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   Este `data-sly-resource` inclui o recurso atual por meio do caminho relativo `'.'` e força a inclusão do recurso atual (ou do recurso de conteúdo de linha de identificação) com o tipo de recurso `core/wcm/components/image/v2/image`.

   O tipo de recurso do componente principal é usado diretamente, não por proxy, porque esse é um uso no script e nunca é persistente no conteúdo.

2. Conclusão de `byline.html` abaixo:

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline" 
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
       class="cmp-byline">
       <div class="cmp-byline__image"
           data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }">
       </div>
       <h2 class="cmp-byline__name">${byline.name}</h2>
       <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

3. Implante a base de código em uma instância local do AEM. Como foram feitas alterações em `core` e `ui.apps`, ambos os módulos precisam ser implantados.

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   Para implantar no AEM 6.5/6.4, chame o perfil `classic`:

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > Você também pode criar todo o projeto a partir da raiz, usando o perfil do Maven `autoInstallSinglePackage`, mas isso pode substituir as alterações de conteúdo na página. Isso ocorre porque o `ui.content/src/main/content/META-INF/vault/filter.xml` foi modificado para o código inicial do tutorial para substituir completamente o conteúdo do AEM existente. No mundo real, isso não seria um problema.

### Revisar o componente de linha de identificação não estilizado {#reviewing-the-unstyled-byline-component}

1. Depois de implantar a atualização, navegue até a página [Ultimate Guide do LA Skateparks](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) ou até onde você tiver adicionado o componente de linha de identificação anteriormente neste capítulo.

1. A **imagem**, o **nome** e as **profissões** aparecem agora, e um componente de linha de identificação não estilizado está presente.

   ![componente de linha de identificação não estilizado](assets/custom-component/unstyled.png)

### Revisar o registro do modelo do Sling {#reviewing-the-sling-model-registration}

A [visualização do status dos modelos do Sling do console da web do AEM](http://localhost:4502/system/console/status-slingmodels) exibe todos os modelos do Sling registrados no AEM. O modelo do Sling de linha de identificação pode ser validado como instalado e reconhecido por meio da conferência desta lista.

Se o **BylineImpl** não for exibido nessa lista, provavelmente há um problema com as anotações do modelo do Sling, ou o modelo não foi adicionado ao pacote correto (`com.adobe.aem.guides.wknd.core.models`) no projeto principal.

![Modelo do Sling de linha de identificação registrado](assets/custom-component/osgi-sling-models.png)

*<http://localhost:4502/system/console/status-slingmodels>*

## Estilos de linha de identificação {#byline-styles}

Para alinhar o componente de linha de identificação com o design criativo fornecido, vamos estilizá-lo. Isso é feito por meio do arquivo SCSS, atualizando-se o arquivo no módulo **ui.frontend**.

### Adicionar um estilo padrão

Adicione estilos padrão ao componente de linha de identificação.

1. Retorne ao IDE e ao projeto **ui.frontend** gerado em `/src/main/webpack/components`:
1. Crie um arquivo chamado `_byline.scss`.

   ![gerenciador do projeto de linha de identificação](assets/custom-component/byline-style-project-explorer.png)

1. Adicionar o CSS de implementações de linha de identificação (gravado como SCSS) ao `_byline.scss`:

   ```scss
   .cmp-byline {
       $imageSize: 60px;
   
       .cmp-byline__image {
           float: left;
   
       /* This class targets a Core Component Image CSS class */
       .cmp-image__image {
           width: $imageSize;
           height: $imageSize;
           border-radius: $imageSize / 2;
           object-fit: cover;
           }
       }
   
       .cmp-byline__name {
           font-size: $font-size-medium;
           font-family: $font-family-serif;
           padding-top: 0.5rem;
           margin-left: $imageSize + 25px;
           margin-bottom: .25rem;
           margin-top:0rem;
       }
   
       .cmp-byline__occupations {
           margin-left: $imageSize + 25px;
           color: $gray;
           font-size: $font-size-xsmall;
           text-transform: uppercase;
       }
   }
   ```

1. Abra um terminal e navegue até o módulo `ui.frontend`.
1. Inicie o processo `watch` com o seguinte comando npm:

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. Retorne ao navegador e navegue até o [artigo “LA SkateParks”](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Você deve ver os estilos atualizados no componente.

   ![componente de linha de identificação concluído](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > Talvez seja necessário limpar o cache do navegador para garantir que o CSS obsoleto não esteja sendo fornecido e atualizar a página com o componente de linha de identificação para obter o estilo completo.

## Parabéns! {#congratulations}

Parabéns, você criou um componente personalizado do zero com o Adobe Experience Manager!

### Próximas etapas {#next-steps}

Continue aprendendo sobre o desenvolvimento de componentes do AEM, explorando como escrever testes de JUnit para o código de linha de identificação em Java™ a fim de garantir que tudo seja desenvolvido corretamente e que a lógica de negócios implementada esteja correta e completa.

* [Gravação de testes de unidade ou componentes do AEM](unit-testing.md)

Veja o código concluído no [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente na ramificação do Git `tutorial/custom-component-solution`.

1. Clone o repositório [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd).
1. Confira a ramificação `tutorial/custom-component-solution`
