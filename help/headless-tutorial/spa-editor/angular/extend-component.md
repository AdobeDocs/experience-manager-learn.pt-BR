---
title: Estender um componente | Introdução ao AEM SPA Editor e Angular
description: Saiba como estender um Componente principal existente a ser usado com o Editor de SPA de AEM. Entender como adicionar propriedades e conteúdo a um componente existente é uma técnica avançada para expandir os recursos de uma implementação do Editor de SPA AEM. Saiba como usar o padrão de delegação para estender Modelos do Sling e recursos do Sling Resource Merger.
sub-product: sites
feature: SPA Editor, Core Components
doc-type: tutorial
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 5871
thumbnail: 5871-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 0265d3df-3de8-4a25-9611-ddf73d725f6e
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1935'
ht-degree: 2%

---

# Estender um componente principal {#extend-component}

Saiba como estender um Componente principal existente a ser usado com o Editor de SPA de AEM. Entender como estender um componente existente é uma técnica poderosa para personalizar e expandir os recursos de uma implementação AEM SPA Editor.

## Objetivo

1. Estenda um Componente principal existente com propriedades e conteúdo adicionais.
2. Entenda os fundamentos da Herança de componentes com o uso de `sling:resourceSuperType`.
3. Saiba como usar o [Padrão de delegação](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) para Modelos do Sling para reutilizar a lógica e funcionalidade existentes.

## O que você vai criar

Neste capítulo, um novo `Card` é criado. O `Card` estende o [Componente principal da imagem](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=pt-BR) adicionar campos de conteúdo adicionais, como um Título e um botão de Ação de chamada para executar a função de um teaser para outro conteúdo dentro do SPA.

![Criação final do componente de cartão](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> Em uma implementação real, pode ser mais apropriado simplesmente usar a variável [Componente Teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/teaser.html) do que a extensão do [Componente principal da imagem](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) para criar um `Card` , dependendo dos requisitos do projeto. É sempre recomendável usar [Componentes principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR) diretamente quando possível.

## Pré-requisitos

Revise as ferramentas necessárias e as instruções para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Obter o código

1. Baixe o ponto de partida para este tutorial via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/extend-component-start
   ```

2. Implante a base de código em uma instância de AEM local usando o Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se estiver usando [AEM 6.x](overview.md#compatibility) adicione o `classic` perfil:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Instale o pacote concluído para o pacote tradicional [Site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0). As imagens fornecidas por [Site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) é reutilizada no SPA WKND. O pacote pode ser instalado usando [Gerenciador de pacotes de AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![O Gerenciador de Pacotes instala o wknd.all](./assets/map-components/package-manager-wknd-all.png)

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) ou faça check-out do código localmente, alternando para a ramificação `Angular/extend-component-solution`.

## Implementação da placa inicial do Inspect

Um componente de cartão inicial foi fornecido pelo código inicial do capítulo. Inspect é o ponto de partida para a implementação do cartão.

1. No IDE de sua escolha, abra o `ui.apps` módulo.
2. Navegar para `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card` e visualize o `.content.xml` arquivo.

   ![Início da definição de AEM do componente de cartão](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   A propriedade `sling:resourceSuperType` pontos a `wknd-spa-angular/components/image` indicando que `Card` herda a funcionalidade do componente SPA Imagem WKND.

3. Inspect o arquivo `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   Observe que a variável `sling:resourceSuperType` pontos a `core/wcm/components/image/v2/image`. Isso indica que o componente WKND SPA Imagem herda a funcionalidade da Imagem do componente principal.

   Também conhecido como [Padrão de proxy](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) A herança de recursos Sling é um padrão de design avançado para permitir que os componentes filho herdem a funcionalidade e estendam/substituam o comportamento quando desejado. A herança Sling suporta vários níveis de herança, portanto, em última análise, o novo `Card` herda a funcionalidade da Imagem do componente principal.

   Muitas equipes de desenvolvimento se esforçam para ser D.R.Y. (não se repitam). A herança do Sling possibilita isso com o AEM.

4. Abaixo da `card` , abra o arquivo `_cq_dialog/.content.xml`.

   Esse arquivo é a definição da caixa de diálogo Componente para a variável `Card` componente. Se estiver usando a herança Sling, é possível usar os recursos do [Fusão de Recursos Sling](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html?lang=pt-BR) para substituir ou estender partes da caixa de diálogo. Neste exemplo, uma nova guia foi adicionada à caixa de diálogo para capturar dados adicionais de um autor para preencher o Componente de cartão.

   Propriedades como `sling:orderBefore` permita que um desenvolvedor escolha onde inserir novas guias ou campos de formulário. Nesse caso, a variável `Text` é inserida antes da variável `asset` guia . Para utilizar plenamente o Sling Resource Merger, é importante conhecer a estrutura original do nó do diálogo para o [Caixa de diálogo do componente de imagem](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml).

5. Abaixo da `card` , abra o arquivo `_cq_editConfig.xml`. Esse arquivo determina o comportamento de arrastar e soltar na interface do usuário de criação de AEM. Ao estender o componente de Imagem, é importante que o tipo de recurso corresponda ao próprio componente. Revise o `<parameters>` nó:

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   A maioria dos componentes não requer um `cq:editConfig`, a Imagem e os descendentes filhos do componente Imagem são exceções.

6. No switch IDE para a `ui.frontend` módulo, navegando até `ui.frontend/src/app/components/card`:

   ![Início do componente Angular](assets/extend-component/angular-card-component-start.png)

7. Inspect o arquivo `card.component.ts`.

   O componente já foi preparado para mapear para o AEM `Card` Componente que usa o padrão `MapTo` .

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   Revise os três `@Input` parâmetros na classe para `src`, `alt`e `title`. Esses são valores JSON esperados do componente AEM que são mapeados para o componente Angular.

8. Abra o arquivo `card.component.html`:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   Neste exemplo, escolhemos reutilizar o componente Imagem do Angular existente `app-image` simplesmente transmitindo a variável `@Input` parâmetros de `card.component.ts`. Posteriormente no tutorial, outras propriedades são adicionadas e exibidas.

## Atualizar a Política de Modelo

Com esta `Card` implementação revise a funcionalidade no Editor de SPA de AEM. Para ver a `Card` é necessário atualizar a política Modelo.

1. Implante o código inicial em uma instância local do AEM, caso ainda não tenha:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navegue até o modelo de página de SPA em [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Atualize a política do Contêiner de layout para adicionar o novo `Card` componente como um componente permitido:

   ![Atualizar política do contêiner de layout](assets/extend-component/card-component-allowed.png)

   Salve as alterações na política e observe o `Card` componente como um componente permitido:

   ![Componente de cartão como um componente permitido](assets/extend-component/card-component-allowed-layout-container.png)

## Componente de cartão inicial do autor

Em seguida, crie o `Card` usando o Editor de SPA AEM.

1. Navegar para [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. Em `Edit` , adicione o `Card` para o `Layout Container`:

   ![Inserir novo componente](assets/extend-component/insert-custom-component.png)

3. Arraste e solte uma imagem do Localizador de ativos na `Card` componente:

   ![Adicionar imagem](assets/extend-component/card-add-image.png)

4. Abra o `Card` e observe a adição de um **Texto** Guia.
5. Insira os seguintes valores no **Texto** guia :

   ![Guia Componente de texto](assets/extend-component/card-component-text.png)

   **Caminho do cartão** - escolha uma página abaixo da página inicial do SPA.

   **Texto CTA** - &quot;Leia mais&quot;

   **Título do cartão** - deixar em branco

   **Obter título da página vinculada** - marque a caixa de seleção para indicar true.

6. Atualize o **Metadados de ativos** para adicionar valores para **Texto alternativo** e **Legenda**.

   No momento, nenhuma alteração adicional é exibida após a atualização da caixa de diálogo. Para expor os novos campos ao Componente do Angular, precisamos atualizar o Modelo do Sling para o `Card` componente.

7. Abra uma nova guia e acesse [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card). Inspect os nós de conteúdo abaixo `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` para encontrar o `Card` conteúdo do componente.

   ![Propriedades do componente CRXDE-Lite](assets/extend-component/crxde-lite-properties.png)

   Observe as propriedades `cardPath`, `ctaText`, `titleFromPage` são persistentes pela caixa de diálogo.

## Atualizar modelo de sling de placa

Para expor os valores da caixa de diálogo do componente ao componente do Angular, precisamos atualizar o Modelo do Sling que preenche o JSON para a variável `Card` componente. Também temos a oportunidade de implementar duas lógicas de negócios:

* If `titleFromPage` para **true**, retorna o título da página especificado por `cardPath` caso contrário, retorne o valor de `cardTitle` campo de texto.
* Retorna a data da última modificação da página especificada por `cardPath`.

Retorne ao IDE de sua escolha e abra o `core` módulo.

1. Abra o arquivo `Card.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`.

   Observe que a variável `Card` a interface atualmente estende `com.adobe.cq.wcm.core.components.models.Image` e, portanto, herda os métodos do `Image` interface. O `Image` a interface do já estende a `ComponentExporter` interface que permite que o Modelo do Sling seja exportado como JSON e mapeado pelo editor de SPA. Por conseguinte, não precisamos de alargar explicitamente `ComponentExporter` como fizemos na [Capítulo do Componente personalizado](custom-component.md).

2. Adicione os seguintes métodos à interface:

   ```java
   @ProviderType
   public interface Card extends Image {
   
       /***
       * The URL to populate the CTA button as part of the card.
       * The link should be based on the cardPath property that points to a page.
       * @return String URL
       */
       public String getCtaLinkURL();
   
       /***
       * The text to display on the CTA button of the card.
       * @return String CTA text
       */
       public String getCtaText();
   
   
   
       /***
       * The date to be displayed as part of the card.
       * This is based on the last modified date of the page specified by the cardPath
       * @return
       */
       public Calendar getCardLastModified();
   
   
       /**
       * Return the title of the page specified by cardPath if `titleFromPage` is set to true.
       * Otherwise return the value of `cardTitle`
       * @return
       */
       public String getCardTitle();
   }
   ```

   Esses métodos são expostos por meio da API do modelo JSON e passados para o componente do Angular.

3. Abrir `CardImpl.java`. Esta é a implementação da `Card.java` interface. Essa implementação foi parcialmente adiada para acelerar o tutorial.  Observe a utilização do `@Model` e `@Exporter` Anotações para garantir que o Modelo do Sling possa ser serializado como JSON por meio do Exportador de Modelo do Sling.

   `CardImpl.java` também usa a variável [Padrão de delegação para modelos Sling](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) para evitar reescrever a lógica do Componente principal de imagem.

4. Observe as seguintes linhas:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   A anotação acima instancia um objeto de Imagem chamado `image` com base no `sling:resourceSuperType` herança da `Card` componente.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   Assim, é possível simplesmente usar a variável `image` objeto para implementar métodos definidos pela variável `Image` interface, sem ter que escrever a lógica. Essa técnica é usada para `getSrc()`, `getAlt()`e `getTitle()`.

5. Em seguida, implemente o `initModel()` método para iniciar uma variável privada `cardPage` com base no valor de `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   O `@PostConstruct initModel()` é chamado quando o Modelo do Sling é inicializado, portanto, é uma boa oportunidade para inicializar objetos que podem ser usados por outros métodos no modelo. O `pageManager` é um de vários [Objetos globais com suporte para Java™](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html) disponibilizado para Modelos do Sling por meio do `@ScriptVariable` anotação. O [getPage](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) O método assume um caminho e retorna um AEM [Página](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) objeto ou nulo se o caminho não apontar para uma página válida.

   Isso inicializa o `cardPage` , que é usada pelos outros novos métodos para retornar dados sobre a página vinculada subjacente.

6. Revise as variáveis globais já mapeadas para as propriedades do JCR que salvaram a caixa de diálogo do autor. O `@ValueMapValue` a anotação é usada para executar o mapeamento automaticamente.

   ```java
   @ValueMapValue
   private String cardPath;
   
   @ValueMapValue
   private String ctaText;
   
   @ValueMapValue
   private boolean titleFromPage;
   
   @ValueMapValue
   private String cardTitle;
   ```

   Essas variáveis são usadas para implementar métodos adicionais para a variável `Card.java` interface.

7. Implemente os métodos adicionais definidos na `Card.java` interface:

   ```java
   @Override
   public String getCtaLinkURL() {
       if(cardPage != null) {
           return cardPage.getPath() + ".html";
       }
       return null;
   }
   
   @Override
   public String getCtaText() {
       return ctaText;
   }
   
   @Override
   public Calendar getCardLastModified() {
      if(cardPage != null) {
          return cardPage.getLastModified();
      }
      return null;
   }
   
   @Override
   public String getCardTitle() {
       if(titleFromPage) {
           return cardPage != null ? cardPage.getTitle() : null;
       }
       return cardTitle;
   }
   ```

   >[!NOTE]
   >
   > Você pode visualizar o [terminou CardImpl.java aqui](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java).

8. Abra uma janela de terminal e implante apenas as atualizações na `core` módulo que usa o Maven `autoInstallBundle` do `core` diretório.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   Se estiver usando [AEM 6.x](overview.md#compatibility) adicione o `classic` perfil.

9. Visualize a resposta do modelo JSON em: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) e pesquise a `wknd-spa-angular/components/card`:

   ```json
   "card": {
       "ctaText": "Read More",
       "cardTitle": "Page 1",
       "title": "Woman chillaxing with river views in Australian bushland",
       "src": "/content/wknd-spa-angular/us/en/home/_jcr_content/root/responsivegrid/card.coreimg.jpeg/1595190732886/adobestock-216674449.jpeg",
       "alt": "Female sitting on a large rock relaxing in afternoon dappled light the Australian bushland with views over the river",
       "cardLastModified": 1591360492414,
       "ctaLinkURL": "/content/wknd-spa-angular/us/en/home/page-1.html",
       ":type": "wknd-spa-angular/components/card"
   }
   ```

   Observe que o modelo JSON é atualizado com outros pares de chave/valor após a atualização dos métodos no `CardImpl` Modelo Sling.

## Atualizar componente do Angular

Agora que o modelo JSON é preenchido com novas propriedades para `ctaLinkURL`, `ctaText`, `cardTitle`e `cardLastModified` podemos atualizar o componente Angular para exibi-los.

1. Retorne ao IDE e abra o `ui.frontend` módulo. Opcionalmente, inicie o servidor de desenvolvimento do webpack a partir de uma nova janela de terminal para ver as alterações em tempo real:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. Abrir `card.component.ts` at `ui.frontend/src/app/components/card/card.component.ts`. Adicione o `@Input` anotações para capturar o novo modelo:

   ```diff
   export class CardComponent implements OnInit {
   
        @Input() src: string;
        @Input() alt: string;
        @Input() title: string;
   +    @Input() cardTitle: string;
   +    @Input() cardLastModified: number;
   +    @Input() ctaLinkURL: string;
   +    @Input() ctaText: string;
   ```

3. Adicione métodos para verificar se a Chamada à ação está pronta e para retornar uma string de data/hora com base na variável `cardLastModified` entrada:

   ```js
   export class CardComponent implements OnInit {
       ...
       get hasCTA(): boolean {
           return this.ctaLinkURL && this.ctaLinkURL.trim().length > 0 && this.ctaText && this.ctaText.trim().length > 0;
       }
   
       get lastModifiedDate(): string {
           const lastModifiedDate = this.cardLastModified ? new Date(this.cardLastModified) : null;
   
           if (lastModifiedDate) {
           return lastModifiedDate.toLocaleDateString();
           }
           return null;
       }
       ...
   }
   ```

4. Abrir `card.component.html` e adicione a seguinte marcação para exibir o título, a chamada para a ação e a data da última modificação:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
       <div class="card__content">
           <h2 class="card__title">
               {{cardTitle}}
               <span class="card__lastmod" *ngIf="lastModifiedDate">{{lastModifiedDate}}</span>
           </h2>
           <div class="card__action-container" *ngIf="hasCTA">
               <a [routerLink]="ctaLinkURL" class="card__action-link" [title]="ctaText">
                   {{ctaText}}
               </a>
           </div>
       </div>
   </div>
   ```

   As regras de Sass já foram adicionadas em `card.component.scss` para criar estilo no título, chame para a ação e data da última modificação.

   >[!NOTE]
   >
   > Você pode visualizar o valor concluído [Código do componente da placa de angular aqui](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card).

5. Implante as alterações completas no AEM da raiz do projeto usando o Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. Navegar para [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) para ver o componente atualizado:

   ![Componente de cartão atualizado em AEM](assets/extend-component/updated-card-in-aem.png)

7. É possível recriar o conteúdo existente para criar uma página semelhante ao seguinte:

   ![Criação final do componente de cartão](assets/extend-component/final-authoring-card.png)

## Parabéns.  {#congratulations}

Parabéns, você aprendeu a estender um componente de AEM e como os Modelos e diálogos do Sling funcionam com o modelo JSON.

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) ou faça check-out do código localmente, alternando para a ramificação `Angular/extend-component-solution`.
