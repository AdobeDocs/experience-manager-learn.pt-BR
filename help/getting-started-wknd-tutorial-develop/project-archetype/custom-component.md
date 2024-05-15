---
title: Componente personalizado
description: Aborda a criação completa de um componente personalizado da linha de comentários que exibe o conteúdo criado. Inclui o desenvolvimento de um Modelo Sling para encapsular a lógica de negócios para preencher o componente de linha de bytes e o HTL correspondente para renderizar o componente.
version: 6.5, Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '3869'
ht-degree: 0%

---

# Componente personalizado {#custom-component}

Este tutorial aborda a criação completa de um arquivo personalizado `Byline` Componente AEM que exibe o conteúdo criado em uma caixa de diálogo e explora o desenvolvimento de um modelo Sling para encapsular a lógica de negócios que preenche o HTL do componente.

## Pré-requisitos {#prerequisites}

Analisar as ferramentas e instruções necessárias para a configuração de um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Projeto inicial

>[!NOTE]
>
> Se você concluiu com êxito o capítulo anterior, é possível reutilizar o projeto e ignorar as etapas para verificar o projeto inicial.

Verifique o código de linha base no qual o tutorial se baseia:

1. Confira o `tutorial/custom-component-start` ramificar de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
   ```

1. Implante a base de código em uma instância de AEM local usando suas habilidades de Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se estiver usando o AEM 6.5 ou 6.4, anexe o `classic` para qualquer comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) ou confira o código localmente alternando para a ramificação `tutorial/custom-component-solution`.

## Objetivo

1. Entender como criar um componente de AEM personalizado
1. Saiba como encapsular a lógica de negócios com modelos Sling
1. Saiba como usar um modelo do Sling de dentro de um script HTL

## O que você vai criar {#what-build}

Nesta parte do tutorial da WKND, é criado um componente de Subtítulo, usado para exibir informações criadas sobre o colaborador de um artigo.

![exemplo do componente byline](assets/custom-component/byline-design.png)

*Componente de byline*

A implementação do componente Subtítulo inclui uma caixa de diálogo que coleta o conteúdo do subtítulo e um modelo Sling personalizado que recupera os detalhes como:

* Nome
* Imagem
* Ocupações

## Criar componente de Subtítulo {#create-byline-component}

Primeiro, crie a estrutura do nó do componente de Subtítulo e defina uma caixa de diálogo. Representa o componente no AEM e define implicitamente o tipo de recurso do componente por sua localização no JCR.

A caixa de diálogo expõe a interface com a qual os autores de conteúdo podem fornecer. Para esta implementação, o Componente principal WCM do AEM **Imagem** componente é usado para lidar com a criação e renderização da imagem do Subtítulo, então ele deve ser definido como deste componente `sling:resourceSuperType`.

### Criar definição de componente {#create-component-definition}

1. No **ui.apps** módulo, navegue até `/apps/wknd/components` e crie uma pasta chamada `byline`.
1. Dentro do `byline` pasta, adicione um arquivo chamado `.content.xml`

   ![caixa de diálogo para criar nó](assets/custom-component/byline-node-creation.png)

1. Preencha o `.content.xml` arquivo com o seguinte:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   O arquivo XML acima fornece a definição do componente, incluindo o título, a descrição e o grupo. A variável `sling:resourceSuperType` aponta para `core/wcm/components/image/v2/image`, que é a [Componente de imagem principal](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/image.html?lang=pt-BR).

### Criar o script HTL {#create-the-htl-script}

1. Dentro do `byline` pasta, adicionar um arquivo `byline.html`, responsável pela apresentação HTML do componente. É importante nomear o arquivo como a pasta, pois ele se torna o script padrão que o Sling usa para renderizar esse tipo de recurso.

1. Adicione o seguinte código à `byline.html`.

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

A variável `byline.html` é [revisitado mais tarde](#byline-htl), após a criação do Modelo do Sling. O estado atual do arquivo HTL permite que o componente seja exibido em um estado vazio no Editor de páginas do AEM Sites quando ele é arrastado e solto na página.

### Criar a definição do diálogo {#create-the-dialog-definition}

Em seguida, defina uma caixa de diálogo para o componente Subtítulo com os seguintes campos:

* **Nome**: um campo de texto que exibe o nome do colaborador.
* **Imagem**: uma referência à imagem biológica do colaborador.
* **Ocupações**: uma lista de profissões atribuídas ao colaborador. As profissões devem ser classificadas alfabeticamente em ordem crescente (a a z).

1. Dentro do `byline` , crie uma pasta chamada `_cq_dialog`.
1. Dentro do `byline/_cq_dialog`, adicione um arquivo chamado `.content.xml`. Esta é a definição XML da caixa de diálogo. Adicione o seguinte XML:

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

   Essas definições de nó de caixa de diálogo usam o [Fusão de recursos do Sling](https://sling.apache.org/documentation/bundles/resource-merger.html) para controlar quais guias de diálogo são herdadas do `sling:resourceSuperType` componente, neste caso, o **Componente de imagem dos Componentes principais**.

   ![caixa de diálogo concluída para subtítulo](assets/custom-component/byline-dialog-created.png)

### Criar a caixa de diálogo Política {#create-the-policy-dialog}

Seguindo a mesma abordagem da criação de caixa de diálogo, crie uma caixa de diálogo de política (anteriormente conhecida como caixa de diálogo de design) para ocultar campos indesejados na configuração de política herdada do componente de Imagem dos Componentes principais.

1. Dentro do `byline` , crie uma pasta chamada `_cq_design_dialog`.
1. Dentro do `byline/_cq_design_dialog`, crie um arquivo chamado `.content.xml`. Atualize o arquivo com o seguinte: com o seguinte XML. É mais fácil abrir o `.content.xml` e copie/cole o XML abaixo nele.

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

   A base para o anterior **Caixa de diálogo Política** O XML foi obtido do [Componente de imagem dos Componentes principais](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   Como na configuração da caixa de diálogo, [Fusão de recursos do Sling](https://sling.apache.org/documentation/bundles/resource-merger.html) é usado para ocultar campos irrelevantes que, de outra forma, são herdados do `sling:resourceSuperType`, conforme visto pelas definições de nó com `sling:hideResource="{Boolean}true"` propriedade.

### Implantar o código {#deploy-the-code}

1. Sincronizar as alterações em `ui.apps` com seu IDE ou usando suas habilidades em Maven.

   ![Exportar para o componente de linha de identificação do servidor AEM](assets/custom-component/export-byline-component-aem.png)

## Adicionar o componente a uma página {#add-the-component-to-a-page}

Para manter as coisas simples e focadas no desenvolvimento de componentes do AEM, vamos adicionar o componente Subtítulo em seu estado atual a uma página de Artigo para verificar o `cq:Component` a definição do nó está correta. Também para verificar se o AEM reconhece a nova definição do componente e a caixa de diálogo do componente funciona para criação.

### Adicionar uma imagem à AEM Assets

Primeiro, carregue uma amostra de head shot no AEM Assets para usar para preencher a imagem no componente Byline.

1. Navegue até a pasta LA Skateparks no AEM Assets: [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. Carregar o tiro na cabeça para  **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)** à pasta.

   ![Headshot carregado no AEM Assets](assets/custom-component/stacey-roswell-headshot-assets.png)

### Criar o componente {#author-the-component}

Em seguida, adicione o componente Subtítulo a uma página no AEM. Como o componente Subtítulo é adicionado à variável **Projeto de sites WKND - Conteúdo** Grupo de componentes, por meio da `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml` definição, ele é disponibilizado automaticamente para qualquer **Container** cujo **Política** permite que o **Projeto de sites WKND - Conteúdo** grupo de componentes. Assim, ele está disponível no Contêiner de layout da página do artigo .

1. Navegue até o artigo LA Skatepark em: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. Na barra lateral esquerda, arraste e solte uma **Componente de byline** em para **bottom** do Contêiner de layout da página de artigo aberta.

   ![adicionar componente de linha de guia à página](assets/custom-component/add-to-page.png)

1. Verifique se a barra lateral esquerda está aberta **e visível, e a** O Localizador de ativos** está selecionado.

1. Selecione o **Espaço reservado do componente de Subtítulo**, que por sua vez exibe a barra de ação e toque no **chave inglesa** ícone para abrir o diálogo.

1. Com a caixa de diálogo aberta e a primeira guia (Ativo) ativa, abra a barra lateral esquerda e, no localizador de ativos, arraste uma imagem para a área de soltar Imagem. Procure por &quot;stacey&quot; para encontrar a imagem bio de Stacey Roswells fornecida no pacote ui.content da WKND.

   ![adicionar imagem à caixa de diálogo](assets/custom-component/add-image.png)

1. Depois de adicionar uma imagem, clique no link **Propriedades** para entrar na **Nome** e **Ocupações**.

   Ao inserir ocupações, insira-as em **alfabético invertido** para verificar a lógica de negócios de alfabetização implementada no Modelo Sling.

   Toque no **Concluído** botão na parte inferior direita para salvar as alterações.

   ![preencher propriedades do componente de byline](assets/custom-component/add-properties.png)

   Os autores do AEM configuram e criam componentes por meio das caixas de diálogo. Nesse ponto, no desenvolvimento do componente Subtítulo, as caixas de diálogo são incluídas para coletar os dados, no entanto, a lógica para renderizar o conteúdo criado ainda não foi adicionada. Portanto, somente o espaço reservado é exibido.

1. Depois de salvar a caixa de diálogo, navegue até [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) e revise como o conteúdo do componente é armazenado no nó do conteúdo do componente byline na página AEM.

   Encontre o nó de conteúdo do componente Byline abaixo da página Los Angeles Skate Parks, ou seja, `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`.

   Observe os nomes das propriedades `name`, `occupations`, e `fileReference` são armazenados no **nó de linha de assinante**.

   Além disso, observe o `sling:resourceType` do nó estiver definido como `wknd/components/content/byline` que é o que vincula esse nó de conteúdo à implementação do componente Subtítulo.

   ![propriedades da linha de bytes no CRXDE](assets/custom-component/byline-properties-crxde.png)

## Criar modelo Sling de byline {#create-sling-model}

Em seguida, vamos criar um Modelo do Sling que atue como o modelo de dados e hospede a lógica de negócios do componente de Linha de negócios.

Os Modelos do Sling são POJOs (Plain Old Java™ Objects) do Java™ orientados por anotações que facilitam o mapeamento de dados do JCR para variáveis do Java™ e fornecem eficiência ao desenvolver no contexto do AEM.

### Revisar dependências do Maven {#maven-dependency}

O modelo Byline Sling depende de várias APIs Java™ fornecidas pelo AEM. Essas APIs são disponibilizadas por meio da `dependencies` listado na `core` do módulo. O projeto usado para este tutorial foi criado para o AEM as a Cloud Service. No entanto, é único, pois é compatível com o AEM 6.5/6.4. Portanto, ambas as dependências para o Cloud Service e AEM 6.x estão incluídas.

1. Abra o `pom.xml` arquivo abaixo `<src>/aem-guides-wknd/core/pom.xml`.
1. Encontre a dependência para `aem-sdk-api` - **AEM somente as a Cloud Service**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   A variável [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en) contém todas as APIs do Java™ públicas expostas pelo AEM. A variável `aem-sdk-api` é usado por padrão ao criar este projeto. A versão é mantida no pom do reator principal da raiz do projeto em `aem-guides-wknd/pom.xml`.

1. Encontre a dependência para a `uber-jar` - **AEM 6.5/6.4 apenas**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   A variável `uber-jar` é incluído somente quando a variável `classic` é chamado, ou seja, `mvn clean install -PautoInstallSinglePackage -Pclassic`. Novamente, isso é exclusivo para este projeto. Em um projeto do mundo real, gerado pelo Arquétipo de Projeto AEM, o `uber-jar` é o padrão se a versão do AEM especificada for 6.5 ou 6.4.

   A variável [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) contém todas as APIs públicas do Java™ expostas pelo AEM 6.x. A versão é mantida no pom do reator principal da raiz do projeto `aem-guides-wknd/pom.xml`.

1. Encontre a dependência para `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   Estas são as APIs do Java™ públicas completas expostas pelos Componentes principais do AEM. Componentes principais do AEM é um projeto mantido fora do AEM e, portanto, tem um ciclo de lançamento separado. Por esse motivo, é uma dependência que precisa ser incluída separadamente e é **não** incluído com o `uber-jar` ou `aem-sdk-api`.

   Como o uber-jar, a versão dessa dependência é mantida no arquivo pom do reator principal de `aem-guides-wknd/pom.xml`.

   Posteriormente neste tutorial, a classe Imagem do componente principal é usada para exibir a imagem no componente Subtítulo. É necessário ter a dependência do Componente principal para criar e compilar o Modelo do Sling.

### Interface de byline {#byline-interface}

Crie uma Interface Java™ pública para o Subtítulo. A variável `Byline.java` define os métodos públicos necessários para conduzir o `byline.html` Script HTL.

1. Dentro, o `core` módulo na `core/src/main/java/com/adobe/aem/guides/wknd/core/models` pasta criar um arquivo chamado `Byline.java`

   ![criar interface de linha de identificação](assets/custom-component/create-byline-interface.png)

1. Atualizar `Byline.java` com os seguintes métodos:

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

   Os dois primeiros métodos expõem os valores para a variável **name** e **ocupações** para o componente Subtítulo.

   A variável `isEmpty()` O método é usado para determinar se o componente tem conteúdo para renderizar ou se está aguardando para ser configurado.

   Observe que não há método para a Imagem; [isso é revisado posteriormente](#tackling-the-image-problem).

1. Os pacotes Java™ que contêm classes públicas Java™, neste caso um Modelo Sling, devem ter a versão criada usando o  `package-info.java` arquivo.

   Desde que o pacote Java™ de origem do WKND `com.adobe.aem.guides.wknd.core.models` declara a versão de `1.0.0`e uma interface pública e métodos ininterruptos estiverem sendo adicionados, a versão deve ser aumentada para `1.1.0`. Abra o arquivo em `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` e atualizar `@Version("1.0.0")` para `@Version("2.1.0")`.

   ```
   @Version("2.1.0")
   package com.adobe.aem.guides.wknd.core.models;
   
   import org.osgi.annotation.versioning.Version;
   ```

Sempre que forem feitas alterações nos arquivos deste pacote, a variável [a versão do pacote deve ser ajustada semanticamente](https://semver.org/). Caso contrário, as configurações do projeto Maven [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd) O detecta uma versão de pacote inválida e quebra a compilação. Felizmente, em caso de falha, o plug-in Maven relata a versão inválida do pacote Java™ e a versão que deveria ser. Atualize o `@Version("...")` declaração na violação do pacote Java™ `package-info.java` à versão recomendada pelo plug-in para correção.

### Implementação de linha de comando {#byline-implementation}

A variável `BylineImpl.java` é a implementação do Modelo Sling que implementa o `Byline.java` definida anteriormente. O código completo para `BylineImpl.java` podem ser encontradas na parte inferior desta seção.

1. Crie uma pasta chamada `impl` debaixo `core/src/main/java/com/adobe/aem/guides/core/models`.
1. No `impl` , criar um arquivo `BylineImpl.java`.

   ![Arquivo Impl de Subtítulo](assets/custom-component/byline-impl-file.png)

1. Abertura `BylineImpl.java`. Especificar que ele implementa o `Byline` interface. Use os recursos de preenchimento automático do IDE ou atualize manualmente o arquivo para incluir os métodos necessários para implementar o `Byline` interface:

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

1. Adicionar as anotações do Modelo do Sling atualizando `BylineImpl.java` com as seguintes anotações em nível de classe. Este `@Model(..)`A anotação é o que transforma a classe em um modelo Sling.

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

   * A variável `@Model` A anotação registra BylineImpl como um Modelo do Sling quando implantado no AEM.
   * A variável `adaptables` especifica que esse modelo pode ser adaptado pela solicitação.
   * A variável `adapters` permite que a classe de implementação seja registrada na interface Byline. Isso permite que o script HTL chame o Modelo Sling pela interface (em vez da implementação diretamente). [Mais detalhes sobre adaptadores podem ser encontrados aqui.](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * A variável `resourceType` aponta para o tipo de recurso do componente Subtítulo (criado anteriormente) e ajuda a resolver o modelo correto se houver várias implementações. [Mais detalhes sobre como associar uma classe de modelo a um tipo de recurso podem ser encontrados aqui](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130).

### Implementação dos métodos do Modelo do Sling {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

O primeiro método implementado é `getName()`, ela simplesmente retorna o valor armazenado no nó de conteúdo JCR da linha sob a propriedade `name`.

Para isso, a variável `@ValueMapValue` A anotação Modelo Sling é usada para injetar o valor em um campo Java™ usando o ValueMap do recurso da solicitação.


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

Como a propriedade JCR compartilha o nome como o campo Java™ (ambos são &quot;name&quot;), `@ValueMapValue` O resolve automaticamente essa associação e injeta o valor da propriedade no campo Java™.

#### getOccupations() {#implementing-get-occupations}

O próximo método para implementar o é `getOccupations()`. Este método carrega as ocupações armazenadas na propriedade JCR `occupations` e retornam uma coleção classificada (alfabeticamente) deles.

Utilizando a mesma técnica explorada no `getName()` o valor da propriedade pode ser inserido no campo Modelo do Sling.

Quando os valores de propriedade do JCR estiverem disponíveis no Modelo Sling por meio do campo Java™ inserido `occupations`, a lógica comercial de classificação pode ser aplicada na variável `getOccupations()` método.


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

O último método público é `isEmpty()` que determina quando o componente deve se considerar &quot;criado o suficiente&quot; para renderizar.

Para esse componente, o requisito comercial são os três campos: `name, image and occupations` deve ser preenchido *antes* o componente pode ser renderizado.


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


#### Como lidar com o &quot;problema da imagem&quot; {#tackling-the-image-problem}

Verificar o nome e as condições de ocupação são triviais e o Apache Commons Lang3 oferece a opção [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) classe. No entanto, não está claro como a **presença da imagem** podem ser validados, pois o componente de Imagem, dos Componentes principais, é usado para destacar a imagem.

Há duas maneiras de resolver isso:

Verifique se `fileReference` A propriedade JCR é resolvida como um ativo. *OU* Converta esse recurso em um Modelo do Sling de imagem dos Componentes principais e verifique se `getSrc()` o método não está vazio.

Vamos usar o **segundo** abordagem. A primeira abordagem provavelmente é suficiente, mas neste tutorial a última é usada para permitir explorar outros recursos dos Modelos Sling.

1. Crie um método privado que obtenha a Imagem. Esse método é deixado privado porque não há necessidade de expor o objeto Image no próprio HTL e é usado apenas para direcionar `isEmpty().`

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

   Como observado acima, há mais duas abordagens para obter a **Modelo de imagem Sling**:

   O primeiro usa o `@Self` anotação, para adaptar automaticamente a solicitação atual ao do Componente principal `Image.class`

   O segundo usa a variável [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) Serviço OSGi, que é um serviço prático, e nos ajuda a criar Modelos Sling de outros tipos no código Java™.

   Vamos usar a segunda abordagem.

   >[!NOTE]
   >
   >Em uma implementação real, aborde &quot;Um&quot;, usando `@Self` é preferível, pois é a solução mais simples e elegante. Neste tutorial, a segunda abordagem é usada, pois requer a exploração de mais facetas dos Modelos Sling que são úteis são componentes mais complexos!

   Como os Modelos Sling são POJOs Java™, e não Serviços OSGi, as anotações de injeção OSGi habituais `@Reference` **não é possível** ser usados, em vez disso os Modelos Sling fornecem um **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** uma anotação que fornece funcionalidade semelhante.

1. Atualizar `BylineImpl.java` para incluir o `OSGiService` anotação para inserir o `ModelFactory`:

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

   Com o `ModelFactory` disponível, um Modelo do Sling de imagem do componente principal pode ser criado usando:

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   No entanto, esse método requer uma solicitação e um recurso, nenhum ainda disponível no Modelo do Sling. Para obtê-las, mais anotações do Modelo Sling são usadas!

   Para obter a solicitação atual, o **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** a anotação pode ser usada para inserir o `adaptable` (que é definido na variável `@Model(..)` as `SlingHttpServletRequest.class`, em um campo de classe Java™.

1. Adicione o **@Self** anotação para obter o **Solicitação SlingHttpServletRequest**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   Lembre-se de usar `@Self Image image` para injetar o Modelo do Sling de imagem do componente principal era uma opção acima - o `@Self` A anotação tenta inserir o objeto adaptável (nesse caso, um SlingHttpServletRequest) e adaptar ao tipo de campo de anotação. Como o Modelo de Sling de imagem do componente principal é adaptável a partir de objetos SlingHttpServletRequest, isso teria funcionado e tem menos código do que o mais exploratório `modelFactory` abordagem.

   Agora, as variáveis necessárias para instanciar o modelo de Imagem por meio da API ModelFactory são inseridas. Vamos usar o do Modelo Sling **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** anotação para obter esse objeto depois que o Modelo Sling for instanciado.

   `@PostConstruct` O é incrivelmente útil e atua com uma capacidade semelhante à de um construtor, no entanto, ele é chamado depois que a classe é instanciada e todos os campos Java™ anotados são inseridos. Enquanto outras anotações do Modelo Sling anotam campos de classe Java™ (variáveis), `@PostConstruct` anota um método de parâmetro void, zero, normalmente chamado de `init()` (mas pode ter qualquer nome).

1. Adicionar **@PostConstruct** método:

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

   Lembre-se, os Modelos do Sling são **NOT** Serviços OSGi, portanto, é seguro manter o estado da classe. Frequentemente `@PostConstruct` deriva e configura o estado da classe Sling Model para uso posterior, semelhante ao que um construtor simples faz.

   Se a variável `@PostConstruct` O método lança uma exceção, o Modelo Sling não é instanciado e é nulo.

1. **getImage()** O agora pode ser atualizado para simplesmente retornar o objeto de imagem.

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. Vamos voltar para `isEmpty()` e conclua a implementação:

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

   Observe várias chamadas para `getImage()` não é problemático, pois retorna a variável inicializada `image` variável de classe e não chama `modelFactory.getModelFromWrappedRequest(...)` o que não é muito caro, mas vale a pena evitar chamar desnecessariamente.

1. A versão final `BylineImpl.java` deve ser semelhante a:


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


## Subtítulo HTL {#byline-htl}

No `ui.apps` módulo, open `/apps/wknd/components/byline/byline.html` que foi criado na configuração anterior do componente AEM.

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

Vamos analisar o que esse script HTL faz até agora:

* A variável `placeholderTemplate` aponta para o espaço reservado dos Componentes principais, que é exibido quando o componente não está totalmente configurado. Isso é renderizado no Editor de páginas do AEM Sites como uma caixa com o título do componente, conforme definido acima na `cq:Component`do  `jcr:title` propriedade.

* A variável `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` carrega o `placeholderTemplate` definido acima e passa em um valor booleano (atualmente codificado para `false`) no modelo de espaço reservado. Quando `isEmpty` for true, o modelo de espaço reservado renderizará a caixa cinza, caso contrário, não renderizará nada.

### Atualizar HTL de byline

1. Atualizar **byline.html** com a seguinte estrutura de HTML esqueleto:

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

   Observe que as classes CSS seguem a variável [Convenção de nomenclatura de BEM](https://getbem.com/naming/). Embora o uso das convenções de BEM não seja obrigatório, o BEM é recomendado, pois é usado nas classes CSS dos Componentes principais e geralmente resulta em regras CSS limpas e legíveis.

### Instanciação de objetos Modelo do Sling no HTL {#instantiating-sling-model-objects-in-htl}

A variável [Usar instrução em bloco](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) é usado para instanciar objetos Modelo do Sling no script HTL e atribuí-lo a uma variável HTL.

A variável `data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` O usa a interface Byline (com.adobe.aem.guides.wknd.models.Byline) implementada por BylineImpl e adapta o SlingHttpServletRequest atual a ele, e o resultado é armazenado em uma linha de nome de variável HTL ( `data-sly-use.<variable-name>`).

1. Atualizar o externo `div` para referenciar a **Subtítulo** Modelo Sling por sua interface pública:

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### Acesso aos métodos do Modelo do Sling {#accessing-sling-model-methods}

O HTL pega emprestado do JSTL e usa o mesmo encurtamento de nomes de métodos Getter do Java™.

Por exemplo, chamar o modelo Sling de byline `getName()` pode ser encurtado para `byline.name`, de forma semelhante em vez de `byline.isEmpty`, pode ser reduzido para `byline.empty`. Usando nomes completos de método, `byline.getName` ou `byline.isEmpty`, também funciona. Observe que `()` nunca são usados para invocar métodos em HTL (semelhante ao JSTL).

Métodos Java™ que exigem um parâmetro **não é possível** ser usado no HTL. Isso ocorre projetado para manter a lógica no HTL simples.

1. O nome do Subtítulo pode ser adicionado ao componente chamando o `getName()` no Modelo Byline Sling ou no HTL: `${byline.name}`.

   Atualize o `h2` tag:

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### Uso de opções de expressão HTL {#using-htl-expression-options}

[Opções de expressões HTL](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) atuam como modificadores no conteúdo em HTL, e variam de formatação de data a tradução i18n. Expressões também podem ser usadas para unir listas ou matrizes de valores, que são o que é necessário para exibir as ocupações em um formato delimitado por vírgulas.

As expressões são adicionadas por meio de `@` operador na expressão HTL.

1. Para ingressar na lista de ocupações com &quot;, &quot;, é usado o seguinte código:

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### Exibição condicional do espaço reservado {#conditionally-displaying-the-placeholder}

A maioria dos scripts HTL para componentes AEM usa o **paradigma de espaço reservado** para fornecer uma dica visual aos autores **indicando que um componente foi criado incorretamente e não é exibido na publicação do AEM**. A convenção para conduzir essa decisão é implementar um método no modelo Sling de apoio do componente, neste caso: `Byline.isEmpty()`.

A variável `isEmpty()` é chamado no modelo Byline Sling e o resultado (ou melhor, é negativo, através da variável `!` operador) é salvo em uma variável HTL chamada `hasContent`:

1. Atualizar o externo `div` para salvar uma variável HTL chamada `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   Observe o uso de `data-sly-test`, o HTL `test` for chave, ele definirá uma variável HTL e renderizará/não renderizará o elemento HTML em que está. Tem como base o resultado da avaliação da expressão HTL. Se &quot;true&quot;, o elemento HTML será renderizado, caso contrário, não será renderizado.

   Essa variável HTL `hasContent` agora podem ser reutilizados para mostrar/ocultar condicionalmente o espaço reservado.

1. Atualize a chamada condicional para o `placeholderTemplate` na parte inferior do arquivo com o seguinte:

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### Exibir a imagem usando os Componentes principais {#using-the-core-components-image}

O script HTL para `byline.html` O agora está quase concluído e a imagem não aparece.

Como a variável `sling:resourceSuperType` aponta para o componente de Imagem do Componente principal para criar a imagem. O componente de Imagem do Componente principal pode ser usado para renderizar a imagem.

Para isso, vamos incluir o recurso de byline atual, mas forçar o tipo de recurso do componente de Imagem do Componente principal, usando o tipo de recurso `core/wcm/components/image/v2/image`. Este é um padrão poderoso para a reutilização de componentes. Para isso, as diretrizes do HTL `data-sly-resource` bloco é usado.

1. Substitua o `div` com uma classe de `cmp-byline__image` com o seguinte:

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   Este `data-sly-resource`, inclui o recurso atual por meio do caminho relativo `'.'`e força a inclusão do recurso atual (ou do recurso de conteúdo de linha de byline) com o tipo de recurso de `core/wcm/components/image/v2/image`.

   O tipo de recurso do Componente principal é usado diretamente, e não por proxy, porque esse é um uso no script e nunca é persistente no conteúdo.

2. Concluído `byline.html` abaixo:

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

3. Implante a base de código em uma instância de AEM local. Como foram feitas alterações no `core` e `ui.apps` ambos os módulos precisam ser implantados.

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   Para implantar no AEM 6.5/6.4, chame o `classic` perfil:

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > Você também pode criar todo o projeto a partir da raiz usando o perfil Maven `autoInstallSinglePackage` mas isso pode substituir as alterações de conteúdo na página. Isso ocorre porque a variável `ui.content/src/main/content/META-INF/vault/filter.xml` foi modificado para o código inicial do tutorial para substituir completamente o conteúdo existente do AEM. Em um cenário do mundo real, isso não é um problema.

### Revisão do componente de Subtítulo não estilizado {#reviewing-the-unstyled-byline-component}

1. Depois de implantar a atualização, navegue até o [Guia Ultimate para os Skateparks de Los Angeles](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) ou sempre que você adicionou o componente Subtítulo anteriormente no capítulo.

1. A variável **imagem**, **name**, e **ocupações** agora aparecem e um não estilizado, mas o componente Subtítulo de trabalho está presente.

   ![componente de linha de guia sem estilo](assets/custom-component/unstyled.png)

### Revisão do registro do Modelo do Sling {#reviewing-the-sling-model-registration}

A variável [Visualização de status dos modelos Sling do console da Web do AEM](http://localhost:4502/system/console/status-slingmodels) exibe todos os modelos Sling registrados no AEM. O modelo Byline Sling pode ser validado como instalado e reconhecido ao revisar esta lista.

Se a variável **BylineImpl** não é exibido nessa lista, provavelmente é um problema com as anotações do Modelo do Sling ou o Modelo não foi adicionado ao pacote correto (`com.adobe.aem.guides.wknd.core.models`) no projeto principal.

![Modelo de Sling de byline registrado](assets/custom-component/osgi-sling-models.png)

*<http://localhost:4502/system/console/status-slingmodels>*

## Estilos de subtítulo {#byline-styles}

Para alinhar o componente Subtítulo com o design criativo fornecido, vamos estilizá-lo. Isso é feito usando o arquivo SCSS e atualizando o arquivo no **ui.frontend** módulo.

### Adicionar um estilo padrão

Adicione estilos padrão ao componente Subtítulo.

1. Retorne ao IDE e à **ui.frontend** projeto em `/src/main/webpack/components`:
1. Crie um arquivo chamado `_byline.scss`.

   ![explorador de projetos byline](assets/custom-component/byline-style-project-explorer.png)

1. Adicione o CSS de implementações de byline (gravado como SCSS) no `_byline.scss`:

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

1. Abra um terminal e navegue até o `ui.frontend` módulo.
1. Inicie o `watch` processe com o seguinte comando npm:

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. Retorne ao navegador e navegue até o [Artigo sobre LA SkateParks](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Você deve ver os estilos atualizados no componente.

   ![componente de linha de destino finalizado](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > Talvez seja necessário limpar o cache do navegador para garantir que o CSS obsoleto não esteja sendo fornecido e atualizar a página com o componente de Subtítulo para obter o estilo completo.

## Parabéns. {#congratulations}

Parabéns, você criou um componente personalizado do zero usando o Adobe Experience Manager!

### Próximas etapas {#next-steps}

Continue a aprender sobre o desenvolvimento de componentes AEM explorando como escrever testes JUnit para o código Byline Java™ para garantir que tudo seja desenvolvido corretamente e que a lógica de negócios implementada seja correta e completa.

* [Gravação de testes de unidade ou componentes do AEM](unit-testing.md)

Exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente em na ramificação Git `tutorial/custom-component-solution`.

1. Clonar o [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) repositório.
1. Confira o `tutorial/custom-component-solution` ramificação
