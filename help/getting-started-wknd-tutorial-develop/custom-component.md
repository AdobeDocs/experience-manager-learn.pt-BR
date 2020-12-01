---
title: Componente personalizado
description: Abrange a criação completa de um componente de byline personalizado que exibe o conteúdo criado. Inclui o desenvolvimento de um Modelo Sling para encapsular a lógica comercial para preencher o componente byline e o HTL correspondente para renderizar o componente.
sub-product: sites
feature: sling-models
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4072
mini-toc-levels: 1
thumbnail: 30181.jpg
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '4011'
ht-degree: 0%

---


# Componente personalizado {#custom-component}

Este tutorial aborda a criação completa de um Componente de Linha de AEM personalizado que exibe o conteúdo criado em uma caixa de diálogo e explora o desenvolvimento de um Modelo Sling para encapsular a lógica comercial que preenche o HTL do componente.

## Pré-requisitos {#prerequisites}

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Projeto inicial

>[!NOTE]
>
> Se você tiver seguido em partes anteriores do tutorial, você observará que o Projeto inicial deste capítulo acelera a implementação. Inclui mais alguns modelos e muito mais conteúdo. Como bônus, sinta-se à vontade para explorar o novo conteúdo e outras áreas da implementação, fora do desenvolvimento de componentes personalizados.

Confira o código básico no qual o tutorial se baseia:

1. Clique no repositório [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd).
1. Confira a ramificação `custom-component/start`

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout custom-component/start
   ```

1. Implante a base de código para uma instância AEM local usando suas habilidades Maven:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

Você sempre pode visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd/tree/custom-component/solution) ou fazer check-out do código localmente ao alternar para a ramificação `custom-component/solution`.

## Objetivo

1. Entenda como criar um componente AEM personalizado
1. Saiba como encapsular a lógica comercial com os Modelos Sling
1. Saiba como usar um Modelo Sling de dentro de um Script HTL

## O que você vai criar {#byline-component}

>[!VIDEO](https://video.tv.adobe.com/v/30181/?quality=12&learn=on)

Nesta parte do tutorial da WKND, um Componente de linha de identificação é criado e será usado para exibir informações de autoria sobre o colaborador de um artigo.

![exemplo de componente de byline](./assets/custom-component/byline-design.png)

*Desenho visual do componente byline fornecido pela equipe de design WKND*

A implementação do componente Linha de identificação inclui uma caixa de diálogo que coleta o conteúdo da linha de bytes e um Modelo de sling personalizado que recupera o conteúdo da linha de bytes:

* Nome
* Imagem
* Ocupações

para exibição por um script HTL, que renderiza o HTML que o navegador exibe.

![descomposição de linha de bytes](./assets/custom-component/byline-decomposition.png)

*Descontinuação de componente de byte*

## Criar componente de Linha de identificação {#create-byline-component}

Primeiro, crie a estrutura do nó Componente de Linha de identificação e defina uma caixa de diálogo. Isso representa o Componente no AEM e define implicitamente o tipo de recurso do componente pela sua localização no JCR.

A caixa de diálogo expõe a interface com a qual os autores de conteúdo podem fornecer. Para essa implementação, o componente **Image** do componente principal do WCM AEM será aproveitado para lidar com a criação e renderização da imagem da Linha de identificação, portanto, ela será definida como `sling:resourceSuperType` do nosso componente.

### Criar Nó de Componente {#create-component-node}

1. No módulo **ui.apps**, navegue até `/apps/wknd/components/content` e crie um novo nó chamado **byline** do tipo `cq:Component`.

   ![diálogo para criar o nó](./assets/custom-component/byline-node-creation.png)

1. Adicione as seguintes propriedades ao nó `cq:Component` do componente Byline.

   ```plain
   jcr:title = Byline
   jcr:description = Displays a contributor's byline.
   componentGroup = WKND.Content
   sling:resourceSuperType =  core/wcm/components/image/v2/image
   ```

   ![Propriedades de componentes de byte](./assets/custom-component/byline-component-properties.png)

   O que resulta neste XML `.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root
       xmlns:sling="https://sling.apache.org/jcr/sling/1.0" xmlns:jcr="https://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND.Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

### Criar o script HTL {#create-the-htl-script}

1. Abaixo do nó `byline`, adicione um novo arquivo `byline.html`, que é responsável pela apresentação em HTML do componente. Nomear o arquivo como o nó `cq:Component` é importante, pois ele se torna o script padrão que Sling usará para renderizar esse tipo de recurso.

1. Adicione o seguinte código ao `byline.html`.

   ```xml
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

`byline.html` for  [revisitado posteriormente](#byline-htl), assim que o Modelo Sling for criado. O estado atual do arquivo HTL permite que o componente seja exibido em um estado vazio, no Editor de páginas do AEM Sites quando ele é arrastado e solto na página.

### Criar a definição da caixa de diálogo {#create-the-dialog-definition}

Em seguida, defina uma caixa de diálogo para o componente Linha de identificação com os seguintes campos:

* **Nome**: um campo de texto com o nome do colaborador.
* **Imagem**: uma referência à imagem biológica do contribuinte.
* **Profissões**: uma lista de ocupações atribuídas ao contribuinte. As profissões devem ser ordenadas por ordem alfabética (a a z).

1. Abaixo do nó do componente `byline` crie um novo nó chamado `cq:dialog` do tipo `nt:unstructured`.
1. Atualize `cq:dialog` com o seguinte XML. É mais fácil abrir o `.content.xml` e copiar/colar o seguinte XML nele.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
           xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
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

   Essas definições de nó usam [Fusão de recursos Sling](https://sling.apache.org/documentation/bundles/resource-merger.html) para controlar quais guias de diálogo são herdadas do componente `sling:resourceSuperType`, neste caso o componente de imagem **Componentes principais&#39;**.

   ![caixa de diálogo concluída para linha de identificação](./assets/custom-component/byline-dialog-created.png)

### Criar a caixa de diálogo Política {#create-the-policy-dialog}

Seguindo a mesma abordagem da criação da caixa de diálogo, crie uma caixa de diálogo Política (anteriormente conhecida como uma caixa de diálogo Design) para ocultar campos indesejados na configuração Política herdada do componente Imagem dos componentes principais.

1. Abaixo do nó `byline` `cq:Component`, crie um novo nó chamado `cq:design_dialog` do tipo `nt:unstructured`.
1. Atualize `cq:design_dialog` com o seguinte XML. É mais fácil abrir o `.content.xml` e copiar/colar o XML abaixo nele.

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

   A base para o XML **diálogo de Política** anterior foi obtida do componente [Imagem dos Componentes Principais](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   Como na configuração da caixa de diálogo, [Sling Resource Fusion](https://sling.apache.org/documentation/bundles/resource-merger.html) é usado para ocultar campos irrelevantes que, de outra forma, são herdados da propriedade `sling:resourceSuperType`, conforme visto pelas definições de nó com a propriedade `sling:hideResource="{Boolean}true"`.

### Implantar o código {#deploy-the-code}

1. Implante a base de código atualizada para uma instância AEM local usando suas habilidades Maven:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallPackage
   ```

### Adicionar o componente a uma página {#add-the-component-to-a-page}

Para manter as coisas simples e focadas AEM desenvolvimento de componentes, adicionaremos o componente Linha de identificação em seu estado atual a uma página do Artigo para verificar se a definição do nó `cq:Component` está implantada e correta, AEM reconhece a nova definição de componente e a caixa de diálogo do componente funciona para criação.

Como nós [adicionamos o componente Byline ao **WKND.Content** Grupo de componentes](#create-component-node), por meio da propriedade `/apps/wknd/components/content/byline@componentGroup=WKND.Content`, ele fica automaticamente disponível para qualquer **Container de layout** cujo **Política** permite o **WKND.Content** grupo de componentes, que é o Container Layout da página de artigo.

#### Arraste e solte o componente na página {#drag-and-drop-the-component-onto-the-page}

1. **** Edite a página do artigo em  **AEM > Sites > Site da WKND > Idioma Principal > Inglês > Revista > Guia Ultimate para LA Skatepark**.
1. Na barra lateral esquerda, arraste e solte um **componente de Linha de identificação** em **bottom** do Container de layout da página de artigo aberta.

   ![adicionar componente de linha de bytes à página](assets/custom-component/add-to-page.png)

#### Crie o componente {#author-the-component}

AEM autores configuram e criam componentes por meio das caixas de diálogo. Nesse ponto do desenvolvimento do componente Linha de identificação, as caixas de diálogo são incluídas para coletar os dados, no entanto, a lógica de renderização do conteúdo criado ainda não foi adicionada.

1. Verifique se a **barra lateral esquerda está aberta** e visível, e se **Localizador de ativos** está selecionada.

   ![localizador de ativos abertos](assets/custom-component/open-asset-finder.png)

1. Selecione **Espaço reservado do componente Byline**, que por sua vez exibe a barra de ação e toque no ícone **chave** para abrir a caixa de diálogo.

   ![barra de ação do componente](assets/custom-component/action-bar.png)

1. Com a caixa de diálogo aberta e a primeira guia (Ativo) ativa, abra a barra lateral esquerda e, a partir do localizador de ativos, arraste uma imagem para a zona suspensa Imagem. Procure &quot;stacey&quot; para encontrar a imagem biológica de Stacey Roswells fornecida no pacote ui.content da WKND.

   **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)**

   ![adicionar imagem à caixa de diálogo](assets/custom-component/add-image.png)

1. Depois de adicionar uma imagem, clique na guia **Propriedades** para digitar **Nome** e **Ocupações**.

   Ao entrar em ocupações, digite-as na ordem **alfabética inversa** para que a lógica comercial alfabética que implementaremos no Modelo Sling seja prontamente aparente.

   Toque no botão **Done** na parte inferior direita para salvar as alterações.

   ![preencher propriedades do componente byline](assets/custom-component/add-properties.png)

1. Depois de salvar a caixa de diálogo, navegue até [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/language-masters/en/magazine/guide-la-skateparks/jcr:content/root/responsivegrid/responsivegrid/byline) e analise como o conteúdo do componente é armazenado no nó de conteúdo do componente de linha de bytes na página AEM.

   Localize o nó de conteúdo do componente Linha de identificação abaixo do nó `jcr:content/root/responsivegrid/responsivegrid`, ou seja, `/content/wknd/language-masters/en/magazine/guide-la-skateparks/jcr:content/root/responsivegrid/responsivegrid/byline`.

   Observe que os nomes de propriedade `name`, `occupations` e `fileReference` são armazenados no **nó de linha de bytes**.

   Além disso, observe que `sling:resourceType` do nó está definido como `wknd/components/content/byline`, que é o que vincula esse nó de conteúdo à implementação do componente Linha de identificação.

   ![propriedades de linha de bytes no CRXDE](assets/custom-component/byline-properties-crxde.png)

   */content/wknd/language-masters/en/Magazine/guide-la-skatepark/jcr:content/root/responsivegrid/responsivegrid/byline*

## Criar Modelo de Sling de Byline {#create-sling-model}

Em seguida, criaremos um Modelo Sling para agir como o modelo de dados e basear a lógica comercial do componente Byline.

Os modelos Sling são Java &quot;POJO&#39;s&quot; (objetos Java simples) orientados por anotações que facilitam o mapeamento de dados do JCR para variáveis Java e fornecem várias outras opções ao se desenvolver no contexto de AEM.

### Revisar Dependências Maven {#maven-dependency}

O Modelo Byline Sling dependerá de várias APIs Java fornecidas pela AEM. Essas APIs são disponibilizadas pelo `dependencies` listado no arquivo POM do módulo `core`.

1. Abra o arquivo `pom.xml` abaixo de `<src>/aem-guides-wknd/core/pom.xml`.
1. Localize a dependência para `uber-jar` na seção dependências do arquivo pom:

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   O [uber-jar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) contém todas as APIs Java públicas expostas pelo AEM. Observe que uma versão não está especificada no arquivo `core/pom.xml`. Em vez disso, a versão é mantida no pom de reator principal localizado na raiz do projeto `aem-guides-wknd/pom.xml`.

1. Encontre a dependência para `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   Todas as APIs Java públicas expostas pelos componentes principais AEM. AEM Componentes principais é um projeto mantido fora do AEM e, portanto, tem um ciclo de lançamento separado. Por isso, é uma dependência que precisa ser incluída separadamente e **não** está incluída no uber-jar.

   Como o uber-jar, a versão dessa dependência é mantida no arquivo pom do reator pai localizado em `aem-guides-wknd/pom.xml`.

   Mais tarde neste tutorial, usaremos a classe Core Component Image para exibir a imagem no componente Byline. É necessário ter a dependência do Componente Principal para criar e compilar nosso Modelo Sling.

### Interface byline {#byline-interface}

Crie uma interface Java pública para a Linha de identificação. `Byline.java` define os métodos públicos necessários para direcionar o script  `byline.html` HTL.

1. No módulo `aem-guides-wknd.core` abaixo de `src/main/java,` crie uma nova interface Java chamada `Byline.java` clicando com o botão direito do mouse no `com.adobe.aem.guides.wknd.core.models` **pacote > Novo > Interface**. Digite **Linha de identificação** como o Nome da interface e clique em Concluir.

   ![criar interface de linha de bytes](assets/custom-component/create-byline-interface.png)

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

   Os dois primeiros métodos expõem os valores para **name** e **ocupações** para o componente Byline.

   O método `isEmpty()` é usado para determinar se o componente tem algum conteúdo para renderizar ou se está aguardando a configuração.

   Observe que não há método para a imagem; [veremos por que isso é mais tarde](#tackling-the-image-problem).

### Implementação de byline {#byline-implementation}

`BylineImpl.java` é a implementação do Modelo Sling que implementa a  `Byline.java` interface definida anteriormente. O código completo para `BylineImpl.java` pode ser encontrado na parte inferior desta seção.

1. No módulo `core` abaixo de `src/main/java`, crie um novo arquivo de classe chamado **BylineImpl.java** clicando com o botão direito do mouse no pacote `com.adobe.aem.guides.wknd.core.models.impl` e selecionando **Novo > Classe**.

   Para o nome, digite **BylineImpl**. Adicione a **interface Byline** como uma interface de implementação.

   ![criar implementação de byline](assets/custom-component/create-byline-impl.png)

1. Abrir `BylineImpl.java`. Ele é preenchido automaticamente com todos os métodos definidos na interface `Byline.java`. Adicione as anotações do Modelo Sling atualizando `BylineImpl.java` com as seguintes anotações de nível de classe. Essa anotação `@Model(..)`é o que transforma a classe em um Modelo Sling.

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
       protected static final String RESOURCE_TYPE = "wknd/components/content/byline";
       ...
   }
   ```

   Vamos rever essa anotação e seus parâmetros:

   * A anotação `@Model` registra BylineImpl como um Modelo Sling quando é implantada no AEM.
   * O parâmetro `adaptables` especifica que esse modelo pode ser adaptado pela solicitação.
   * O parâmetro `adapters` permite que a classe de implementação seja registrada na interface de Linha de identificação. Isso permite que o script HTL chame o Modelo Sling pela interface (em vez do impl diretamente). [Mais detalhes sobre os adaptadores podem ser encontrados aqui](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * O `resourceType` aponta para o tipo de recurso do componente Byline (criado anteriormente) e ajuda a resolver o modelo correto se houver várias implementações. [Mais detalhes sobre como associar uma classe de modelo a um tipo de recurso podem ser encontrados aqui](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130).

### Implementação dos métodos do Modelo Sling {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

O primeiro método que abordaremos é `getName()` que simplesmente retorna o valor armazenado para o nó de conteúdo JCR da linha de bytes sob a propriedade `name`.

Para isso, a anotação `@ValueMapValue` do Modelo Sling é usada para inserir o valor em um campo Java usando o ValueMap do recurso da Solicitação.

```java
...
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
...
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

Como a propriedade JCR compartilha o mesmo nome do campo Java (ambos são &quot;nome&quot;), `@ValueMapValue` resolve automaticamente essa associação e injeta o valor da propriedade no campo Java.

#### getOccupations() {#implementing-get-occupations}

O próximo método a ser implementado é `getOccupations()`. Este método coleta todas as ocupações armazenadas na propriedade JCR `occupations` e retorna uma coleção classificada (alfabeticamente) delas.

Usando a mesma técnica explorada em `getName()`, o valor da propriedade pode ser inserido no campo do Modelo Sling.

Quando os valores de propriedade JCR estiverem disponíveis no Modelo Sling por meio do campo Java inserido `occupations`, a lógica comercial de classificação poderá ser aplicada no método `getOccupations()`.

```java
import java.util.ArrayList;
import java.util.Collections;
...

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private List<String> occupations;
    ...
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

O último método público é `isEmpty()` que determina quando o componente deve se considerar &quot;suficientemente criado&quot; para renderizar.

Para este componente, temos requisitos de negócios que indicam que todos os três campos, nome, imagem e ocupações devem ser preenchidos *antes de* que o componente possa ser renderizado.

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

#### Como solucionar o &quot;problema de imagem&quot; {#tackling-the-image-problem}

Marcar as condições de nome e ocupação são triviais (e o Apache Commons Lang3 fornece a classe sempre útil [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html)), no entanto, não está claro como a **presença da imagem** pode ser validada, pois o componente de Imagem dos Componentes Principais é usado para destacar a imagem.

Há duas maneiras de lidar com isso:

1. Verifique se a propriedade JCR `fileReference` resolve para um ativo.
1. Converta esse recurso em um Modelo principal de divisão de imagens de componentes e verifique se o método `getSrc()` não está vazio.

   Optaremos pela abordagem **second**. A primeira abordagem provavelmente é suficiente, mas neste tutorial o último será usado para permitir a exploração de outros recursos dos Modelos Sling.

1. Crie um método privado que obtenha a imagem. Este método é deixado privado porque não é necessário expor o objeto Image no próprio HTL e seu único método usado para direcionar `isEmpty().`

   O seguinte método particular para `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   Como observado acima, há mais duas abordagens para obter o **Modelo de divisão de imagens**:

   O primeiro usa a anotação `@Self` para adaptar automaticamente a solicitação atual ao `Image.class` do Componente Principal

   ```java
   @Self
   private Image image;
   ```

   O segundo usa o serviço OSGi [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html), que é um serviço muito útil, e ajuda a criar Modelos Sling de outros tipos no código Java.

   Optaremos pela segunda abordagem.

   >[!NOTE]
   >
   >Em uma implementação real, a abordagem &quot;Um&quot;, usando `@Self` é preferida, já que é a solução mais simples e elegante. Neste tutorial, usaremos a segunda abordagem, pois ela exige que exploremos mais aspectos dos Modelos Sling que são extremamente úteis são componentes mais complexos!

   Como os Modelos Sling são Java POJO&#39;s e não OSGi Services, as anotações de injeção OSGi habituais `@Reference` **não podem** ser usadas, em vez disso, os Modelos Sling fornecem uma anotação especial **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** que fornece funcionalidade semelhante.

1. Atualize `BylineImpl.java` para incluir a anotação `OSGiService` para injetar o `ModelFactory`:

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

   Com o `ModelFactory` disponível, um Modelo principal de divisão de imagens de componentes pode ser criado usando:

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   No entanto, esse método exige tanto uma solicitação quanto um recurso, ainda não disponível no Modelo Sling. Para obtê-los, mais anotações do Modelo de sling são usadas!

   Para obter a solicitação atual, a anotação **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** pode ser usada para injetar `adaptable` (que está definida em `@Model(..)` como `SlingHttpServletRequest.class`, em um campo de classe Java.

1. Adicione a anotação **@Self** para obter a solicitação **SlingHttpServletRequest**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   Lembre-se, usar `@Self Image image` para injetar o Modelo principal de divisão de imagens do componente era uma opção acima - a anotação `@Self` tenta injetar o objeto adaptável (no nosso caso um SlingHttpServletRequest) e adaptar-se ao tipo de campo de anotação. Como o Modelo Sling da imagem do componente principal é adaptável a partir de objetos SlingHttpServletRequest, isso teria funcionado e seria menos código do que nossa abordagem mais exploratória.

   Agora, inserimos as variáveis necessárias para instanciar nosso modelo de imagem por meio da API ModelFactory. Usaremos a anotação **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** do Modelo Sling para obter esse objeto após a instância do Modelo Sling.

   `@PostConstruct` é incrivelmente útil e atua em uma capacidade semelhante à de um construtor, no entanto, é chamado depois que a classe é instanciada e todos os campos Java anotados são inseridos. Enquanto outras anotações do Modelo Sling anotam campos de classe Java (variáveis), `@PostConstruct` anota um método de parâmetro void e zero, normalmente chamado `init()` (mas pode ser nomeado como qualquer coisa).

1. Adicione o método **@PostConstruct**:

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

   Lembre-se de que os Modelos Sling são **NOT** Serviços OSGi, portanto, é seguro manter o estado da classe. Geralmente `@PostConstruct` deriva e configura o estado da classe do Modelo Sling para uso posterior, semelhante ao que um construtor simples faz.

   Observe que se o método `@PostConstruct` lançar uma exceção, o Modelo Sling não instanciará (será nulo).

1. **getImage()** pode ser atualizado para simplesmente retornar o objeto de imagem.

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. Vamos voltar para `isEmpty()` e concluir a implementação:

   ```java
   @Override
   public boolean isEmpty() {
       ...
       } else if (getImage() == null || StringUtils.isBlank(getImage().getSrc())) {
           // A valid image is required
           return true;
       } else {
       ...
   }
   ```

   Observe que várias chamadas para `getImage()` não são problemáticas, pois retorna a variável de classe `image` inicializada e não chama `modelFactory.getModelFromWrappedRequest(...)` que não é um custo excessivo, mas vale a pena evitar chamar desnecessariamente.

1. O `BylineImpl.java` final deve ter a seguinte aparência:

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
       protected static final String RESOURCE_TYPE = "wknd/components/content/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       @PostConstruct
       private void init() {
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
           final Image image = getImage();
   
           if (StringUtils.isBlank(name)) {
               // Name is missing, but required
               return true;
           } else if (occupations == null || occupations.isEmpty()) {
               // At least one occupation is required
               return true;
           } else if (image == null || StringUtils.isBlank(image.getSrc())) {
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

## Byline HTL {#byline-htl}

No módulo `ui.apps`, abra `/apps/wknd/components/content/byline/byline.html` que criamos na configuração anterior do Componente AEM.

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

Vamos rever o que este script HTL faz até agora:

* O `placeholderTemplate` aponta para o espaço reservado dos Componentes principais, que é exibido quando o componente não está totalmente configurado. Isso é renderizado no AEM Sites Page Editor como uma caixa com o título do componente, conforme definido acima na propriedade `cq:Component` de `jcr:title`.

* O `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` carrega o `placeholderTemplate` definido acima e passa um valor booliano (atualmente codificado em `false`) para o modelo de espaço reservado. Quando `isEmpty` é verdadeiro, o modelo de espaço reservado renderiza a caixa cinza, caso contrário, não renderiza nada.

### Atualizar HTL do Byline

1. Atualize **byline.html** com a seguinte estrutura HTML esqueletal:

   ```xml
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!-- Include the Core Components Image Component -->
           </div>
           <h2 class="cmp-byline__name"><!-- Include the name --></h2>
           <p class="cmp-byline__occupations"><!-- Include the occupations --></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   Observe que as classes CSS seguem a [convenção de nomenclatura BEM](https://getbem.com/naming/). Embora o uso de convenções de BEM não seja obrigatório, a BEM é recomendada, pois é usada em classes CSS de componentes principais e geralmente resulta em regras CSS limpas e legíveis.

#### Instanciando objetos do Modelo Sling em HTL {#instantiating-sling-model-objects-in-htl}

A [Usar instrução de bloco](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) é usada para instanciar objetos do Modelo Sling no script HTL e atribuí-la a uma variável HTL.

`data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` usa a interface de Linha de identificação (com.adobe.aem.guides.wknd.models.Byline) implementada por BylineImpl e adapta o SlingHttpServletRequest atual a ela, e o resultado é armazenado em uma linha de byline de nome de variável HTL (  `data-sly-use.<variable-name>`).

1. Atualize o `div` externo para fazer referência ao **Modelo de sling** pela sua interface pública:

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

#### Acessar métodos do Modelo Sling {#accessing-sling-model-methods}

HTL recebe emprestado do JSTL e usa a mesma redução dos nomes dos métodos do getter Java.

Por exemplo, chamar o método `getName()` do Modelo de Sling de Byline pode ser encurtado para `byline.name`, da mesma forma em vez de `byline.isEmpty`, isso pode ser encurtado para `byline.empty`. Usando nomes completos de métodos, `byline.getName` ou `byline.isEmpty` também funciona. Observe que `()` nunca é usado para chamar métodos em HTL (semelhante ao JSTL).

Métodos Java que exigem um parâmetro **não podem** ser usados em HTL. Isso é feito para manter a lógica em HTL simples.

1. O nome da Linha de identificação pode ser adicionado ao componente chamando o método `getName()` no Modelo de Sling de Linha de identificação, ou em HTL: `${byline.name}`.

   Atualize a tag `h2`:

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

#### Usando Opções de Expressão HTL {#using-htl-expression-options}

[As opções de Expressões HTL ](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) atuam como modificadores no conteúdo em HTL e variam de formatação de data para conversão i18n. As expressões também podem ser usadas para unir listas ou matrizes de valores, o que é necessário para exibir as ocupações em um formato delimitado por vírgulas.

As expressões são adicionadas pelo operador `@` na expressão HTL.

1. Para associar-se à lista de ocupações com &quot;, &quot;, é usado o seguinte código:

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

#### Exibindo condicionalmente o espaço reservado {#conditionally-displaying-the-placeholder}

A maioria dos scripts HTL para AEM Componentes aproveita o **paradigma de espaço reservado** para fornecer uma dica visual aos autores **indicando que um componente foi criado incorretamente e não será exibido no AEM Publish**. A convenção para liderar esta decisão é implementar um método no modelo Sling de apoio do componente, no nosso caso: `Byline.isEmpty()`.

`isEmpty()` é chamado no Modelo de Sling de Linha Byte e o resultado (ou melhor, seu negativo, por meio do  `!` operador) é salvo em uma variável HTL chamada  `hasContent`:

1. Atualize o arquivo externo `div` para salvar uma variável HTL chamada `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   Observe o uso de `data-sly-test`, o bloco HTL `test` é interessante porque ele define uma variável HTL E renderiza/não renderiza o elemento HTML em que está, com base se o resultado da expressão HTL for verdadeiro ou não. Se &quot;verdadeiro&quot;, o elemento HTML será renderizado, caso contrário, ele não será renderizado.

   Essa variável HTL `hasContent` agora pode ser reutilizada para mostrar/ocultar condicionalmente o espaço reservado.

1. Atualize a chamada condicional para `placeholderTemplate` na parte inferior do arquivo com o seguinte:

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

#### Exibir a imagem usando os componentes principais {#using-the-core-components-image}

O script HTL para `byline.html` está agora quase completo e só falta a imagem.

```html
<!--/* current progress of byline.html */-->
<div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
     data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-test.hasContent="${!byline.empty}"
     class="cmp-byline">
    <div class="cmp-byline__image">
        <!-- Include the Core Components Image component -->
    </div>
    <h2 class="cmp-byline__name">${byline.name}</h2>
    <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
```

Como usamos `sling:resourceSuperType` o componente Imagem dos componentes principais para fornecer a criação da imagem, também podemos usar o componente Imagem do componente principal para renderizar a imagem!

Para isso, precisamos incluir o recurso de linha de bytes atual, mas forçar o tipo de recurso do componente de Imagem dos Componentes Principais, usando o tipo de recurso `core/wcm/components/image/v2/image`. Este é um padrão poderoso para a reutilização de componentes. Para isso, o bloco `data-sly-resource` de HTL é usado.

1. Substitua `div` por uma classe de `cmp-byline__image` pelo seguinte:

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   Esse `data-sly-resource`, incluía o recurso atual pelo caminho relativo `'.'` e força a inclusão do recurso atual (ou do recurso de conteúdo de linha de bytes) com o tipo de recurso `core/wcm/components/image/v2/image`.

   O tipo de recurso Componente principal é usado diretamente, e não por meio de um proxy, pois é um uso em script e nunca é persistente para nosso conteúdo.

2. Concluído `byline.html` abaixo:

   ```html
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
       class="cmp-byline">
       <div class="cmp-byline__image"
            data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }">
       </div>
           <h2 class="cmp-byline__name">${byline.name}</h2>
           <p class="cmp-byline__occupations">${byline.occupations @ join=','}</p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

3. Implante a base de código para uma instância AEM local. Como foram feitas alterações importantes nos arquivos POM, execute uma compilação Maven completa a partir do diretório raiz do projeto.

   >[!WARNING]
   >
   > Observe que o projeto WKND está configurado de modo que `ui.content` substitua quaisquer alterações no JCR; portanto, queremos garantir que implantemos apenas o projeto `ui.apps` para evitar a eliminação do componente Byline adicionado à página do artigo anteriormente.

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.apps
   $ mvn -PautoInstallPackage clean install
   ...
   Package imported.
   Package installed in 338ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

#### Revisando o componente Byline sem estilo {#reviewing-the-unstyled-byline-component}

1. Após implantar a atualização, navegue até a página [Guia Ultimate para LA Skatepark ](http://localhost:4502/editor.html/content/wknd/language-masters/en/magazine/guide-la-skateparks.html) ou onde você tiver adicionado o componente Byline anteriormente no capítulo.

1. As **image**, **name** e **ocupações** agora são exibidas e temos um componente de Linha de identificação sem estilo, mas funcionando.

   ![componente de linha de bytes sem estilo](assets/custom-component/unstyled.png)

#### Revisando o registro do Modelo Sling {#reviewing-the-sling-model-registration}

A visualização [AEM Status Models Sling Models](http://localhost:4502/system/console/status-slingmodels) do Console Web exibe todos os Modelos Sling registrados em AEM. O Modelo de divisão em linha Byline pode ser validado como sendo instalado e reconhecido revisando essa lista.

Se o **BylineImpl** não for exibido nessa lista, provavelmente houve um problema com as anotações do Modelo Sling ou o Modelo Sling não foi adicionado ao pacote de Modelos Sling registrados (com.adobe.aem.guides.wknd.core.models) no projeto principal.

![Modelo Byline Sling registrado](assets/custom-component/osgi-sling-models.png)

*http://localhost:4502/system/console/status-slingmodels*

## Estilos de byline {#byline-styles}

O componente de Linha de identificação precisa ser estilizado para alinhar com o design criativo do componente de Linha de identificação. Isso será feito usando o SCSS, que AEM oferece suporte para o subprojeto **ui.frontendent** Maven.

Depois de estilizar, o componente Linha de identificação deve adotar a estética a seguir.

![estilos de modelo de linha de bytes](./assets/custom-component/byline-design.png)

*Deslocar o design de componente conforme definido pela equipe de criação da WKND*

### Adicionar um estilo padrão

Adicione estilos padrão para o componente Linha de identificação. No projeto **ui.frontendent** em `/src/main/webpack/components/content`:

1. Crie uma nova pasta chamada `byline`.
1. Crie uma nova pasta abaixo da pasta `byline` chamada `scss`.
1. Crie um novo arquivo abaixo da pasta `byline/scss` chamada `byline.scss`.
1. Crie uma nova pasta abaixo da pasta `byline/scss` chamada `styles`.
1. Crie um novo arquivo abaixo da pasta `byline/scss/styles` chamada `default.scss`.

   ![explorador de projeto byline](assets/custom-component/byline-style-project-explorer.png)

1. Start preenchendo **byline.scss** para incluir o estilo padrão:

   ```scss
    /* WKND Byline styles */
   @import 'styles/default';
   ```

1. Adicione o CSS de implementações Byline (gravado como SCSS) no `default.scss`:

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
           font-size: $font-size-large;
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

1. Abra o arquivo `main.scss` no projeto **ui.front-end** em `/src/main/webpack/site` e adicione a seguinte linha na seção `/* Components */`:

   ```scss
   @import '../components/content/byline/scss/byline.scss';
   ```

1. Crie e compile o módulo `ui.frontend` usando o NPM:

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.frontend
    $ npm run dev
   ```

1. Crie e implante o projeto `ui.apps`, que incluirá transitoriamente o projeto `ui.frontend`, em uma instância AEM local usando Maven:

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.apps
    $ mvn clean install -PautoInstallPackage
   ```

   >[!TIP]
   >
   >Talvez seja necessário limpar o cache do navegador para garantir que o CSS obsoleto não esteja sendo atendido e atualizar a página com o componente de Linha de identificação para obter o estilo completo.

## Juntando {#putting-it-together}

Abaixo está a aparência do componente Linha de identificação totalmente criado e estilizado na página AEM.

![componente de byline finalizado](assets/custom-component/final-byline-component.png)

Assista ao vídeo abaixo para uma rápida apresentação do que foi criado neste tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/30174/?quality=12&learn=on)

## Parabéns! {#congratulations}

Parabéns, você acabou de criar um componente personalizado do zero usando o Adobe Experience Manager!

### Próximas etapas {#next-steps}

Continue a aprender sobre AEM desenvolvimento de componentes explorando como gravar testes JUnit para o código Java Byline para garantir que tudo seja desenvolvido corretamente e que a lógica comercial implementada esteja correta e completa.

* [Gravando testes de unidade ou componentes AEM](unit-testing.md)

Visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente no bloco Git `custom-component/solution`.

1. Clique no repositório [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd).
1. Confira a ramificação `custom-component/solution`

## Resolução de problemas {#troubleshooting}

### Pastas de origem ausentes

Se você não vir a pasta de origem `src/main/java` no Eclipse, poderá adicionar as pastas clicando com o botão direito do mouse em src e adicionando pastas para principal e java. Depois de adicionar as pastas, você deverá ver o pacote `src/main/java` exibido.

### Pacotes não resolvidos

![solução de problemas de pacotes não resolvidos](assets/custom-component/troubleshoot-unresolved-packages.png)

>[!NOTE]
>
> Se você tiver importações de pacote não resolvidas para algumas das novas dependências adicionadas ao projeto principal, tente atualizar o projeto aem-guides-wknd maven, que por sua vez atualizará todos os subprojetos. Para fazer isso, clique com o botão direito do mouse em **aem-guides-wknd > Maven > Atualizar projeto**.
