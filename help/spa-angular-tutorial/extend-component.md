---
title: Estender um componente | Introdução ao AEM SPA Editor e Angular
description: Saiba como estender um Componente principal existente para ser usado com o AEM SPA Editor. Entender como adicionar propriedades e conteúdo a um componente existente é uma técnica poderosa para expandir os recursos de uma implementação do Editor de SPA AEM. Saiba como usar o padrão de delegação para estender os Modelos Sling e os recursos da Sling Resource Fusão.
sub-product: sites
feature: Editor SPA
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5871
thumbnail: 5871-spa-angular.jpg
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '1986'
ht-degree: 2%

---


# Estender um componente principal {#extend-component}

Saiba como estender um Componente principal existente para ser usado com o AEM SPA Editor. Entender como estender um componente existente é uma técnica poderosa para personalizar e expandir os recursos de uma implementação do Editor de SPA AEM.

## Objetivo

1. Estenda um componente principal existente com propriedades e conteúdo adicionais.
2. Entenda o básico da Herança de componentes com o uso de `sling:resourceSuperType`.
3. Saiba como aproveitar o [Padrão de delegação](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) para Modelos Sling para reutilizar a lógica e a funcionalidade existentes.

## O que você vai criar

Neste capítulo, um novo componente `Card` será criado. O componente `Card` estenderá o [Componente principal de imagem](https://docs.adobe.com/content/help/br/experience-manager-core-components/using/components/image.html) adicionando campos de conteúdo adicionais, como um Título e um botão de Ação Chamada, para executar a função de um teaser para outro conteúdo dentro do SPA.

![Criação final do componente de cartão](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> Em uma implementação real, pode ser mais apropriado usar o [Teaser Component](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/teaser.html) e estender o [Image Core Component](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html) para fazer um componente `Card` dependendo dos requisitos do projeto. Sempre é recomendável usar [Componentes principais](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/introduction.html) diretamente quando possível.

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Obter o código

1. Baixe o ponto de partida para este tutorial via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/extend-component-start
   ```

2. Implante a base de código para uma instância AEM local usando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se estiver usando [AEM 6.x](overview.md#compatibility) adicione o perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Instale o pacote finalizado para o [site de referência WKND tradicional](https://github.com/adobe/aem-guides-wknd/releases/latest). As imagens fornecidas pelo [site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) serão reutilizadas no SPA WKND. O pacote pode ser instalado usando [AEM Gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp).

   ![O gerenciador de pacotes instala wknd.all](./assets/map-components/package-manager-wknd-all.png)

Você sempre pode visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) ou fazer check-out do código localmente ao alternar para a ramificação `Angular/extend-component-solution`.

## Implementação inicial da placa Inspect

Um componente de cartão inicial foi fornecido pelo código inicial do capítulo. Inspect o ponto de partida para a implementação do cartão.

1. No IDE de sua escolha, abra o módulo `ui.apps`.
2. Navegue até `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/card` e visualização o arquivo `.content.xml`.

   ![Start de definição AEM componente de placa](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-angular/components/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   A propriedade `sling:resourceSuperType` aponta para `wknd-spa-angular/components/image`, indicando que o componente `Card` herdará toda a funcionalidade do componente de Imagem SPA WKND.

3. Inspect o arquivo `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   Observe que `sling:resourceSuperType` aponta para `core/wcm/components/image/v2/image`. Isso indica que o componente WKND SPA Image herda toda a funcionalidade da Imagem do componente principal.

   Também conhecido como [Padrão de proxy](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) A herança de recursos Sling é um padrão de design avançado para permitir que os componentes filhos herdem a funcionalidade e estendam/substituam o comportamento quando desejado. A herança Sling suporta vários níveis de herança, portanto, em última análise, o novo componente `Card` herda a funcionalidade da Imagem do Componente Principal.

   Muitas equipes de desenvolvimento se esforçam para ser DRY. (não se repita). A herança de sling torna isso possível com a AEM.

4. Abaixo da pasta `card`, abra o arquivo `_cq_dialog/.content.xml`.

   Esse arquivo é a definição da caixa de diálogo Componente para o componente `Card`. Se estiver usando a herança Sling, é possível usar os recursos de [Fusão de recursos Sling](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/sling-resource-merger.html) para substituir ou estender partes da caixa de diálogo. Nessa amostra, uma nova guia foi adicionada à caixa de diálogo para capturar dados adicionais de um autor para preencher o Componente de cartão.

   Propriedades como `sling:orderBefore` permitem que um desenvolvedor escolha onde inserir novas guias ou campos de formulário. Nesse caso, a guia `Text` será inserida antes da guia `asset`. Para utilizar plenamente a Fusão de Recursos Sling, é importante saber a estrutura original do nó de diálogo para a caixa de diálogo [do componente de imagem](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml).

5. Abaixo da pasta `card`, abra o arquivo `_cq_editConfig.xml`. Esse arquivo determina o comportamento de arrastar e soltar na interface de criação do AEM. Ao estender o componente de Imagem, é importante que o tipo de recurso corresponda ao próprio componente. Revise o nó `<parameters>`:

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-angular/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   A maioria dos componentes não requer `cq:editConfig`, os descendentes de Imagem e filho do componente de Imagem são exceções.

6. No switch IDE para o módulo `ui.frontend`, navegando até `ui.frontend/src/app/components/card`:

   ![Start do componente angular](assets/extend-component/angular-card-component-start.png)

7. Inspect o arquivo `card.component.ts`.

   O componente já foi posicionado para mapear para o componente AEM `Card` usando a função padrão `MapTo`.

   ```js
   MapTo('wknd-spa-angular/components/card')(CardComponent, CardEditConfig);
   ```

   Examine os três parâmetros `@Input` na classe para `src`, `alt` e `title`. Esses são valores JSON esperados do componente AEM que serão mapeados para o componente Angular.

8. Abra o arquivo `card.component.html`:

   ```html
   <div class="card"  *ngIf="hasContent">
       <app-image class="card__image" [src]="src" [alt]="alt" [title]="title"></app-image>
   </div>
   ```

   Neste exemplo, optamos por reutilizar o componente Imagem do Angular existente `app-image` simplesmente transmitindo os parâmetros `@Input` de `card.component.ts`. Mais tarde no tutorial, outras propriedades serão adicionadas e exibidas.

## Atualizar a Política de Modelo

Com essa implementação inicial `Card`, reveja a funcionalidade no Editor de SPA AEM. Para ver o componente `Card` inicial, é necessário atualizar a política de modelo.

1. Implante o código inicial para uma instância local do AEM, caso ainda não tenha:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navegue até o Modelo de página SPA em [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Atualize a política de Container de layout para adicionar o novo componente `Card` como um componente permitido:

   ![Atualizar política de Container de layout](assets/extend-component/card-component-allowed.png)

   Salve as alterações na política e observe o componente `Card` como um componente permitido:

   ![Componente de cartão como um componente permitido](assets/extend-component/card-component-allowed-layout-container.png)

## Componente de cartão inicial do autor

Em seguida, crie o componente `Card` usando o Editor de SPA AEM.

1. Navegue até [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. No modo `Edit`, adicione o componente `Card` ao `Layout Container`:

   ![Inserir novo componente](assets/extend-component/insert-custom-component.png)

3. Arraste e solte uma imagem do Localizador de ativos no componente `Card`:

   ![Adicionar imagem](assets/extend-component/card-add-image.png)

4. Abra a caixa de diálogo `Card` do componente e observe a adição de uma guia **Texto**.
5. Digite os seguintes valores na guia **Texto**:

   ![Guia Componente de texto](assets/extend-component/card-component-text.png)

   **Caminho**  do cartão - escolha uma página abaixo da página inicial SPA.

   **Texto**  CTA - &quot;Leia mais&quot;

   **Título**  do cartão - deixe em branco

   **Obter título da página**  vinculada - marque a caixa de seleção para indicar true.

6. Atualize a guia **Metadados do ativo** para adicionar valores para **Texto alternativo** e **Legenda**.

   No momento, nenhuma alteração adicional é exibida após a atualização da caixa de diálogo. Para expor os novos campos ao Componente do Angular, precisamos atualizar o Modelo Sling para o componente `Card`.

7. Abra uma nova guia e navegue até [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-angular/us/en/home/jcr%3Acontent/root/responsivegrid/card). Inspect os nós de conteúdo abaixo de `/content/wknd-spa-angular/us/en/home/jcr:content/root/responsivegrid` para localizar o conteúdo do componente `Card`.

   ![Propriedades de componentes CRXDE-Lite](assets/extend-component/crxde-lite-properties.png)

   Observe que as propriedades `cardPath`, `ctaText`, `titleFromPage` são mantidas pela caixa de diálogo.

## Atualizar modelo Sling da placa

Para expor os valores da caixa de diálogo do componente ao componente do Angular, precisamos atualizar o Modelo Sling que preenche o JSON para o componente `Card`. Também temos a oportunidade de implementar duas lógicas de negócios:

* Se `titleFromPage` para **true**, retorne o título da página especificada por `cardPath`, caso contrário, retornará o valor de `cardTitle` campo de texto.
* Retorna a última data modificada da página especificada por `cardPath`.

Retorne ao IDE de sua escolha e abra o módulo `core`.

1. Abra o arquivo `Card.java` em `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/Card.java`.

   Observe que a interface `Card` estende atualmente `com.adobe.cq.wcm.core.components.models.Image` e, portanto, herda todos os métodos da interface `Image`. A interface `Image` já estende a interface `ComponentExporter`, que permite que o Modelo Sling seja exportado como JSON e mapeado pelo editor de SPA. Portanto, não precisamos estender explicitamente a interface `ComponentExporter` como fizemos no [capítulo de Componente personalizado](custom-component.md).

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

   Esses métodos serão expostos pela API do modelo JSON e passados para o componente do Angular.

3. Abrir `CardImpl.java`. Esta é a implementação da interface `Card.java`. Essa implementação já foi parcialmente adiada para acelerar o tutorial.  Observe o uso das anotações `@Model` e `@Exporter` para garantir que o Modelo Sling possa ser serializado como JSON por meio do Exportador de Modelo Sling.

   `CardImpl.java` também usa o padrão de  [Delegação para ](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) Modelos Sling para evitar a regravação de toda a lógica do componente principal da Imagem.

4. Observe as seguintes linhas:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   A anotação acima instanciará um objeto de Imagem chamado `image` com base na herança `sling:resourceSuperType` do componente `Card`.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   É possível simplesmente usar o objeto `image` para implementar métodos definidos pela interface `Image`, sem precisar gravar a lógica por conta própria. Essa técnica é usada para `getSrc()`, `getAlt()` e `getTitle()`.

5. Em seguida, implemente o método `initModel()` para iniciar uma variável particular `cardPage` com base no valor de `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   O `@PostConstruct initModel()` sempre será chamado quando o Modelo Sling for inicializado, portanto, é uma boa oportunidade para inicializar objetos que podem ser usados por outros métodos no modelo. `pageManager` é um entre vários [objetos globais com suporte Java](https://docs.adobe.com/content/help/en/experience-manager-htl/using/htl/global-objects.html#java-backed-objects) disponibilizados para Modelos Sling por meio da anotação `@ScriptVariable`. O método [getPage](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-) assume um caminho e retorna um objeto AEM [Page](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/Page.html) ou nulo se o caminho não apontar para uma página válida.

   Isso inicializará a variável `cardPage`, que será usada pelos outros novos métodos para retornar dados sobre a página vinculada subjacente.

6. Revise as variáveis globais já mapeadas para as propriedades do JCR salvas na caixa de diálogo do autor. A anotação `@ValueMapValue` é usada para executar automaticamente o mapeamento.

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

   Essas variáveis serão usadas para implementar os métodos adicionais para a interface `Card.java`.

7. Implemente os métodos adicionais definidos na interface `Card.java`:

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
   > Você pode visualização o [CardImpl.java finalizado aqui](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CardImpl.java).

8. Abra uma janela de terminal e implante apenas as atualizações no módulo `core` usando o perfil Maven `autoInstallBundle` do diretório `core`.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   Se estiver usando [AEM 6.x](overview.md#compatibility), adicione o perfil `classic`.

9. Visualização a resposta do modelo JSON em: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) e procure `wknd-spa-angular/components/card`:

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

   Observe que o modelo JSON é atualizado com pares de chave/valor adicionais depois de atualizar os métodos no `CardImpl` Modelo Sling.

## Atualizar componente do Angular

Agora que o modelo JSON está preenchido com novas propriedades para `ctaLinkURL`, `ctaText`, `cardTitle` e `cardLastModified`, podemos atualizar o componente de Angular para exibi-los.

1. Retorne ao IDE e abra o módulo `ui.frontend`. Opcionalmente, start o servidor dev de webpack de uma nova janela de terminal para ver as alterações em tempo real:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. Abra `card.component.ts` em `ui.frontend/src/app/components/card/card.component.ts`. Adicione as anotações adicionais `@Input` para capturar o novo modelo:

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

3. Adicione métodos para verificar se a Chamada para Ação está pronta e para retornar uma string de data/hora com base na entrada `cardLastModified`:

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

4. Abra `card.component.html` e adicione a marcação a seguir para exibir o título, a chamada para a ação e a data da última modificação:

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

   As regras Sass já foram adicionadas em `card.component.scss` para criar o estilo do título, chamar para ação e data da última modificação.

   >[!NOTE]
   >
   > Você pode visualização o código [do componente da placa de Angular finalizado aqui](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution/ui.frontend/src/app/components/card).

5. Implante as alterações completas no AEM da raiz do projeto usando o Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

6. Navegue até [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html) para ver o componente atualizado:

   ![Atualização do componente de cartão no AEM](assets/extend-component/updated-card-in-aem.png)

7. Você deve ser capaz de recriar o conteúdo existente para criar uma página semelhante ao seguinte:

   ![Criação final do componente de cartão](assets/extend-component/final-authoring-card.png)

## Parabéns! {#congratulations}

Parabéns, você aprendeu a estender um componente AEM usando o modelo e como os Modelos Sling e as caixas de diálogo funcionam com o modelo JSON.

Você sempre pode visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/extend-component-solution) ou fazer check-out do código localmente ao alternar para a ramificação `Angular/extend-component-solution`.
