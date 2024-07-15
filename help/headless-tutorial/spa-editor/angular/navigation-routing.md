---
title: Adicionar navegação e roteamento | Introdução ao Editor e Angular SPA do AEM
description: Saiba como várias exibições no SPA são compatíveis usando páginas AEM e o SDK Editor de SPA. A navegação dinâmica é implementada usando rotas de Angular e adicionada a um componente de Cabeçalho existente.
feature: SPA Editor
version: Cloud Service
jira: KT-5312
thumbnail: 5312-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 197a0c1f-4d0a-4b99-ba89-cdff2e6ac4ec
duration: 669
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2531'
ht-degree: 0%

---

# Adicionar navegação e roteamento {#navigation-routing}

Saiba como várias exibições no SPA são compatíveis usando páginas AEM e o SDK Editor de SPA. A navegação dinâmica é implementada usando rotas de Angular e adicionada a um componente de Cabeçalho existente.

## Objetivo

1. Entenda as opções de roteamento do modelo SPA disponíveis ao usar o Editor SPA.
2. Saiba como usar o [roteamento de Angulars](https://angular.io/guide/router) para navegar entre diferentes exibições do SPA.
3. Implemente uma navegação dinâmica orientada pela hierarquia de página do AEM.

## O que você vai criar

Este capítulo adiciona um menu de navegação a um componente `Header` existente. O menu de navegação é orientado pela hierarquia de páginas AEM e usa o modelo JSON fornecido pelo [Componente Principal de Navegação](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![Navegação implementada](assets/navigation-routing/final-navigation-implemented.gif)

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Obter o código

1. Baixe o ponto de partida para este tutorial pelo Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/navigation-routing-start
   ```

2. Implante a base de código em uma instância de AEM local usando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se estiver usando o [AEM 6.x](overview.md#compatibility), adicione o perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Instale o pacote concluído para o [site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) tradicional. As imagens fornecidas pelo [site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) são reutilizadas no SPA WKND. O pacote pode ser instalado usando o [Gerenciador de Pacotes do AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Instalar wknd.all](./assets/map-components/package-manager-wknd-all.png) do Gerenciador de Pacotes

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) ou verificar o código localmente alternando para a ramificação `Angular/navigation-routing-solution`.

## Atualizações do Inspect HeaderComponent {#inspect-header}

Nos capítulos anteriores, o componente `HeaderComponent` foi adicionado como um componente de Angular puro incluído via `app.component.html`. Neste capítulo, o componente `HeaderComponent` é removido do aplicativo e adicionado por meio do [Editor de Modelos](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=pt-BR). Isso permite que os usuários configurem o menu de navegação do `HeaderComponent` de dentro do AEM.

>[!NOTE]
>
> Várias atualizações de CSS e JavaScript já foram feitas na base de código para iniciar este capítulo. Para se concentrar nos conceitos principais, nem **todas** das alterações de código são discutidas. Você pode exibir as alterações completas [aqui](https://github.com/adobe/aem-guides-wknd-spa/compare/Angular/map-components-solution...Angular/navigation-routing-start).

1. No IDE de sua escolha, abra o projeto inicial do SPA para este capítulo.
2. Abaixo do módulo `ui.frontend`, inspecione o arquivo `header.component.ts` em: `ui.frontend/src/app/components/header/header.component.ts`.

   Várias atualizações foram feitas, incluindo a adição de um `HeaderEditConfig` e um `MapTo` para habilitar o componente a ser mapeado para um componente AEM `wknd-spa-angular/components/header`.

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

   Anote a anotação `@Input()` para `items`. `items` conterá uma matriz de objetos de navegação transmitidos do AEM.

3. No módulo `ui.apps`, inspecione a definição do componente AEM `Header`: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-angular/components/navigation"
       componentGroup="WKND SPA Angular - Structure"/>
   ```

   O componente AEM `Header` herdará toda a funcionalidade do [Componente principal de navegação](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html) por meio da propriedade `sling:resourceSuperType`.

## Adicionar o componente de Cabeçalho ao modelo SPA {#add-header-template}

1. Abra um navegador e faça logon no AEM, [http://localhost:4502/](http://localhost:4502/). A base de código inicial já deve estar implantada.
2. Navegue até o **[!UICONTROL Modelo de página do SPA]**: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Selecione o **[!UICONTROL Contêiner de Layout Raiz]** mais externo e clique em seu ícone **[!UICONTROL Política]**. Tenha cuidado **não** para selecionar o **[!UICONTROL Contêiner de layout]** desbloqueado para criação.

   ![Selecione o ícone da política de contêiner de layout raiz](assets/navigation-routing/root-layout-container-policy.png)

4. Copie a política atual e crie uma nova política chamada **[!UICONTROL Estrutura SPA]**:

   ![Política de Estrutura SPA](assets/map-components/spa-policy-update.png)

   Em **[!UICONTROL Componentes Permitidos]** > **[!UICONTROL Geral]** > selecione o componente **[!UICONTROL Contêiner de Layout]**.

   Em **[!UICONTROL Componentes Permitidos]** > **[!UICONTROL ANGULAR SPA WKND - ESTRUTURA]** > selecione o componente **[!UICONTROL Cabeçalho]**:

   ![Selecionar componente de cabeçalho](assets/map-components/select-header-component.png)

   Em **[!UICONTROL Componentes Permitidos]** > **[!UICONTROL ANGULAR WKND SPA - Conteúdo]** > selecione os componentes **[!UICONTROL Imagem]** e **[!UICONTROL Texto]**. Você deve ter um total de quatro componentes selecionados.

   Clique em **[!UICONTROL Concluído]** para salvar as alterações.

5. **Atualizar** a página. Adicione o componente **[!UICONTROL Cabeçalho]** acima do **[!UICONTROL Contêiner de Layout]** desbloqueado:

   ![adicionar componente de Cabeçalho ao modelo](./assets/navigation-routing/add-header-component.gif)

6. Selecione o componente **[!UICONTROL Cabeçalho]** e clique em seu ícone **Política** para editar a política.

   ![Clique na política de cabeçalho](assets/navigation-routing/header-policy-icon.png)

7. Crie uma nova política com um **[!UICONTROL Título da Política]** de **&quot;Cabeçalho SPA WKND&quot;**.

   Em **[!UICONTROL Propriedades]**:

   * Defina a **[!UICONTROL Raiz de Navegação]** como `/content/wknd-spa-angular/us/en`.
   * Defina os **[!UICONTROL Excluir Níveis de Raiz]** para **1**.
   * Desmarcar **[!UICONTROL Coletar todas as páginas secundárias]**.
   * Defina a **[!UICONTROL Profundidade da Estrutura de Navegação]** como **3**.

   ![Configurar Política de Cabeçalho](assets/navigation-routing/header-policy.png)

   Isso coletará os 2 níveis de navegação abaixo de `/content/wknd-spa-angular/us/en`.

8. Depois de salvar as alterações, você deverá ver o `Header` preenchido como parte do modelo:

   ![Componente de cabeçalho preenchido](assets/navigation-routing/populated-header.png)

## Criar páginas secundárias

Em seguida, crie páginas adicionais no AEM que servirão como as diferentes visualizações no SPA. Também vamos inspecionar a estrutura hierárquica do modelo JSON fornecido pelo AEM.

1. Navegue até o console **Sites**: [http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-angular/us/en/home). Selecione a **Página Inicial do Angular do WKND SPA** e clique em **[!UICONTROL Criar]** > **[!UICONTROL Página]**:

   ![Criar nova página](assets/navigation-routing/create-new-page.png)

2. Em **[!UICONTROL Modelo]**, selecione **[!UICONTROL Página SPA]**. Em **[!UICONTROL Propriedades]**, digite **&quot;Página 1&quot;** para o **[!UICONTROL Título]** e **&quot;página-1&quot;** como o nome.

   ![Insira as propriedades da página inicial](assets/navigation-routing/initial-page-properties.png)

   Clique em **[!UICONTROL Criar]** e, na janela pop-up da caixa de diálogo, clique em **[!UICONTROL Abrir]** para abrir a página no Editor de SPA AEM.

3. Adicionar um novo componente **[!UICONTROL Texto]** ao **[!UICONTROL Contêiner de Layout]** principal. Edite o componente e insira o texto: **&quot;Página 1&quot;** usando o RTE e o elemento **H1** (você terá que entrar no modo de tela cheia para alterar os elementos de parágrafo)

   ![Exemplo de página de conteúdo 1](assets/navigation-routing/page-1-sample-content.png)

   Fique à vontade para adicionar mais conteúdo, como uma imagem.

4. Retorne ao console do AEM Sites e repita as etapas acima, criando uma segunda página denominada **&quot;Página 2&quot;** como irmã da **Página 1**. Adicionar conteúdo à **Página 2** para identificá-lo facilmente.
5. Por fim, crie uma terceira página, **&quot;Página 3&quot;**, mas como **criança** da **Página 2**. Depois de concluída, a hierarquia do site deve ser semelhante ao seguinte:

   ![Hierarquia do Site de Exemplo](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

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

   Em `:children` você deve ver uma entrada para cada uma das páginas criadas. O conteúdo de todas as páginas está nesta solicitação JSON inicial. Depois que o roteamento de navegação é implementado, as exibições subsequentes do SPA são carregadas rapidamente, já que o conteúdo já está disponível no lado do cliente.

   Não é recomendável carregar **TODOS** do conteúdo de um SPA na solicitação JSON inicial, pois isso retardaria o carregamento da página inicial. Em seguida, vamos analisar como a profundidade da hierarquia das páginas é coletada.

7. Navegue até o modelo **Raiz SPA** em: [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-app-template/structure.html).

   Clique no menu **[!UICONTROL Propriedades da página]** > **[!UICONTROL Política da página]**:

   ![Abrir a política de página para Raiz SPA](assets/navigation-routing/open-page-policy.png)

8. O modelo **Raiz SPA** tem uma guia **[!UICONTROL Estrutura Hierárquica]** extra para controlar o conteúdo JSON coletado. A **[!UICONTROL Profundidade da Estrutura]** determina a profundidade na hierarquia do site para coletar páginas secundárias abaixo da **raiz**. Você também pode usar o campo **[!UICONTROL Padrões de estrutura]** para filtrar páginas adicionais com base em uma expressão regular.

   Atualize a **[!UICONTROL Profundidade da Estrutura]** para **&quot;2&quot;**:

   ![Atualizar profundidade da estrutura](assets/navigation-routing/update-structure-depth.png)

   Clique em **[!UICONTROL Concluído]** para salvar as alterações na política.

9. Reabra o modelo JSON [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

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

   Observe que o caminho **Página 3** foi removido: `/content/wknd-spa-angular/us/en/home/page-2/page-3` do modelo JSON inicial.

   Posteriormente, observaremos como o SDK do editor SPA AEM pode carregar dinamicamente conteúdo adicional.

## Implementar a navegação

Em seguida, implemente o menu de navegação com um novo `NavigationComponent`. Podemos adicionar o código diretamente em `header.component.html`, mas uma prática recomendada é evitar componentes grandes. Em vez disso, implemente um `NavigationComponent` que possa ser reutilizado posteriormente.

1. Revise o JSON exposto pelo componente AEM `Header` em [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json):

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

   A natureza hierárquica das páginas AEM é modelada no JSON que pode ser usado para preencher um menu de navegação. Lembre-se de que o componente `Header` herda toda a funcionalidade do [Componente principal de Navegação](https://www.aemcomponents.dev/content/core-components-examples/library/core-structure/navigation.html) e o conteúdo exposto por meio do JSON é mapeado automaticamente para a anotação do Angular `@Input`.

2. Abra uma nova janela de terminal e navegue até a pasta `ui.frontend` do projeto SPA. Crie um novo `NavigationComponent` usando a ferramenta Angular CLI:

   ```shell
   $ cd ui.frontend
   $ ng generate component components/navigation
   CREATE src/app/components/navigation/navigation.component.scss (0 bytes)
   CREATE src/app/components/navigation/navigation.component.html (25 bytes)
   CREATE src/app/components/navigation/navigation.component.spec.ts (656 bytes)
   CREATE src/app/components/navigation/navigation.component.ts (286 bytes)
   UPDATE src/app/app.module.ts (2032 bytes)
   ```

3. Em seguida, crie uma classe chamada `NavigationLink` usando a CLI do Angular no diretório `components/navigation` recém-criado:

   ```shell
   $ cd src/app/components/navigation/
   $ ng generate class NavigationLink
   CREATE src/app/components/navigation/navigation-link.spec.ts (187 bytes)
   CREATE src/app/components/navigation/navigation-link.ts (32 bytes)
   ```

4. Retorne ao IDE de sua escolha e abra o arquivo em `navigation-link.ts` em `/src/app/components/navigation/navigation-link.ts`.

   ![Abrir o arquivo navigation-link.ts](assets/navigation-routing/ide-navigation-link-file.png)

5. Popular `navigation-link.ts` com o seguinte:

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

   É uma classe simples para representar um link de navegação individual. No construtor de classe, esperamos que `data` seja o objeto JSON transmitido pelo AEM. Essa classe é usada em `NavigationComponent` e `HeaderComponent` para preencher facilmente a estrutura de navegação.

   Nenhuma transformação de dados é executada, essa classe é criada principalmente para digitar o modelo JSON. Observe que `this.children` é digitado como `NavigationLink[]` e que o construtor cria recursivamente novos objetos `NavigationLink` para cada um dos itens na matriz `children`. Lembre-se de que o modelo JSON para `Header` é hierárquico.

6. Abra o arquivo `navigation-link.spec.ts`. Este é o arquivo de teste para a classe `NavigationLink`. Atualize-o com o seguinte:

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

   Observe que `const data` segue o mesmo modelo JSON inspecionado anteriormente para um único link. Isso está longe de ser um teste de unidade robusto, no entanto, deve ser suficiente para testar o construtor de `NavigationLink`.

7. Abra o arquivo `navigation.component.ts`. Atualize-o com o seguinte:

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

   `NavigationComponent` espera um `object[]` chamado `items` que seja o modelo JSON do AEM. Esta classe expõe um único método `get navigationLinks()` que retorna uma matriz de `NavigationLink` objetos.

8. Abra o arquivo `navigation.component.html` e atualize-o com o seguinte:

   ```html
   <ul *ngIf="navigationLinks && navigationLinks.length > 0" class="navigation__group">
       <ng-container *ngTemplateOutlet="recursiveListTmpl; context:{ links: navigationLinks }"></ng-container>
   </ul>
   ```

   Isso gera um `<ul>` inicial e chama o método `get navigationLinks()` de `navigation.component.ts`. Um `<ng-container>` é usado para fazer uma chamada para um modelo chamado `recursiveListTmpl` e passar o `navigationLinks` como uma variável chamada `links`.

   Adicionar o `recursiveListTmpl` em seguida:

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

   Aqui, o restante da renderização do link de navegação é implementado. Observe que a variável `link` é do tipo `NavigationLink` e todos os métodos/propriedades criados por essa classe estão disponíveis. [`[routerLink]`](https://angular.io/api/router/RouterLink) é usado em vez do atributo `href` normal. Isso nos permite vincular a rotas específicas no aplicativo, sem uma atualização de página inteira.

   A parte recursiva da navegação também é implementada com a criação de outro `<ul>` se o `link` atual tiver uma matriz `children` não vazia.

9. Atualize `navigation.component.spec.ts` para adicionar suporte a `RouterTestingModule`:

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

   É necessário adicionar `RouterTestingModule` porque o componente usa `[routerLink]`.

10. Atualize `navigation.component.scss` para adicionar alguns estilos básicos a `NavigationComponent`:

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

Agora que o `NavigationComponent` foi implementado, o `HeaderComponent` deve ser atualizado para fazer referência a ele.

1. Abra um terminal e navegue até a pasta `ui.frontend` no projeto SPA. Inicie o **servidor de desenvolvimento do webpack**:

   ```shell
   $ npm start
   ```

2. Abra uma guia do navegador e navegue até [http://localhost:4200/](http://localhost:4200/).

   O **servidor de desenvolvimento do webpack** deve ser configurado para usar o proxy do modelo JSON de uma instância local do AEM (`ui.frontend/proxy.conf.json`). Isso nos permitirá codificar diretamente em relação ao conteúdo criado no AEM anteriormente no tutorial.

   ![alternância de menu funcionando](./assets/navigation-routing/nav-toggle-static.gif)

   Atualmente, o `HeaderComponent` já tem a funcionalidade de alternância de menu implementada. Em seguida, adicione o componente de navegação.

3. Retorne ao IDE de sua escolha e abra o arquivo `header.component.ts` em `ui.frontend/src/app/components/header/header.component.ts`.
4. Atualize o método `setHomePage()` para remover a String codificada e usar as props dinâmicas transmitidas pelo componente AEM:

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

   Uma nova instância de `NavigationLink` é criada com base em `items[0]`, a raiz do modelo JSON de navegação transmitido pelo AEM. `this.route.snapshot.data.path` retorna o caminho da rota de Angular atual. Este valor é usado para determinar se a rota atual é a **Página Inicial**. `this.homePageUrl` é usado para preencher o link de âncora no **logotipo**.

5. Abra `header.component.html` e substitua o espaço reservado estático para a navegação por uma referência ao `NavigationComponent` recém-criado:

   ```diff
       <div class="header-navigation">
           <div class="navigation">
   -            Navigation Placeholder
   +           <app-navigation [items]="items"></app-navigation>
           </div>
       </div>
   ```

   O atributo `[items]=items` passa o `@Input() items` de `HeaderComponent` para `NavigationComponent`, onde ele compilará a navegação.

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

   Como o `NavigationComponent` agora é usado como parte do `HeaderComponent`, ele precisa ser declarado como parte da base de testes.

7. Salve as alterações nos arquivos abertos e retorne ao **servidor de desenvolvimento do webpack**: [http://localhost:4200/](http://localhost:4200/)

   ![Navegação de cabeçalho concluída](assets/navigation-routing/completed-header.png)

   Abra a navegação clicando na opção de menu e você verá os links de navegação preenchidos. Você deve ser capaz de navegar para diferentes visualizações do SPA.

## Entender o roteamento SPA

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

   A matriz `routes: Routes = [];` define as rotas ou os caminhos de navegação para mapeamentos de componentes de Angular.

   `AemPageMatcher` é um roteador de Angular personalizado [UrlMatcher](https://angular.io/api/router/UrlMatcher), que corresponde a qualquer item que &quot;se pareça&quot; com uma página no AEM que faz parte deste aplicativo de Angular.

   `PageComponent` é o Componente de Angular que representa uma Página no AEM e usado para renderizar as rotas correspondentes. O `PageComponent` é revisado posteriormente no tutorial.

   `AemPageDataResolver`, fornecido pelo SDK JS do Editor SPA AEM, é um [Resolvedor de Roteador de Angular AEM AEM](https://angular.io/api/router/Resolve) personalizado usado para transformar a URL da rota, que é o caminho no, incluindo a extensão .html, para o caminho do recurso no, que é o caminho da página menos a extensão.

   Por exemplo, o `AemPageDataResolver` transforma a URL de uma rota de `content/wknd-spa-angular/us/en/home.html` em um caminho de `/content/wknd-spa-angular/us/en/home`. Isso é usado para resolver o conteúdo da página com base no caminho na API do modelo JSON.

   `AemPageRouteReuseStrategy`, fornecido pelo SDK JS do Editor SPA AEM, é uma [RouteReuseStrategy](https://angular.io/api/router/RouteReuseStrategy) personalizada que impede a reutilização de `PageComponent` entre rotas. Caso contrário, o conteúdo da página &quot;A&quot; poderá ser exibido ao navegar para a página &quot;B&quot;.

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

   O `PageComponent` é necessário para processar o JSON recuperado do AEM e é usado como o componente do Angular para renderizar as rotas.

   `ActivatedRoute`, que é fornecido pelo módulo Angular Router, contém o estado que indica qual conteúdo JSON da Página AEM deve ser carregado nesta instância do componente Página de Angular.

   `ModelManagerService`, obtém os dados JSON com base na rota e mapeia os dados para as variáveis de classe `path`, `items`, `itemsOrder`. Eles serão passados para o [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md)

3. Abrir o arquivo `page.component.html` em `ui.frontend/src/app/components/page/`

   ```html
   <aem-page 
       class="structure-page" 
       [attr.data-cq-page-path]="path" 
       [cqPath]="path" 
       [cqItems]="items" 
       [cqItemsOrder]="itemsOrder">
   </aem-page>
   ```

   `aem-page` inclui o [AEMPageComponent](https://www.npmjs.com/package/@adobe/cq-angular-editable-components#aempagecomponent.md). As variáveis `path`, `items` e `itemsOrder` são passadas para `AEMPageComponent`. O `AemPageComponent`, fornecido por meio do SDK JavaScript do Editor de SPA, iterará sobre esses dados e instanciará dinamicamente os componentes do Angular com base nos dados JSON, conforme visto no [tutorial de Componentes do mapa](./map-components.md).

   O `PageComponent` é na verdade apenas um proxy para o `AEMPageComponent` e é o `AEMPageComponent` que faz a maioria do trabalho pesado para mapear corretamente o modelo JSON para os componentes do Angular.

## Inspect: o roteamento do SPA no AEM

1. Abra um terminal e pare o **servidor de desenvolvimento do webpack**, se iniciado. Navegue até a raiz do projeto e implante o projeto no AEM usando suas habilidades em Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!CAUTION]
   >
   > O projeto Angular tem algumas regras de lista muito rigorosas ativadas. Se a compilação Maven falhar, verifique o erro e procure **erros Lint encontrados nos arquivos listados.**. Corrija todos os problemas encontrados pelo linter e execute novamente o comando Maven.

2. Navegue até a página inicial do SPA no AEM: [http://localhost:4502/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/content/wknd-spa-angular/us/en/home.html) e abra as ferramentas de desenvolvedor do seu navegador. As capturas de tela abaixo são feitas no navegador Google Chrome.

   Atualize a página e você verá uma solicitação XHR para `/content/wknd-spa-angular/us/en.model.json`, que é a Raiz SPA. Observe que apenas três páginas secundárias são incluídas com base na configuração de profundidade da hierarquia para o modelo Raiz SPA definido anteriormente no tutorial. Isso não inclui a **Página 3**.

   ![Solicitação JSON inicial - Raiz SPA](assets/navigation-routing/initial-json-request.png)

3. Com as ferramentas do desenvolvedor abertas, navegue até a **Página 3**:

   ![Página 3 Navegar](assets/navigation-routing/page-three-navigation.png)

   Observe que uma nova solicitação XHR é feita para: `/content/wknd-spa-angular/us/en/home/page-2/page-3.model.json`

   ![Solicitação XHR da página três](assets/navigation-routing/page-3-xhr-request.png)

   O Gerenciador de Modelos AEM entende que o conteúdo JSON da **Página 3** não está disponível e aciona automaticamente a solicitação XHR adicional.

4. Continue navegando no SPA usando os vários links de navegação. Observe que nenhuma solicitação XHR adicional é feita e que nenhuma atualização de página completa ocorre. Isso agiliza o SPA para o usuário final e reduz solicitações desnecessárias de volta ao AEM.

   ![Navegação implementada](assets/navigation-routing/final-navigation-implemented.gif)

5. Experimente os deep links navegando diretamente para: [http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-angular/us/en/home/page-2.html). Observe que o botão Voltar do navegador continua funcionando.

## Parabéns. {#congratulations}

Parabéns, você aprendeu como várias exibições no SPA podem ser suportadas pelo mapeamento para páginas AEM com o SDK do Editor do SPA. A navegação dinâmica foi implementada com o roteamento de Angulars e adicionada ao componente `Header`.

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/navigation-routing-solution) ou verificar o código localmente alternando para a ramificação `Angular/navigation-routing-solution`.

### Próximas etapas {#next-steps}

AEM [Criar um componente personalizado](custom-component.md) - Saiba como criar um componente personalizado a ser usado com o Editor de SPA. Saiba como desenvolver caixas de diálogo de criação e Modelos Sling para estender o modelo JSON e preencher um componente personalizado.
