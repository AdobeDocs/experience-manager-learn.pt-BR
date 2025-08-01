---
title: Mapear componentes de SPA para componentes do AEM | Introdução ao AEM SPA Editor e Angular
description: Saiba como mapear componentes do Angular para componentes do Adobe Experience Manager (AEM) com o AEM SPA Editor JS SDK. O mapeamento de componentes permite que os usuários façam atualizações dinâmicas em componentes SPA no Editor SPA do AEM, de modo semelhante à criação tradicional no AEM.
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-5311
thumbnail: 5311-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 19a8917c-a1e7-4293-9ce1-9f4c1a565861
duration: 509
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '2213'
ht-degree: 0%

---

# Mapear componentes de SPA para componentes do AEM {#map-components}

{{spa-editor-deprecation}}

Saiba como mapear componentes do Angular para componentes do Adobe Experience Manager (AEM) com o AEM SPA Editor JS SDK. O mapeamento de componentes permite que os usuários façam atualizações dinâmicas em componentes SPA no Editor SPA do AEM, de modo semelhante à criação tradicional no AEM.

Este capítulo detalha a API do modelo JSON do AEM e mostra como o conteúdo JSON exposto por um componente do AEM pode ser inserido automaticamente em um componente do Angular como props.

## Objetivo

1. Saiba como mapear componentes do AEM para componentes SPA.
2. Entenda a diferença entre os componentes de **Contêiner** e os componentes de **Conteúdo**.
3. Crie um novo componente do Angular que mapeie para um componente existente do AEM.

## O que você vai criar

Este capítulo verificará como o componente de SPA `Text` fornecido é mapeado para o componente `Text` do AEM. Um novo componente de SPA `Image` é criado e pode ser usado no SPA e criado no AEM. Os recursos prontos das políticas do **Contêiner de layout** e **Editor de modelos** também serão usados para criar um modo de exibição um pouco mais variado na aparência.

![Criação final de amostra de capítulo](./assets/map-components/final-page.png)

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Obter o código

1. Baixe o ponto de partida para este tutorial pelo Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/map-components-start
   ```

2. Implante a base de código em uma instância do AEM local usando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se estiver usando o [AEM 6.x](overview.md#compatibility), adicione o perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) ou verificar o código localmente alternando para a ramificação `Angular/map-components-solution`.

## Abordagem de mapeamento

O conceito básico é mapear um componente de SPA para um componente do AEM. Componentes do AEM, executar no lado do servidor, exportar conteúdo como parte da API do modelo JSON. O conteúdo JSON é consumido pelo SPA, executando no lado do cliente no navegador. Um mapeamento 1:1 entre componentes de SPA e um componente do AEM é criado.

![Visão geral de alto nível do mapeamento de um componente do AEM para um componente do Angular](./assets/map-components/high-level-approach.png)

*Visão geral de alto nível do mapeamento de um componente do AEM para um componente do Angular*

## Inspecione o componente de Texto

O [Arquétipo de Projeto do AEM](https://github.com/adobe/aem-project-archetype) fornece um componente `Text` que é mapeado para o [componente de Texto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html?lang=pt-BR) do AEM. Este é um exemplo de um componente **conteúdo**, no qual ele renderiza *conteúdo* do AEM.

Vamos ver como o componente funciona.

### Inspecionar o modelo JSON

1. Antes de pular para o código SPA, é importante entender o modelo JSON que o AEM fornece. Navegue até a [Biblioteca de Componentes principais](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) e exiba a página do componente de Texto. A Biblioteca de componentes principais fornece exemplos de todos os Componentes principais do AEM.
2. Selecione a guia **JSON** para um dos exemplos:

   ![Modelo JSON de texto](./assets/map-components/text-json.png)

   Você deve ver três propriedades: `text`, `richText` e `:type`.

   `:type` é uma propriedade reservada que lista o `sling:resourceType` (ou caminho) do Componente AEM. O valor de `:type` é o que é usado para mapear o componente AEM para o componente SPA.

   `text` e `richText` são propriedades adicionais expostas ao componente de SPA.

### Inspecionar o componente de Texto

1. Abra um novo terminal e navegue até a pasta `ui.frontend` dentro do projeto. Execute `npm install` e depois `npm start` para iniciar o **servidor de desenvolvimento do webpack**:

   ```shell
   $ cd ui.frontend
   $ npm run start:mock
   ```

   O módulo `ui.frontend` está configurado atualmente para usar o [modelo JSON simulado](./integrate-spa.md#mock-json).

2. Você deve ver uma nova janela de navegador aberta para [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)

   ![Servidor de desenvolvimento do Webpack com conteúdo fictício](assets/map-components/initial-start.png)

3. No IDE de sua escolha, abra o Projeto AEM para o WKND SPA. Expanda o módulo `ui.frontend` e abra o arquivo **text.component.ts** em `ui.frontend/src/app/components/text/text.component.ts`:

   ![Código Source do Componente Angular do Text.js](assets/map-components/vscode-ide-text-js.png)

4. A primeira área a ser inspecionada é o `class TextComponent` na linha ~35:

   ```js
   export class TextComponent {
       @Input() richText: boolean;
       @Input() text: string;
       @Input() itemName: string;
   
       @HostBinding('innerHtml') get content() {
           return this.richText
           ? this.sanitizer.bypassSecurityTrustHtml(this.text)
           : this.text;
       }
       @HostBinding('attr.data-rte-editelement') editAttribute = true;
   
       constructor(private sanitizer: DomSanitizer) {}
   }
   ```

   O decorador [@Input()](https://angular.io/api/core/Input) é usado para declarar campos cujos valores são definidos por meio do objeto JSON mapeado, revisado anteriormente.

   `@HostBinding('innerHtml') get content()` é um método que expõe o conteúdo do texto criado a partir do valor de `this.text`. Caso o conteúdo seja rich text (determinado pelo sinalizador `this.richText`), a segurança interna do Angular é ignorada. O [DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer) da Angular é usado para &quot;limpar&quot; o HTML bruto e evitar vulnerabilidades de script entre sites. O método está associado à propriedade `innerHtml` usando o decorador [@HostBinding](https://angular.io/api/core/HostBinding).

5. Em seguida, verifique o `TextEditConfig` em ~line 24:

   ```js
   const TextEditConfig = {
       emptyLabel: 'Text',
       isEmpty: cqModel =>
           !cqModel || !cqModel.text || cqModel.text.trim().length < 1
   };
   ```

   O código acima é responsável por determinar quando renderizar o espaço reservado no ambiente de autor do AEM. Se o método `isEmpty` retornar **true**, o espaço reservado será renderizado.

6. Por fim, dê uma olhada na chamada `MapTo` em ~line 53:

   ```js
   MapTo('wknd-spa-angular/components/text')(TextComponent, TextEditConfig );
   ```

   **MapTo** é fornecido pelo AEM SPA Editor JS SDK (`@adobe/cq-angular-editable-components`). O caminho `wknd-spa-angular/components/text` representa o `sling:resourceType` do componente AEM. Esse caminho corresponde ao `:type` exposto pelo modelo JSON observado anteriormente. **MapTo** analisa a resposta do modelo JSON e passa os valores corretos para as variáveis `@Input()` do componente SPA.

   Você pode encontrar a definição do componente `Text` do AEM em `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/text`.

7. Experimente modificando o arquivo **en.model.json** em `ui.frontend/src/mocks/json/en.model.json`.

   Em ~line 62, atualize o primeiro valor `Text` para usar as marcas **`H1`** e **`u`**:

   ```json
       "text": {
           "text": "<h1><u>Hello World!</u></h1>",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       }
   ```

   Retorne ao navegador para ver os efeitos do **servidor de desenvolvimento do webpack**:

   ![Modelo de texto atualizado](assets/map-components/updated-text-model.png)

   Tente alternar a propriedade `richText` entre **true** / **false** para ver a lógica de renderização em ação.

8. Inspecionar **text.component.html** em `ui.frontend/src/app/components/text/text.component.html`.

   Este arquivo está vazio porque todo o conteúdo do componente está definido pela propriedade `innerHTML`.

9. Inspecione o **app.module.ts** em `ui.frontend/src/app/app.module.ts`.

   ```js
   @NgModule({
   imports: [
       BrowserModule,
       SpaAngularEditableComponentsModule,
       AppRoutingModule
   ],
   providers: [ModelManagerService, { provide: APP_BASE_HREF, useValue: '/' }],
   declarations: [AppComponent, TextComponent, PageComponent, HeaderComponent],
   entryComponents: [TextComponent, PageComponent],
   bootstrap: [AppComponent]
   })
   export class AppModule {}
   ```

   O **TextComponent** não está incluído explicitamente, mas dinamicamente por meio de **AEMResponsiveGridComponent** fornecido pelo AEM SPA Editor JS SDK. Portanto, deve ser listado na matriz **app.module.ts**&#39; [entryComponents](https://angular.io/guide/entry-components).

## Criar o componente de Imagem

Em seguida, crie um componente do Angular `Image` que seja mapeado para o [componente de Imagem](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=pt-BR) do AEM. O componente `Image` é outro exemplo de um componente **content**.

### Inspecionar o JSON

Antes de pular para o código SPA, inspecione o modelo JSON fornecido pelo AEM.

1. Navegue até os [exemplos de imagem na biblioteca de Componentes principais](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![Componente principal de imagem JSON](./assets/map-components/image-json.png)

   Propriedades de `src`, `alt` e `title` são usadas para popular o componente de SPA `Image`.

   >[!NOTE]
   >
   > Há outras propriedades de Imagem expostas (`lazyEnabled`, `widths`) que permitem a um desenvolvedor criar um componente adaptável e de carregamento lento. O componente compilado neste tutorial é simples e **não** usa essas propriedades avançadas.

2. Retorne ao IDE e abra o `en.model.json` em `ui.frontend/src/mocks/json/en.model.json`. Como este é um componente novo para o nosso projeto, precisamos &quot;simular&quot; o JSON de imagem.

   Em ~line 70, adicione uma entrada JSON para o modelo `image` (não se esqueça da vírgula à direita `,` após o segundo `text_386303036`) e atualize a matriz `:itemsOrder`.

   ```json
   ...
   ":items": {
               ...
               "text_386303036": {
                   "text": "<p>A new text component.</p>\r\n",
                   "richText": true,
                   ":type": "wknd-spa-angular/components/text"
                   },
               "image": {
                   "alt": "Rock Climber in New Zealand",
                   "title": "Rock Climber in New Zealand",
                   "src": "/mocks/images/adobestock-140634652.jpeg",
                   ":type": "wknd-spa-angular/components/image"
               }
           },
           ":itemsOrder": [
               "text",
               "text_386303036",
               "image"
           ],
   ```

   O projeto inclui uma imagem de exemplo em `/mock-content/adobestock-140634652.jpeg` que é usada com o **servidor de desenvolvimento do webpack**.

   Você pode exibir o [en.model.json completo aqui](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/map-components-solution/ui.frontend/src/mocks/json/en.model.json).

3. Adicione uma foto do estoque a ser exibida pelo componente.

   Crie uma nova pasta chamada **imagens** abaixo de `ui.frontend/src/mocks`. Baixe [adobestock-140634652.jpeg](assets/map-components/adobestock-140634652.jpeg) e coloque-o na pasta **images** recém-criada. Você pode usar sua própria imagem, se desejar.

### Implementar o componente de Imagem

1. Pare o **servidor de desenvolvimento do webpack**, se iniciado.
2. Crie um novo componente de Imagem executando o comando `ng generate component` da Angular CLI na pasta `ui.frontend`:

   ```shell
   $ ng generate component components/image
   ```

3. No IDE, abra **image.component.ts** em `ui.frontend/src/app/components/image/image.component.ts` e atualize da seguinte maneira:

   ```js
   import {Component, Input, OnInit} from '@angular/core';
   import {MapTo} from '@adobe/cq-angular-editable-components';
   
   const ImageEditConfig = {
   emptyLabel: 'Image',
   isEmpty: cqModel =>
       !cqModel || !cqModel.src || cqModel.src.trim().length < 1
   };
   
   @Component({
   selector: 'app-image',
   templateUrl: './image.component.html',
   styleUrls: ['./image.component.scss']
   })
   export class ImageComponent implements OnInit {
   
   @Input() src: string;
   @Input() alt: string;
   @Input() title: string;
   
   constructor() { }
   
   get hasImage() {
       return this.src && this.src.trim().length > 0;
   }
   
   ngOnInit() { }
   }
   
   MapTo('wknd-spa-angular/components/image')(ImageComponent, ImageEditConfig);
   ```

   `ImageEditConfig` é a configuração para determinar se o espaço reservado do autor deve ser renderizado no AEM, com base no preenchimento da propriedade `src`.

   `@Input()` de `src`, `alt` e `title` são as propriedades mapeadas da API JSON.

   `hasImage()` é um método que determinará se a imagem deve ser renderizada.

   `MapTo` mapeia o componente SPA para o componente AEM localizado em `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/image`.

4. Abra **image.component.html** e atualize da seguinte maneira:

   ```html
   <ng-container *ngIf="hasImage">
       <img class="image" [src]="src" [alt]="alt" [title]="title"/>
   </ng-container>
   ```

   Isso renderizará o elemento `<img>` se `hasImage` retornar **true**.

5. Abra **image.component.scss** e atualize da seguinte maneira:

   ```scss
   :host-context {
       display: block;
   }
   
   .image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

   >[!NOTE]
   >
   > A regra `:host-context` é **crítica** para que o espaço reservado do editor de SPA do AEM funcione corretamente. Todos os componentes de SPA que devem ser criados no editor de páginas do AEM precisarão dessa regra no mínimo.

6. Abra `app.module.ts` e adicione `ImageComponent` à matriz `entryComponents`:

   ```js
   entryComponents: [TextComponent, PageComponent, ImageComponent],
   ```

   Assim como o `TextComponent`, o `ImageComponent` é carregado dinamicamente e deve ser incluído na matriz `entryComponents`.

7. Inicie o **servidor de desenvolvimento do webpack** para ver a renderização de `ImageComponent`.

   ```shell
   $ npm run start:mock
   ```

   ![Imagem adicionada ao modelo](assets/map-components/image-added-mock.png)

   *Imagem adicionada ao SPA*

   >[!NOTE]
   >
   > **Desafio de bônus**: implemente um novo método para exibir o valor de `title` como uma legenda abaixo da imagem.

## Atualizar políticas no AEM

O componente `ImageComponent` só é visível no **servidor de desenvolvimento do webpack**. Em seguida, implante o SPA atualizado no AEM e atualize as políticas do modelo.

1. Pare o **servidor de desenvolvimento do webpack** e, a partir da **raiz** do projeto, implante as alterações no AEM usando suas habilidades em Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Na tela inicial do AEM, navegue até **[!UICONTROL Ferramentas]** > **[!UICONTROL Modelos]** > **[WKND SPA Angular](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-angular)**.

   Selecione e edite a **Página do SPA**:

   ![Editar Modelo de Página de SPA](assets/map-components/edit-spa-page-template.png)

3. Selecione o **Contêiner de layout** e clique em seu ícone de **política** para editar a política:

   ![Política de Contêiner de Layout](./assets/map-components/layout-container-policy.png)

4. Em **Componentes Permitidos** > **WKND SPA Angular - Conteúdo** > verifique o componente **Imagem**:

   ![Componente de imagem selecionado](assets/map-components/check-image-component.png)

   Em **Componentes Padrão** > **Adicionar mapeamento** e escolha o componente **Imagem - WKND SPA Angular - Conteúdo**:

   ![Definir componentes padrão](assets/map-components/default-components.png)

   Insira um **tipo MIME** de `image/*`.

   Clique em **Concluído** para salvar as atualizações de política.

5. No **Contêiner de Layout**, clique no ícone **política** para o componente **Texto**:

   ![Ícone da política do componente de texto](./assets/map-components/edit-text-policy.png)

   Crie uma nova política chamada **WKND SPA Text**. Em **Plugins** > **Formatação** > marque todas as caixas para habilitar opções de formatação adicionais:

   ![Habilitar Formatação RTE](assets/map-components/enable-formatting-rte.png)

   Em **Plug-ins** > **Estilos de parágrafo** >, marque a caixa para **Habilitar estilos de parágrafo**:

   ![Habilitar estilos de parágrafo](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Clique em **Concluído** para salvar a atualização de política.

6. Navegue até a **Página inicial** [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).

   Você também pode editar o componente `Text` e adicionar outros estilos de parágrafo no modo **tela cheia**.

   ![Edição de Rich Text em Tela Inteira](assets/map-components/full-screen-rte.png)

7. Você também pode arrastar e soltar uma imagem do **Localizador de ativos**:

   ![Arrastar e soltar imagem](./assets/map-components/drag-drop-image.gif)

8. Adicione suas próprias imagens via [AEM Assets](http://localhost:4502/assets.html/content/dam) ou instale a base de código concluída para o [site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) padrão. O [site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) inclui muitas imagens que podem ser reutilizadas no SPA do WKND. O pacote pode ser instalado usando o [Gerenciador de Pacotes](http://localhost:4502/crx/packmgr/index.jsp) da AEM.

   ![Instalar wknd.all](./assets/map-components/package-manager-wknd-all.png) do Gerenciador de Pacotes

## Inspecione o contêiner de layout

O suporte para o **Contêiner de layout** é fornecido automaticamente pelo SDK do Editor SPA do AEM. O **Contêiner de Layout**, conforme indicado pelo nome, é um componente **contêiner**. Componentes de contêiner são componentes que aceitam estruturas JSON que representam *outros* componentes e os instanciam dinamicamente.

Vamos analisar mais detalhadamente o Contêiner de layout.

1. No IDE, abra **responsive-grid.component.ts** em `ui.frontend/src/app/components/responsive-grid`:

   ```js
   import { AEMResponsiveGridComponent,MapTo } from '@adobe/cq-angular-editable-components';
   
   MapTo('wcm/foundation/components/responsivegrid')(AEMResponsiveGridComponent);
   ```

   O `AEMResponsiveGridComponent` é implementado como parte do SDK do Editor SPA do AEM e está incluído no projeto via `import-components`.

2. Em um navegador, navegue até [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   ![API do modelo JSON - Grade responsiva](./assets/map-components/responsive-grid-modeljson.png)

   O componente **Contêiner de Layout** tem um `sling:resourceType` de `wcm/foundation/components/responsivegrid` e é reconhecido pelo Editor SPA usando a propriedade `:type`, exatamente como os componentes `Text` e `Image`.

   Os mesmos recursos de redimensionamento de um componente usando o [Modo de layout](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html?lang=pt-BR#defining-layouts-layout-mode) estão disponíveis com o Editor de SPA.

3. Retorne a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). Adicione componentes adicionais de **Imagem** e tente redimensioná-los usando a opção **Layout**:

   ![Redimensionar imagem usando o modo Layout](./assets/map-components/responsive-grid-layout-change.gif)

4. Abra novamente o modelo JSON [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json) e observe o `columnClassNames` como parte do JSON:

   ![Nomes de Classe de Coluna](./assets/map-components/responsive-grid-classnames.png)

   O nome de classe `aem-GridColumn--default--4` indica que o componente deve ter 4 colunas de largura com base em uma grade de 12 colunas. Mais detalhes sobre a [grade responsiva podem ser encontrados aqui](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

5. Retorne ao IDE e no módulo `ui.apps` há uma biblioteca do lado do cliente definida em `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-grid`. Abra o arquivo `less/grid.less`.

   Este arquivo determina os pontos de interrupção (`default`, `tablet` e `phone`) usados pelo **Contêiner de Layout**. Este arquivo deve ser personalizado de acordo com as especificações do projeto. Os pontos de interrupção estão definidos como `1200px` e `650px`.

6. Você deve ser capaz de usar os recursos responsivos e as políticas de rich text atualizadas do componente `Text` para criar uma exibição como a seguinte:

   ![Criação final de amostra de capítulo](assets/map-components/final-page.png)

## Parabéns! {#congratulations}

Parabéns, você aprendeu a mapear componentes de SPA para Componentes do AEM e implementou um novo componente `Image`. Você também pode explorar os recursos responsivos do **Contêiner de layout**.

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/map-components-solution) ou verificar o código localmente alternando para a ramificação `Angular/map-components-solution`.

### Próximas etapas {#next-steps}

[Navegação e roteamento](navigation-routing.md) - Saiba como é possível oferecer suporte a várias exibições no SPA mapeando para Páginas do AEM com o SDK do Editor de SPA. A navegação dinâmica é implementada usando o Angular Router e adicionada a um componente Cabeçalho existente.

## Bônus - Configurações persistentes para o controle de origem {#bonus}

Em muitos casos, especialmente no início de um projeto do AEM, é valioso manter as configurações, como modelos e políticas de conteúdo relacionadas, no controle de origem. Isso garante que todos os desenvolvedores trabalhem com o mesmo conjunto de conteúdo e configurações e possa garantir consistência adicional entre os ambientes. Quando um projeto atinge um determinado nível de maturidade, a prática de gerenciar modelos pode ser transferida para um grupo especial de usuários avançados.

As próximas etapas ocorrerão usando o IDE do Visual Studio Code e o [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync), mas pode ser usando qualquer ferramenta e qualquer IDE que você tenha configurado para **extrair** ou **importar** conteúdo de uma instância local do AEM.

1. No Visual Studio Code IDE, verifique se você tem o **VSCode AEM Sync** instalado por meio da extensão do Marketplace:

   ![Sincronização com o AEM VSCode](./assets/map-components/vscode-aem-sync.png)

2. Expanda o módulo **ui.content** no Gerenciador de projetos e navegue até `/conf/wknd-spa-angular/settings/wcm/templates`.

3. **Clique com o botão direito do mouse** na pasta `templates` e selecione **Importar do AEM Server**:

   ![Modelo de importação do VSCode](assets/map-components/import-aem-servervscode.png)

4. Repita as etapas para importar o conteúdo, mas selecione a pasta **políticas** localizada em `/conf/wknd-spa-angular/settings/wcm/policies`.

5. Inspecione o arquivo `filter.xml` localizado em `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-angular" mode="merge"/>
        <filter root="/content/wknd-spa-angular" mode="merge"/>
        <filter root="/content/dam/wknd-spa-angular" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-angular" mode="merge"/>
    </workspaceFilter>
   ```

   O arquivo `filter.xml` é responsável por identificar os caminhos dos nós instalados com o pacote. Observe o `mode="merge"` em cada filtro que indica que o conteúdo existente não será modificado, somente o novo conteúdo será adicionado. Como os autores de conteúdo podem estar atualizando esses caminhos, é importante que uma implantação de código **não** substitua o conteúdo. Consulte a [documentação do FileVault](https://jackrabbit.apache.org/filevault/filter.html) para obter mais detalhes sobre como trabalhar com elementos de filtro.

   Compare `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` para entender os diferentes nós gerenciados por cada módulo.
