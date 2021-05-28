---
title: Adicionar navegação e roteamento | Introdução ao AEM SPA Editor e Angular
description: Saiba como várias exibições no SPA são compatíveis com o uso de Páginas AEM e do SDK do Editor SPA. A navegação dinâmica é implementada usando rotas Angular e adicionada a um componente Cabeçalho existente.
sub-product: sites
feature: Editor de SPA
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5312
thumbnail: 5312-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: bf9ab30f57faa23721d7d27b837d8e0f0e8cf4f1
workflow-type: tm+mt
source-wordcount: '2723'
ht-degree: 1%

---


# Adicionar navegação e roteamento {#navigation-routing}

Saiba como várias exibições no SPA são compatíveis com o uso de Páginas AEM e do SDK do Editor SPA. A navegação dinâmica é implementada usando rotas Angular e adicionada a um componente Cabeçalho existente.

## Objetivo

1. Entenda as opções de roteamento do modelo de SPA disponíveis ao usar o Editor de SPA.
2. Saiba como usar [Angular routing](https://angular.io/guide/router) para navegar entre diferentes visualizações do SPA.
3. Implemente uma navegação dinâmica orientada pela hierarquia de página de AEM.

## O que você vai criar

Este capítulo adiciona um menu de navegação a um componente `Header` existente. O menu de navegação é orientado pela hierarquia de página de AEM e usa o modelo JSON fornecido pelo [Componente principal de navegação](https://docs.adobe.com/content/help/pt/experience-manager-core-components/using/components/navigation.html).

![Navegação implementada](assets/navigation-routing/final-navigation-implemented.gif)

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Obter o código

1. Baixe o ponto de partida para este tutorial via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/navigation-routing-start
   ```

2. Implante a base de código em uma instância de AEM local usando o Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se estiver usando [AEM 6.x](overview.md#compatibility) adicione o perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Instale o pacote concluído para o site de referência tradicional [WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). As imagens fornecidas pelo [site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) serão reutilizadas no SPA WKND. O pacote pode ser instalado usando [AEM Gerenciador de Pacotes](http://localhost:4502/crx/packmgr/index.jsp).

   ![O Gerenciador de Pacotes instala o wknd.all](./assets/map-components/package-manager-wknd-all.png)

Você sempre pode visualizar o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) ou verificar o código localmente ao alternar para a ramificação `Angular/navigation-routing-solution`.

## Atualizações do HeaderComponent do Inspect {#inspect-header}

Em capítulos anteriores, o componente `HeaderComponent` era adicionado como um componente de Angular puro incluído por meio de `app.component.html`. Neste capítulo, o componente `HeaderComponent` é removido do aplicativo e será adicionado por meio do [Editor de modelo](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html). Isso permite que os usuários configurem o menu de navegação do `HeaderComponent` de dentro do AEM.

>[!NOTE]
>
> Várias atualizações de CSS e JavaScript já foram feitas na base de código para iniciar este capítulo. Para se concentrar nos conceitos principais, não **todas** das alterações de código são discutidas. Você pode visualizar as alterações completas [aqui](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start).

1. No IDE de sua escolha, abra o SPA projeto inicial para este capítulo.
2. Abaixo do módulo `ui.frontend` inspecione o arquivo `header.component.ts` em: `ui.frontend/src/app/components/header/header.component.ts`.

   Várias atualizações foram feitas, incluindo a adição de um `HeaderEditConfig` e um `MapTo` para permitir que o componente seja mapeado para um componente AEM `wknd-spa-angular/components/header`.

   ```js
   /* header.component.ts */
   ...
   const HeaderEditConfig = {
       ...
   };
   
   @Component({
   selector: 'app-header',
   templateUrl: './header.component.html',
   styleUrls: ['./header.component.scss']
   })
   export class HeaderComponent implements OnInit {
   @Input() items: object[];
       ...
   }
   ...
   MapTo('wknd-spa-angular/components/header')(withRouter(Header), HeaderEditConfig);
   ```

   Observe a anotação `@Input()` para `items`. `items` conterá uma matriz de objetos de navegação passados do AEM.

3. No módulo `ui.apps` inspecione a definição do componente do AEM `Header`: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   O componente `Header` AEM herdará toda a funcionalidade do [Componente principal de navegação](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) por meio da propriedade `sling:resourceSuperType`.

## Adicione o HeaderComponent ao modelo de SPA {#add-header-template}

1. Abra um navegador e faça logon no AEM, [http://localhost:4502/](http://localhost:4502/). A base de código inicial já deve ser implantada.
2. Navegue até **[!UICONTROL SPA Modelo de Página]**: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Selecione o **[!UICONTROL Contêiner de layout raiz]** mais externo e clique no ícone **[!UICONTROL Política]**. Tenha cuidado **e não** para selecionar o **[!UICONTROL Contêiner de layout]** desbloqueado para criação.

   ![Selecione o ícone de política do contêiner de layout de raiz](assets/navigation-routing/root-layout-container-policy.png)

4. Copie a política atual e crie uma nova política chamada **[!UICONTROL SPA Estrutura]**:

   ![Política de Estrutura SPA](assets/map-components/spa-policy-update.png)

   Em **[!UICONTROL Componentes permitidos]** > **[!UICONTROL Geral]** > selecione o componente **[!UICONTROL Contêiner de layout]**.

   Em **[!UICONTROL Componentes permitidos]** > **[!UICONTROL ANGULAR SPA WKND - ESTRUTURA]** > selecione o componente **[!UICONTROL Cabeçalho]**:

   ![Selecionar componente do cabeçalho](assets/map-components/select-header-component.png)

   Em **[!UICONTROL Componentes permitidos]** > **[!UICONTROL ANGULAR SPA WKND - Conteúdo]** > selecione os componentes **[!UICONTROL Imagem]** e **[!UICONTROL Texto]**. Você deve ter quatro componentes totais selecionados.

   Clique em **[!UICONTROL Concluído]** para salvar as alterações.

5. **Atualize a página.** Adicione o componente **[!UICONTROL Cabeçalho]** acima do **[!UICONTROL Contêiner de layout desbloqueado]**:

   ![adicionar componente Cabeçalho ao modelo](./assets/navigation-routing/add-header-component.gif)

6. Selecione o componente **[!UICONTROL Cabeçalho]** e clique no ícone **Política** para editar a política.

   ![Clique na política Cabeçalho](assets/navigation-routing/header-policy-icon.png)

7. Crie uma nova política com um **[!UICONTROL Título da Política]** de **&quot;Cabeçalho SPA WKND&quot;**.

   Em **[!UICONTROL Propriedades]**:

   * Defina **[!UICONTROL Raiz de navegação]** para `/content/wknd-spa-angular/us/en`.
   * Defina **[!UICONTROL Excluir níveis de raiz]** para **1**.
   * Desmarque **[!UICONTROL Coletar todas as páginas secundárias]**.
   * Defina **[!UICONTROL Profundidade da estrutura de navegação]** para **3**.

   ![Configurar política de cabeçalho](assets/navigation-routing/header-policy.png)

   Isso coletará os 2 níveis de navegação abaixo de `/content/wknd-spa-angular/us/en`.

8. Depois de salvar suas alterações, você deve ver o `Header` preenchido como parte do modelo:

   ![Componente de cabeçalho preenchido](assets/navigation-routing/populated-header.png)

## Criar páginas secundárias

Em seguida, crie páginas adicionais no AEM que servirão como visualizações diferentes no SPA. Inspecionaremos também a estrutura hierárquica do modelo JSON fornecido pelo AEM.

1. Navegue até o console **Sites**: [http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). Selecione a **Página Inicial SPA Angular WKND** e clique em **[!UICONTROL Criar]** > **[!UICONTROL Página]**:

   ![Criar nova página](assets/navigation-routing/create-new-page.png)

2. Em **[!UICONTROL Modelo]** selecione **[!UICONTROL SPA Página]**. Em **[!UICONTROL Propriedades]**, digite **&quot;Página 1&quot;** para o **[!UICONTROL Título]** e **&quot;página-1&quot;** como o nome.

   ![Insira as propriedades da página inicial](assets/navigation-routing/initial-page-properties.png)

   Clique em **[!UICONTROL Criar]** e, na janela pop-up, clique em **[!UICONTROL Abrir]** para abrir a página no Editor de SPA de AEM.

3. Adicione um novo componente **[!UICONTROL Text]** ao **[!UICONTROL Contêiner de layout]** principal. Edite o componente e insira o texto: **&quot;Página 1&quot;** usando o RTE e o elemento **H1** (você terá que entrar no modo de tela cheia para alterar os elementos de parágrafo)

   ![Exemplo de página de conteúdo 1](assets/navigation-routing/page-1-sample-content.png)

   Você pode adicionar conteúdo adicional, como uma imagem.

4. Retorne ao console do AEM Sites e repita as etapas acima, criando uma segunda página chamada **&quot;Page 2&quot;** como um irmão da **Page 1**. Adicione conteúdo a **Página 2** para que seja facilmente identificado.
5. Por fim, crie uma terceira página, **&quot;Page 3&quot;**, mas como **child** de **Page 2**. Depois de concluída, a hierarquia do site deve ser semelhante ao seguinte:

   ![Hierarquia de site de exemplo](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

6. Em uma nova guia, abra a API do modelo JSON fornecida pelo AEM: [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Esse conteúdo JSON é solicitado quando o SPA é carregado pela primeira vez. A estrutura externa tem a seguinte aparência:

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-angular/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-angular/us/en",
   ":children": {
       "/content/wknd-spa-angular/us/en/home": {},
       "/content/wknd-spa-angular/us/en/home/page-1": {},
       "/content/wknd-spa-angular/us/en/home/page-2": {},
       "/content/wknd-spa-angular/us/en/home/page-2/page-3": {}
       }
   }
   ```

   Em `:children` você deve ver uma entrada para cada uma das páginas criadas. O conteúdo de todas as páginas está nesta solicitação JSON inicial. Depois que o roteamento de navegação for implementado, as exibições subsequentes do SPA serão carregadas rapidamente, já que o conteúdo já está disponível no lado do cliente.

   Não é recomendável carregar **ALL** do conteúdo de um SPA na solicitação JSON inicial, pois isso atrasaria o carregamento da página inicial. Em seguida, vamos examinar como a profundidade da hierarquia das páginas é coletada.

7. Navegue até o modelo **Raiz SPA** em: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   Clique no menu **[!UICONTROL Propriedades da página]** > **[!UICONTROL Política da página]**:

   ![Abra a política de página para SPA raiz](assets/navigation-routing/open-page-policy.png)

8. O modelo **SPA Root** tem uma guia **[!UICONTROL Hierarchical Structure]** extra para controlar o conteúdo JSON coletado. A **[!UICONTROL Profundidade da estrutura]** determina o quão profundo na hierarquia do site é coletar páginas secundárias abaixo de **root**. Também é possível usar o campo **[!UICONTROL Structure Patterns]** para filtrar páginas adicionais com base em uma expressão regular.

   Atualize a **[!UICONTROL Profundidade da estrutura]** para **&quot;2&quot;**:

   ![Atualizar profundidade da estrutura](assets/navigation-routing/update-structure-depth.png)

   Clique em **[!UICONTROL Concluído]** para salvar as alterações na política.

9. Abra novamente o modelo JSON [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-angular/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-angular/us/en",
   ":children": {
       "/content/wknd-spa-angular/us/en/home": {},
       "/content/wknd-spa-angular/us/en/home/page-1": {},
       "/content/wknd-spa-angular/us/en/home/page-2": {}
       }
   }
   ```

   Observe que o caminho **Page 3** foi removido: `/content/wknd-spa-angular/us/en/home/page-2/page-3` do modelo JSON inicial.

   Posteriormente, observaremos como o SDK do Editor de SPA AEM pode carregar dinamicamente conteúdo adicional.

## Implementar a navegação

Em seguida, implemente o menu de navegação com um novo `NavigationComponent`. Podemos adicionar o código diretamente em `header.component.html`, mas uma prática melhor é evitar componentes grandes. Em vez disso, implemente um `NavigationComponent` que possa ser reutilizado posteriormente.

1. Revise o JSON exposto pelo componente `Header` do AEM em [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json):

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-angular/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-angular/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA Angular Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-angular/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-angular/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-angular/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-angular/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-angular/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-angular/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-angular/components/header"
   ```

   A natureza hierárquica das páginas de AEM é modelada no JSON que pode ser usado para preencher um menu de navegação. Lembre-se de que o componente `Header` herda toda a funcionalidade do [Componente principal de navegação](https://www.aemcomponents.dev/content/core-components-examples/library/templating/navigation.html) e que o conteúdo exposto por meio do JSON será mapeado automaticamente para a anotação `@Input` do Angular.

2. Abra uma nova janela de terminal e navegue até a pasta `ui.frontend` do projeto de SPA. Crie um novo `NavigationComponent` usando a ferramenta Angular CLI:

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. Em seguida, crie uma classe chamada `NavigationLink` usando a CLI do Angular no diretório recém-criado `components/navigation`:

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. Retorne ao IDE de sua escolha e abra o arquivo em `navigation-link.ts` em `/src/app/components/navigation/navigation-link.ts`.

   ![Abrir arquivo navigation-link.ts](assets/navigation-routing/ide-navigation-link-file.png)

5. Preencha `navigation-link.ts` com o seguinte:

   ```js
   export class NavigationLink {
   
       title: string;
       path: string;
       url: string;
       level: number;
       children: NavigationLink[];
       active: boolean;
   
       constructor(data) {
           this.path = data.path;
           this.title = data.title;
           this.url = data.url;
           this.level = data.level;
           this.active = data.active;
           this.children = data.children.map( item => {
               return new NavigationLink(item);
           });
       }
   }
   ```

   Esta é uma classe simples para representar um link de navegação individual. No construtor de classe, esperamos que `data` seja o objeto JSON transmitido de AEM. Essa classe será usada dentro de `NavigationComponent` e `HeaderComponent` para preencher facilmente a estrutura de navegação.

   Nenhuma transformação de dados é executada, essa classe é criada primariamente para digitar fortemente o modelo JSON. Observe que `this.children` é digitado como `NavigationLink[]` e que o construtor cria recursivamente novos objetos `NavigationLink` para cada um dos itens na matriz `children`. Lembre-se de que o modelo JSON para `Header` é hierárquico.

6. Abra o arquivo `navigation-link.spec.ts`. Este é o arquivo de teste para a classe `NavigationLink`. Atualize com o seguinte:

   ```js
   import { NavigationLink } from './navigation-link';
   
   describe('NavigationLink', () => {
       it('should create an instance', () => {
           const data = {
               children: [],
               level: 1,
               active: false,
               path: '/content/wknd-spa-angular/us/en/home/page-1',
               description: null,
               url: '/content/wknd-spa-angular/us/en/home/page-1.html',
               lastModified: 1589429385100,
               title: 'Page 1'
           };
           expect(new NavigationLink(data)).toBeTruthy();
       });
   });
   ```

   Observe que `const data` segue o mesmo modelo JSON inspecionado anteriormente para um único link. Isso está longe de ser um teste de unidade robusto, no entanto, deve ser suficiente testar o construtor de `NavigationLink`.

7. Abra o arquivo `navigation.component.ts`. Atualize com o seguinte:

   ```js
   import { Component, OnInit, Input } from '@angular/core';
   import { NavigationLink } from './navigation-link';
   
   @Component({
   selector: 'app-navigation',
   templateUrl: './navigation.component.html',
   styleUrls: ['./navigation.component.scss']
   })
   export class NavigationComponent implements OnInit {
   
       @Input() items: object[];
   
       constructor() { }
   
       get navigationLinks(): NavigationLink[] {
   
           if (this.items && this.items.length > 0) {
               return this.items.map(item => {
                   return new NavigationLink(item);
               });
           }
   
           return null;
       }
   
       ngOnInit() {}
   
   }
   ```

   `NavigationComponent` O espera um  `object[]` nome  `items` que seja o modelo JSON do AEM. Essa classe expõe um único método `get navigationLinks()` que retorna uma matriz de objetos `NavigationLink`.

8. Abra o arquivo `navigation.component.html` e atualize-o com o seguinte:

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   Isso gera um `<ul>` inicial e chama o método `get navigationLinks()` de `navigation.component.ts`. Um `<ng-container>` é usado para fazer uma chamada para um template chamado `recursiveListTmpl` e o transmite como `navigationLinks` como uma variável chamada `links`.

   Adicione o `recursiveListTmpl` próximo:

   ```html
   <ng-template #recursiveListTmpl let-links="links">
       <li *ngFor="let link of links" class="{{'navigation__item navigation__item--' + link.level}}">
           <a [routerLink]="link.url" class="navigation__item-link" [title]="link.title" [attr.aria-current]="link.active">
               {{link.title}}
           </a>
           <ul *ngIf="link.children && link.children.length > 0">
               <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: link.children }"></ng-container>
           </ul>
       </li>
   </ng-template>
   ```

   Aqui, o restante da renderização do link de navegação é implementado. Observe que a variável `link` é do tipo `NavigationLink` e todos os métodos/propriedades criados por essa classe estão disponíveis. [`[routerLink]`](https://angular.io/api/router/RouterLink) é usado em vez do  `href` atributo normal. Isso nos permite vincular a rotas específicas no aplicativo, sem uma atualização de página inteira.

   A parte recursiva da navegação também é implementada pela criação de outro `<ul>` se o `link` atual tiver uma matriz `children` não vazia.

9. Atualize `navigation.component.spec.ts` para adicionar suporte para `RouterTestingModule`:

   ```diff
    ...
   + import { RouterTestingModule } from '@angular/router/testing';
    ...
    beforeEach(async(() => {
       TestBed.configureTestingModule({
   +   imports: [ RouterTestingModule ],
       declarations: [ NavigationComponent ]
       })
       .compileComponents();
    }));
    ...
   ```

   É necessário adicionar o `RouterTestingModule` porque o componente usa `[routerLink]`.

10. Atualize `navigation.component.scss` para adicionar alguns estilos básicos ao `NavigationComponent`:

   ```scss
   @import "~src/styles/variables";
   
   $link-color: $black;
   $link-hover-color: $white;
   $link-background: $black;
   
   :host-context {
       display: block;
       width: 100%;
   }
   
   .navigation__item {
       list-style: none;
   }
   
   .navigation__item-link {
       color: $link-color;
       font-size: $font-size-large;
       text-transform: uppercase;
       padding: $gutter-padding;
       display: flex;
       border-bottom: 1px solid $gray;
   
       &:hover {
           background: $link-background;
           color: $link-hover-color;
       }
   
   }
   ```

## Atualizar o componente de cabeçalho

Agora que `NavigationComponent` foi implementado, o `HeaderComponent` deve ser atualizado para referenciá-lo.

1. Abra um terminal e navegue até a pasta `ui.frontend` no projeto SPA. Inicie o **servidor de desenvolvimento do webpack**:

   ```shell
   $ npm start
   ```

2. Abra uma guia do navegador e navegue até [http://localhost:4200/](http://localhost:4200/).

   O **servidor de desenvolvimento de webpack** deve ser configurado para proxy do modelo JSON de uma instância local de AEM (`ui.frontend/proxy.conf.json`). Isso nos permitirá codificar diretamente no conteúdo criado no AEM do anteriormente no tutorial.

   ![alternância de menu funcionando](./assets/navigation-routing/nav-toggle-static.gif)

   O `HeaderComponent` atualmente possui a funcionalidade de alternância de menu já implementada. Em seguida, adicione o componente de navegação.

3. Retorne ao IDE de sua escolha e abra o arquivo `header.component.ts` em `ui.frontend/src/app/components/header/header.component.ts`.
4. Atualize o método `setHomePage()` para remover a String codificada e use as props dinâmicas passadas pelo componente AEM:

   ```js
   /* header.component.ts */
   import { NavigationLink } from '../navigation/navigation-link';
   ...
    setHomePage() {
       if (this.hasNavigation) {
           const rootNavigationLink: NavigationLink = new NavigationLink(this.items[0]);
           this.isHome = rootNavigationLink.path === this.route.snapshot.data.path;
           this.homePageUrl = rootNavigationLink.url;
       }
   }
   ...
   ```

   Uma nova instância de `NavigationLink` é criada com base em `items[0]`, a raiz do modelo JSON de navegação transmitida do AEM. `this.route.snapshot.data.path` retorna o caminho da rota de Angular atual. Esse valor é usado para determinar se a rota atual é a **Home Page**. `this.homePageUrl` é usada para preencher o link de âncora no  **logotipo**.

5. Abra `header.component.html` e substitua o espaço reservado estático para a navegação por uma referência para o `NavigationComponent` recém-criado:

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   `[items]=items` transmite o  `@Input() items` do  `HeaderComponent` para o  `NavigationComponent` local em que criará a navegação.

6. Abra `header.component.spec.ts` e adicione uma declaração para `NavigationComponent`:

   ```diff
       /* header.component.spect.ts */
   +   import { NavigationComponent } from '../navigation/navigation.component';
   
       describe('HeaderComponent', () => {
       let component: HeaderComponent;
       let fixture: ComponentFixture<HeaderComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           imports: [ RouterTestingModule ],
   +       declarations: [ HeaderComponent, NavigationComponent ]
           })
           .compileComponents();
       }));
   ```

   Como o `NavigationComponent` agora é usado como parte do `HeaderComponent`, ele precisa ser declarado como parte da base de teste.

7. Salve as alterações em qualquer arquivo aberto e retorne ao **servidor dev do webpack**: [http://localhost:4200/](http://localhost:4200/)

   ![Navegação de cabeçalho concluída](assets/navigation-routing/completed-header.png)

   Abra a navegação clicando no botão de alternância do menu e você deverá ver os links de navegação preenchidos. É possível navegar para diferentes exibições do SPA.

## Entender o roteamento de SPA

Agora que a navegação foi implementada, inspecione o roteamento no AEM.

1. No IDE, abra o arquivo `app-routing.module.ts` em `ui.frontend/src/app`.

   ```js
   /* app-routing.module.ts */
   import { AemPageDataResolver, AemPageRouteReuseStrategy } from '@adobe/cq-angular-editable-components';
   import { NgModule } from '@angular/core';
   import { RouteReuseStrategy, RouterModule, Routes, UrlMatchResult, UrlSegment } from '@angular/router';
   import { PageComponent } from './components/page/page.component';
   
   export function AemPageMatcher(url: UrlSegment[]): UrlMatchResult {
       if (url.length) {
           return {
               consumed: url,
               posParams: {
                   path: url[url.length - 1]
               }
           };
       }
   }
   
   const routes: Routes = [
       {
           matcher: AemPageMatcher,
           component: PageComponent,
           resolve: {
               path: AemPageDataResolver
           }
       }
   ];
   @NgModule({
       imports: [RouterModule.forRoot(routes)],
       exports: [RouterModule],
       providers: [
           AemPageDataResolver,
           {
           provide: RouteReuseStrategy,
           useClass: AemPageRouteReuseStrategy
           }
       ]
   })
   export class AppRoutingModule {}
   ```

   A matriz `routes: Routes = [];` define as rotas ou os caminhos de navegação para os mapeamentos de componentes do Angular.

   `AemPageMatcher` é um roteador de Angular personalizado  [UrlMatcher](https://angular.io/api/router/UrlMatcher), que corresponde a qualquer coisa que &quot;pareça&quot; com uma página no AEM que faz parte deste aplicativo Angular.

   `PageComponent` é o Componente do Angular que representa uma Página no AEM e as rotas correspondentes serão chamadas. O `PageComponent` será inspecionado posteriormente.

   `AemPageDataResolver`, fornecido pelo SDK JS do Editor de SPA AEM, é um  [Angular Router ](https://angular.io/api/router/Resolve) Resolverused personalizado para transformar a URL da rota, que é o caminho em AEM incluindo a extensão .html, para o caminho do recurso em AEM, que é o caminho da página menos a extensão.

   Por exemplo, o `AemPageDataResolver` transforma o URL de uma rota de `content/wknd-spa-angular/us/en/home.html` em um caminho de `/content/wknd-spa-angular/us/en/home`. Isso é usado para resolver o conteúdo da página com base no caminho na API do modelo JSON.

   `AemPageRouteReuseStrategy`, fornecido pelo Editor de SPA AEM JS SDK, é uma  [](https://angular.io/api/router/RouteReuseStrategy) RouteReuseStrategy personalizada que impede a reutilização de  `PageComponent` várias rotas. Caso contrário, o conteúdo da página &quot;A&quot; pode ser exibido ao navegar até a página &quot;B&quot;.

2. Abra o arquivo `page.component.ts` em `ui.frontend/src/app/components/page/`.

   ```js
   ...
   export class PageComponent {
       items;
       itemsOrder;
       path;
   
       constructor(
           private route: ActivatedRoute,
           private modelManagerService: ModelManagerService
       ) {
           this.modelManagerService
           .getData({ path: this.route.snapshot.data.path })
           .then(data => {
               this.path = data[Constants.PATH_PROP];
               this.items = data[Constants.ITEMS_PROP];
               this.itemsOrder = data[Constants.ITEMS_ORDER_PROP];
           });
       }
   }
   ```

   O `PageComponent` é necessário para processar o JSON recuperado do AEM e é usado como o componente Angular para renderizar as rotas.

   `ActivatedRoute`, que é fornecido pelo módulo Roteador do Angular, contém o estado que indica qual conteúdo JSON da Página AEM deve ser carregado nesta instância do componente Página do Angular.

   `ModelManagerService`, obtém os dados JSON com base na rota e mapeia os dados para variáveis de classe  `path`,  `items`,  `itemsOrder`. Eles serão passados para o [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. Abra o arquivo `page.component.html` em `ui.frontend/src/app/components/page/`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` inclui o  [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md). As variáveis `path`, `items` e `itemsOrder` são passadas para `AEMPageComponent`. O `AemPageComponent`, fornecido por meio dos SDKs do JavaScript do Editor de SPA, iterará sobre esses dados e instanciará dinamicamente os componentes do Angular com base nos dados JSON, conforme visto no [Tutorial Mapear componentes](./map-components.md).

   O `PageComponent` é realmente apenas um proxy para o `AEMPageComponent` e é o `AEMPageComponent` que faz a maioria do trabalho pesado para mapear corretamente o modelo JSON para os componentes do Angular.

## Inspect o roteamento de SPA no AEM

1. Abra um terminal e pare o **servidor dev do webpack**, se iniciado. Navegue até a raiz do projeto e implante o projeto para AEM usando suas habilidades Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > O projeto do Angular tem algumas regras de impressão muito restritas ativadas. Se a build Maven falhar, verifique o erro e procure por **Lint errors encontrados nos arquivos listados.**. Corrija quaisquer problemas encontrados pelo link e execute novamente o comando Maven.

2. Navegue até a página inicial do SPA em AEM: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) e abra as ferramentas de desenvolvedor do seu navegador. As capturas de tela abaixo são capturadas pelo navegador Google Chrome.

   Atualize a página e você deve ver uma solicitação XHR para `/content/wknd-spa-angular/us/en.model.json`, que é a Raiz SPA. Observe que apenas três páginas filhas são incluídas com base na configuração de profundidade da hierarquia para o modelo Raiz SPA feito anteriormente no tutorial. Isso não inclui **Página 3**.

   ![Solicitação JSON inicial - Raiz SPA](assets/navigation-routing/initial-json-request.png)

3. Com as ferramentas do desenvolvedor abertas, navegue até **Página 3**:

   ![Página 3: Navegar](assets/navigation-routing/page-three-navigation.png)

   Observe que uma nova solicitação de XHR é feita para: `/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![Página três Solicitação XHR](assets/navigation-routing/page-3-xhr-request.png)

   O Gerenciador de modelos de AEM entende que o conteúdo JSON da **Página 3** não está disponível e aciona automaticamente a solicitação XHR adicional.

4. Continue navegando no SPA usando os vários links de navegação. Observe que nenhuma solicitação XHR adicional é feita e que nenhuma atualização de página completa ocorre. Isso agiliza o SPA para o usuário final e reduz solicitações desnecessárias para AEM.

   ![Navegação implementada](assets/navigation-routing/final-navigation-implemented.gif)

5. Experimente com deep links navegando diretamente para: [http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). Observe que o botão Voltar do navegador continua funcionando.

## Parabéns! {#congratulations}

Parabéns, você aprendeu como várias exibições no SPA podem ser suportadas com o mapeamento para AEM páginas com o SDK do Editor SPA. A navegação dinâmica foi implementada usando o roteamento de Angular e adicionada ao componente `Header`.

Você sempre pode visualizar o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) ou verificar o código localmente ao alternar para a ramificação `Angular/navigation-routing-solution`.

### Próximas etapas {#next-steps}

[Criar um componente personalizado](custom-component.md)  - saiba como criar um componente personalizado a ser usado com o Editor de SPA de AEM. Saiba como desenvolver caixas de diálogo do autor e Modelos do Sling para estender o modelo JSON para preencher um componente personalizado.
