---
title: Adicionar navegação e roteamento | Introdução ao AEM SPA Editor e React
description: Saiba como várias exibições no SPA podem ser compatíveis com o mapeamento para páginas do AEM com o SDK do Editor do SPA. A navegação dinâmica é implementada usando o Roteador React e adicionada a um componente Cabeçalho existente.
sub-product: sites
feature: Editor SPA
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2117'
ht-degree: 2%

---


# Adicionar navegação e roteamento {#navigation-routing}

Saiba como várias exibições no SPA podem ser compatíveis com o mapeamento para páginas do AEM com o SDK do Editor do SPA. A navegação dinâmica é implementada usando o Roteador React e adicionada a um componente Cabeçalho existente.

## Objetivo

1. Entenda as opções de roteamento do modelo SPA disponíveis ao usar o Editor SPA.
1. Saiba como usar o [React Router](https://reacttraining.com/react-router/) para navegar entre diferentes visualizações do SPA.
1. Implemente uma navegação dinâmica orientada pela hierarquia de página do AEM.

## O que você vai criar

Este capítulo adicionará um menu de navegação a um componente `Header` existente. O menu de navegação será orientado pela hierarquia de página do AEM e usará o modelo JSON fornecido pelo [Componente principal de navegação](https://docs.adobe.com/content/help/pt/experience-manager-core-components/using/components/navigation.html).

![Navegação implementada](assets/navigation-routing/final-navigation-implemented.gif)

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Obter o código

1. Baixe o ponto de partida para este tutorial via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/navigation-routing-start
   ```

1. Implante a base de código em uma instância do AEM local usando o Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se estiver usando [AEM 6.x](overview.md#compatibility) adicione o perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Instale o pacote concluído para o site de referência tradicional [WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). As imagens fornecidas pelo [site de referência WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) serão reutilizadas no SPA da WKND. O pacote pode ser instalado usando o [Gerenciador de Pacotes do AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![O Gerenciador de Pacotes instala o wknd.all](./assets/map-components/package-manager-wknd-all.png)

Você sempre pode visualizar o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) ou verificar o código localmente ao alternar para a ramificação `React/navigation-routing-solution`.

## Inspecionar atualizações de cabeçalho {#inspect-header}

Em capítulos anteriores, o componente `Header` era adicionado como um componente React puro incluído por meio de `App.js`. Neste capítulo, o componente `Header` foi removido e será adicionado por meio do [Editor de modelo](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html). Isso permitirá que os usuários configurem o menu de navegação do `Header` de dentro do AEM.

>[!NOTE]
>
> Várias atualizações de CSS e JavaScript já foram feitas na base de código para iniciar este capítulo. Para se concentrar nos conceitos principais, não **todas** das alterações de código são discutidas. Você pode visualizar as alterações completas [aqui](https://github.com/adobe/aem-guides-wknd-spa/compare/React/map-components-solution...React/navigation-routing-start).

1. No IDE de sua escolha, abra o projeto inicial do SPA para este capítulo.
1. Abaixo do módulo `ui.frontend` inspecione o arquivo `Header.js` em: `ui.frontend/src/components/Header/Header.js`.

   Várias atualizações foram feitas, incluindo a adição de um `HeaderEditConfig` e um `MapTo` para permitir que o componente seja mapeado para um componente do AEM `wknd-spa-react/components/header`.

   ```js
   /* Header.js */
   ...
   export const HeaderEditConfig = {
       ...
   }
   ...
   MapTo('wknd-spa-react/components/header')(withRouter(Header), HeaderEditConfig);
   ```

1. No módulo `ui.apps` inspecione a definição do componente do AEM `Header`: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-react/components/navigation"
       componentGroup="WKND SPA React - Structure"/>
   ```

   O componente `Header` do AEM herdará toda a funcionalidade do [Componente principal de navegação](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) através da propriedade `sling:resourceSuperType`.

## Adicionar o cabeçalho ao modelo {#add-header-template}

1. Abra um navegador e faça logon no AEM, [http://localhost:4502/](http://localhost:4502/). A base de código inicial já deve ser implantada.
1. Navegue até **Modelo de Página SPA**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Selecione o **Contêiner de layout raiz** mais externo e clique no ícone **Política**. Tenha cuidado **e não** para selecionar o **Contêiner de layout** desbloqueado para criação.

   ![Selecione o ícone de política do contêiner de layout de raiz](assets/navigation-routing/root-layout-container-policy.png)

1. Crie uma nova política chamada **Estrutura de SPA**:

   ![Política de Estrutura do SPA](assets/navigation-routing/spa-policy-update.png)

   Em **Componentes permitidos** > **Geral** > selecione o componente **Contêiner de layout**.

   Em **Componentes permitidos** > **WKND SPA REACT - ESTRUTURA** > selecione o componente **Cabeçalho**:

   ![Selecionar componente do cabeçalho](assets/navigation-routing/select-header-component.png)

   Em **Componentes permitidos** > **WKND SPA REACT - Content** > selecione os componentes **Image** e **Text**. Você deve ter quatro componentes totais selecionados.

   Clique em **Concluído** para salvar as alterações.

1. Atualize a página e adicione o componente **Cabeçalho** acima do **Contêiner de layout** desbloqueado:

   ![adicionar componente Cabeçalho ao modelo](./assets/navigation-routing/add-header-component.gif)

1. Selecione o componente **Cabeçalho** e clique no ícone **Política** para editar a política.
1. Crie uma nova política com um **Título da Política** de **Cabeçalho SPA WKND**.

   Em **Propriedades**:

   * Defina **Raiz de navegação** para `/content/wknd-spa-react/us/en`.
   * Defina **Excluir níveis de raiz** para **1**.
   * Desmarque **Coletar todas as páginas secundárias**.
   * Defina **Profundidade da estrutura de navegação** para **3**.

   ![Configurar política de cabeçalho](assets/navigation-routing/header-policy.png)

   Isso coletará os 2 níveis de navegação abaixo de `/content/wknd-spa-react/us/en`.

1. Depois de salvar suas alterações, você deve ver o `Header` preenchido como parte do modelo:

   ![Componente de cabeçalho preenchido](assets/navigation-routing/populated-header.png)

## Criar páginas filhas

Em seguida, crie páginas adicionais no AEM que servirão como diferentes exibições no SPA. Também verificaremos a estrutura hierárquica do modelo JSON fornecido pelo AEM.

1. Navegue até o console **Sites**: [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Selecione a **Página Inicial de Reação de SPA do WKND** e clique em **Criar** > **Página**:

   ![Criar nova página](assets/navigation-routing/create-new-page.png)

1. Em **Modelo** selecione **Página SPA**. Em **Properties** digite **Page 1** para o **Title** e **page-1** como o nome.

   ![Insira as propriedades da página inicial](assets/navigation-routing/initial-page-properties.png)

   Clique em **Criar** e, na janela pop-up, clique em **Abrir** para abrir a página no Editor de SPA do AEM.

1. Adicione um novo componente **Text** ao **Contêiner de layout** principal. Edite o componente e insira o texto: **Página 1** usando o RTE e o elemento **H1** (você terá que entrar no modo de tela cheia para alterar os elementos de parágrafo)

   ![Exemplo de página de conteúdo 1](assets/navigation-routing/page-1-sample-content.png)

   Você pode adicionar conteúdo adicional, como uma imagem.

1. Retorne ao console Sites do AEM e repita as etapas acima, criando uma segunda página chamada **Página 2** como um irmão da **Página 1**.
1. Por fim, crie uma terceira página, **Page 3**, mas como **child** de **Page 2**. Depois de concluída, a hierarquia do site deve ser semelhante ao seguinte:

   ![Hierarquia de site de exemplo](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. Em uma nova guia, abra a API do modelo JSON fornecida pelo AEM: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Esse conteúdo JSON é solicitado quando o SPA é carregado pela primeira vez. A estrutura externa tem a seguinte aparência:

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
       "/content/wknd-spa-react/us/en/home": {},
       "/content/wknd-spa-react/us/en/home/page-1": {},
       "/content/wknd-spa-react/us/en/home/page-2": {},
       "/content/wknd-spa-react/us/en/home/page-2/page-3": {}
       }
   }
   ```

   Em `:children` você deve ver uma entrada para cada uma das páginas criadas. O conteúdo de todas as páginas está nesta solicitação JSON inicial. Depois que o roteamento de navegação for implementado, as exibições subsequentes do SPA serão carregadas rapidamente, já que o conteúdo já está disponível no lado do cliente.

   Não é recomendável carregar **ALL** do conteúdo de um SPA na solicitação JSON inicial, pois isso atrasaria o carregamento da página inicial. Em seguida, vamos examinar como a profundidade da hierarquia das páginas é coletada.

1. Navegue até o modelo **Raiz do SPA** em: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   Clique no menu **Propriedades da página** > **Política da página**:

   ![Abra a política de página para a Raiz do SPA](assets/navigation-routing/open-page-policy.png)

1. O modelo **Raiz do SPA** tem uma guia **Estrutura Hierárquica** extra para controlar o conteúdo JSON coletado. A **Profundidade da estrutura** determina o quão profundo na hierarquia do site é coletar páginas secundárias abaixo de **root**. Também é possível usar o campo **Structure Patterns** para filtrar páginas adicionais com base em uma expressão regular.

   Atualize a **Profundidade da estrutura** para **2**:

   ![Atualizar profundidade da estrutura](assets/navigation-routing/update-structure-depth.png)

   Clique em **Concluído** para salvar as alterações na política.

1. Abra novamente o modelo JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
       "/content/wknd-spa-react/us/en/home": {},
       "/content/wknd-spa-react/us/en/home/page-1": {},
       "/content/wknd-spa-react/us/en/home/page-2": {}
       }
   }
   ```

   Observe que o caminho **Page 3** foi removido: `/content/wknd-spa-react/us/en/home/page-2/page-3` do modelo JSON inicial.

   Posteriormente, observaremos como o SDK do Editor SPA do AEM pode carregar dinamicamente conteúdo adicional.

## Implementar a navegação

Em seguida, implemente o menu de navegação como parte do `Header`. Podemos adicionar o código diretamente em `Header.js`, mas uma prática melhor com o é evitar componentes grandes. Em vez disso, implementaremos um componente SPA `Navigation` que poderá ser reutilizado posteriormente.

1. Revise o JSON exposto pelo componente `Header` do AEM em [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json):

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-react/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-react/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA React Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-react/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-react/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-react/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-react/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-react/components/header"
   ```

   A natureza hierárquica das páginas do AEM é modelada no JSON que pode ser usado para preencher um menu de navegação. Lembre-se de que o componente `Header` herda toda a funcionalidade do [Componente principal de navegação](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) e que o conteúdo exposto por meio do JSON será mapeado automaticamente para props do React.

1. Abra uma nova janela de terminal e navegue até a pasta `ui.frontend` do projeto SPA. Inicie o **webpack-dev-server** com o comando `npm start`.

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

1. Abra uma nova guia do navegador e navegue até [http://localhost:3000/](http://localhost:3000/).

   O **webpack-dev-server** deve ser configurado para proxy do modelo JSON de uma instância local do AEM (`ui.frontend/.env.development`). Isso nos permitirá codificar diretamente no conteúdo criado no AEM no exercício anterior. Certifique-se de que você esteja autenticado no AEM na mesma sessão de navegação.

   ![alternância de menu funcionando](./assets/navigation-routing/nav-toggle-static.gif)

   O `Header` atualmente possui a funcionalidade de alternância de menu já implementada. Em seguida, implemente o menu de navegação.

1. Retorne ao IDE de sua escolha e abra `Header.js` em `ui.frontend/src/components/Header/Header.js`.
1. Atualize o método `homeLink()` para remover a String codificada e use as props dinâmicas passadas pelo componente AEM:

   ```js
   /* Header.js */
   ...
   get homeLink() {
        //expect a single root defined as part of the navigation
       if(!this.props.items || this.props.items.length !== 1) {
           return null;
       }
   
       return this.props.items[0].url;
   }
   ...
   ```

   O código acima preencherá um url com base no item de navegação raiz configurado pelo componente. `homeLink()` é usado para preencher o logotipo no  `logo()` método e para determinar se o botão voltar deve ser exibido em  `backButton()`.

   Salve as alterações em `Header.js`.

1. Adicione uma linha na parte superior de `Header.js` para importar o componente `Navigation` abaixo das outras importações:

   ```js
   /* Header.js */
   ...
   import Navigation from '../Navigation/Navigation';
   ```

1. Em seguida, atualize o método `get navigation()` para instanciar o componente `Navigation`:

   ```js
   /* Header.js */
   ...
   get navigation() {
       //pass all the props to Navigation component
       return <Navigation {...this.props} />;
   }
   ...
   ```

   Como mencionado anteriormente, em vez de implementar a navegação dentro do `Header` implementaremos a maioria da lógica no componente `Navigation`.  As props do `Header` incluem a estrutura JSON necessária para criar o menu e passamos todas as props.
1. Abra o arquivo `Navigation.js` em `ui.frontend/src/components/Navigation/Navigation.js`.
1. Implemente o método `renderGroupNav(children)`:

   ```js
   /* Navigation.js */
   ...
   renderGroupNav(children) {
   
       if(children === null || children.length < 1 ) {
           return null;
       }
       return (<ul className={this.baseCss + '__group'}>
                   {children.map(
                       (item,index) => { return this.renderNavItem(item,index)}
                   )}
               </ul>
       );
   }
   ...
   ```

   Esse método obtém uma matriz de itens de navegação, `children`, e cria uma lista não ordenada. Em seguida, ele repete a matriz e passa o item para `renderNavItem`, que será implementado em seguida.

1. Implemente o `renderNavItem`:

   ```js
   /* Navigation.js */
   ...
   renderNavItem(item, index) {
       const cssClass = this.baseCss + '__item ' + 
                        this.baseCss + '__item--level-' + item.level + ' ' +
                        (item.active ? ' ' + this.baseCss + '__item--active' : '');
       return (
           <li key={this.baseCss + '__item-' + index} className={cssClass}>
                   { this.renderLink(item) }
                   { this.renderGroupNav(item.children) }
           </li>
       );
   }
   ...
   ```

   Esse método renderiza um item de lista, com classes CSS com base nas propriedades `level` e `active`. O método então chama `renderLink` para criar a tag de âncora. Como o conteúdo `Navigation` é hierárquico, uma estratégia recursiva é usada para chamar o `renderGroupNav` para os filhos do item atual.

1. Implemente o método `renderLink`:

   Adicione um método de importação para o componente [Link](https://reacttraining.com/react-router/web/api/Link), parte do roteador React, na parte superior do arquivo com as outras importações:

   ```js
   import {Link} from "react-router-dom";
   ```

   Em seguida, conclua a implementação do método `renderLink`:

   ```js
   renderLink(item){
       return (
           <Link to={item.url} title={item.title} aria-current={item.active && 'page'}
              className={this.baseCss + '__item-link'}>{item.title}</Link>
       );
   }
   ```

   Observe que, em vez de uma tag de âncora normal, `<a>`, o componente [Link](https://reacttraining.com/react-router/web/api/Link) é usado. Isso garante que uma atualização de página completa não seja acionada e, em vez disso, aproveita o roteador React fornecido pelo SDK JS do Editor SPA do AEM.

1. Salve as alterações em `Navigation.js` e retorne para **webpack-dev-server**: [http://localhost:3000](http://localhost:3000)

   ![Navegação de cabeçalho concluída](assets/navigation-routing/completed-header.png)

   Abra a navegação clicando no botão de alternância do menu e você deverá ver os links de navegação preenchidos. É possível navegar para diferentes exibições do SPA.

## Inspecionar o roteamento SPA

Agora que a navegação foi implementada, inspecione o roteamento no AEM.

1. No IDE, abra o arquivo `index.js` em `ui.frontend/src/index.js`.

   ```js
   /* index.js */
   import { Router } from 'react-router-dom';
   ...
   ...
    ModelManager.initialize().then(pageModel => {
       const history = createBrowserHistory();
       render(
       <Router history={history}>
           <App
           history={history}
           cqChildren={pageModel[Constants.CHILDREN_PROP]}
           cqItems={pageModel[Constants.ITEMS_PROP]}
           cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
           cqPath={pageModel[Constants.PATH_PROP]}
           locationPathname={window.location.pathname}
           />
       </Router>,
       document.getElementById('spa-root')
       );
   });
   ```

   Observe que `App` está envolvido no componente `Router` de [React Router](https://reacttraining.com/react-router/). O `ModelManager`, fornecido pelo SDK JS do Editor do SPA do AEM, adiciona as rotas dinâmicas às Páginas do AEM com base na API do modelo JSON.

1. Abra um terminal, navegue até a raiz do projeto e implante o projeto no AEM usando suas habilidades Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Navegue até a página inicial do SPA no AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) e abra as ferramentas de desenvolvedor do seu navegador. As capturas de tela abaixo são capturadas pelo navegador Google Chrome.

   Atualize a página e você deve ver uma solicitação XHR para `/content/wknd-spa-react/us/en.model.json`, que é a Raiz do SPA. Observe que apenas três páginas filhas são incluídas com base na configuração de profundidade da hierarquia para o modelo de Raiz do SPA feito anteriormente no tutorial. Isso não inclui **Página 3**.

   ![Solicitação JSON inicial - Raiz do SPA](assets/navigation-routing/initial-json-request.png)

1. Com as ferramentas do desenvolvedor abertas, use a navegação `Header` para navegar até **Página 3**:

   ![Página 3: Navegar](assets/navigation-routing/page-three-navigation.png)

   Observe que uma nova solicitação de XHR é feita para: `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Página três Solicitação XHR](assets/navigation-routing/page-3-xhr-request.png)

   O Gerenciador de modelos do AEM entende que o conteúdo JSON da **Página 3** não está disponível e aciona automaticamente a solicitação XHR adicional.

1. Continue navegando no SPA usando os vários links de navegação do componente `Header`. Observe que nenhuma solicitação XHR adicional é feita e que nenhuma atualização de página completa ocorre. Isso torna o SPA rápido para o usuário final e reduz solicitações desnecessárias de volta para o AEM.

   ![Navegação implementada](assets/navigation-routing/final-navigation-implemented.gif)

1. Experimente com deep links navegando diretamente para: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). Observe que o botão Voltar do navegador continua funcionando.

## Parabéns! {#congratulations}

Parabéns, você aprendeu como várias exibições no SPA podem ser suportadas com o mapeamento para páginas do AEM com o SDK do Editor do SPA. A navegação dinâmica foi implementada usando o Roteador React e adicionada ao componente `Header`.

Você sempre pode visualizar o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) ou verificar o código localmente ao alternar para a ramificação `React/navigation-routing-solution`.
