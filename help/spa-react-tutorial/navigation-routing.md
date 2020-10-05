---
title: Adicionar navegação e roteamento | Introdução ao Editor SPA AEM e Reação
description: Saiba como várias visualizações no SPA podem ser suportadas pelo mapeamento para AEM páginas com o SDK do Editor SPA. A navegação dinâmica é implementada usando o Roteador de Reação e adicionada a um componente de Cabeçalho existente.
sub-product: sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
translation-type: tm+mt
source-git-commit: 892cb074814eabd347ba7aef883721df0ee4d431
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Adicionar navegação e roteamento {#navigation-routing}

Saiba como várias visualizações no SPA podem ser suportadas pelo mapeamento para AEM páginas com o SDK do Editor SPA. A navegação dinâmica é implementada usando o Roteador de Reação e adicionada a um componente de Cabeçalho existente.

## Objetivo

1. Entenda as opções de roteamento do modelo SPA disponíveis ao usar o Editor SPA.
1. Saiba como usar o Roteador [](https://reacttraining.com/react-router/) React para navegar entre diferentes visualizações do SPA.
1. Implemente uma navegação dinâmica orientada pela hierarquia de páginas AEM.

## O que você vai criar

Este capítulo adicionará um menu de navegação a um `Header` componente existente. O menu de navegação será conduzido pela hierarquia de páginas AEM e usará o modelo JSON fornecido pelo Componente [principal de](https://docs.adobe.com/content/help/pt/experience-manager-core-components/using/components/navigation.html)navegação.

![Navegação implementada](assets/navigation-routing/final-navigation-implemented.gif)

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um ambiente [de desenvolvimento](overview.md#local-dev-environment)local.

### Obter o código

1. Baixe o ponto de partida para este tutorial via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/navigation-routing-start
   ```

1. Implante a base de código para uma instância AEM local usando Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Se estiver usando [AEM 6.x](overview.md#compatibility) , adicione o `classic` perfil:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Instale o pacote finalizado para o site [de referência](https://github.com/adobe/aem-guides-wknd/releases/latest)WKND tradicional. As imagens fornecidas pelo site [de referência](https://github.com/adobe/aem-guides-wknd/releases/latest) WKND serão reutilizadas no SPA WKND. O pacote pode ser instalado usando [AEM Gerenciador](http://localhost:4502/crx/packmgr/index.jsp)de pacotes.

   ![O gerenciador de pacotes instala wknd.all](./assets/map-components/package-manager-wknd-all.png)

Você sempre pode visualização o código finalizado no [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) ou fazer check-out do código localmente ao alternar para a ramificação `React/navigation-routing-solution`.

## Atualizações do cabeçalho do Inspect {#inspect-header}

Em capítulos anteriores, o `Header` componente foi adicionado como um componente puro React incluído via `App.js`. Neste capítulo, o `Header` componente foi removido e será adicionado por meio do Editor [de](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)modelos. Isso permitirá que os usuários configurem o menu de navegação do `Header` de dentro do AEM.

>[!NOTE]
>
> Várias atualizações de CSS e JavaScript já foram feitas na base de código para start deste capítulo. Para focalizar os conceitos principais, nem **todas** as alterações de código são discutidas. Você pode visualização as mudanças completas [aqui](https://github.com/adobe/aem-guides-wknd-spa/compare/React/map-components-solution...React/navigation-routing-start).

1. No IDE de sua escolha, abra o projeto inicial do SPA para este capítulo.
1. Abaixo do `ui.frontend` módulo, inspecione o arquivo `Header.js` em: `ui.frontend/src/components/Header/Header.js`.

   Várias atualizações foram feitas, incluindo a adição de um `HeaderEditConfig` e um `MapTo` para permitir que o componente seja mapeado para um componente AEM `wknd-spa-react/components/header`.

   ```js
   /* Header.js */
   ...
   export const HeaderEditConfig = {
       ...
   }
   ...
   MapTo('wknd-spa-react/components/header')(withRouter(Header), HeaderEditConfig);
   ```

1. No `ui.apps` módulo inspecione a definição do componente do AEM `Header` : `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-react/components/navigation"
       componentGroup="WKND SPA React - Structure"/>
   ```

   O componente AEM `Header` herdará toda a funcionalidade do Componente [principal de](https://docs.adobe.com/content/help/pt/experience-manager-core-components/using/components/navigation.html) navegação por meio da `sling:resourceSuperType` propriedade.

## Adicionar o cabeçalho ao modelo {#add-header-template}

1. Abra um navegador e faça logon no AEM, [http://localhost:4502/](http://localhost:4502/). A base de código inicial já deve ser implantada.
1. Navegue até o Modelo **de Página** SPA: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Selecione o Container **de layout** raiz mais externo e clique no ícone **Política** . Tenha cuidado para **não** selecionar o Container **de** layout desbloqueado para criação.

   ![Selecione o ícone da política de container do layout raiz](assets/navigation-routing/root-layout-container-policy.png)

1. Crie uma nova política chamada Estrutura **SPA**:

   ![Política de Estrutura do SPA](assets/navigation-routing/spa-policy-update.png)

   Em **Componentes** permitidos > **Geral** > selecione o componente de Container **de** layout.

   Em Componentes **** permitidos > **WKND SPA REACT - STRUCTURE** > selecione o componente **Cabeçalho** :

   ![Selecionar componente do cabeçalho](assets/navigation-routing/select-header-component.png)

   Em Componentes **** permitidos > **WKND SPA REACT - Conteúdo** > selecione os componentes **Imagem** e **Texto** . Você deve ter quatro componentes totais selecionados.

   Click **Done** to save the changes.

1. Atualize a página e adicione o componente **Cabeçalho** acima do Container **de** layout não bloqueado:

   ![adicionar componente Cabeçalho ao modelo](./assets/navigation-routing/add-header-component.gif)

1. Selecione o componente **Cabeçalho** e clique no ícone **Política** para editar a política.
1. Crie uma nova política com um Título **de** política do cabeçalho SPA **WKND**.

   Em **Propriedades**:

   * Defina a **Raiz** de navegação como `/content/wknd-spa-react/us/en`.
   * Defina **Excluir níveis** raiz como **1**.
   * Uncheck **Collect all child pages**.
   * Defina a Profundidade **da estrutura de** navegação como **3**.

   ![Configurar política de cabeçalho](assets/navigation-routing/header-policy.png)

   Isso coletará os 2 níveis de navegação abaixo `/content/wknd-spa-react/us/en`.

1. Depois de salvar as alterações, você deve ver o preenchimento `Header` como parte do modelo:

   ![Componente de cabeçalho preenchido](assets/navigation-routing/populated-header.png)

## Criar páginas secundárias

Em seguida, crie páginas adicionais no AEM que servirão como visualizações diferentes no SPA. Inspecionaremos também a estrutura hierárquica do modelo JSON fornecido pela AEM.

1. Navegue até o console **Sites** : [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Selecione o Home page **WKND SPA React e clique em** Criar **>** Página ****:

   ![Criar nova página](assets/navigation-routing/create-new-page.png)

1. Em **Modelo** , selecione Página **** SPA. Em **Propriedades** , insira a **Página 1** para o **Título** e a **página 1** como nome.

   ![Insira as propriedades iniciais da página](assets/navigation-routing/initial-page-properties.png)

   Clique em **Criar** e, no pop-up de diálogo, clique em **Abrir** para abrir a página no Editor SPA AEM.

1. Adicione um novo componente de **Texto** ao Container **principal de** Layout. Edite o componente e insira o texto: **Página 1** usando o RTE e o elemento **H1** (será necessário entrar no modo de tela cheia para alterar os elementos de parágrafo)

   ![Exemplo de página de conteúdo 1](assets/navigation-routing/page-1-sample-content.png)

   Sinta-se à vontade para adicionar conteúdo adicional, como uma imagem.

1. Retorne ao console do AEM Sites e repita as etapas acima, criando uma segunda página chamada **Página 2** como um irmão da **Página 1**.
1. Por fim, crie uma terceira página, **Página 3** , mas como **filho** da **Página 2**. Após a conclusão, a hierarquia do site deve ser semelhante ao seguinte:

   ![Exemplo de hierarquia do site](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. Em uma nova guia, abra a API do modelo JSON fornecida pela AEM: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Este conteúdo JSON é solicitado quando o SPA é carregado pela primeira vez. A estrutura externa tem a seguinte aparência:

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

   Em `:children` você deve ver uma entrada para cada página criada. O conteúdo de todas as páginas está nesta solicitação JSON inicial. Depois que o roteamento de navegação for implementado, as visualizações subsequentes do SPA serão carregadas rapidamente, já que o conteúdo já está disponível no cliente.

   Não é recomendável carregar **TODO** o conteúdo de um SPA na solicitação JSON inicial, pois isso reduziria o carregamento da página inicial. Em seguida, vamos ver como a profundidade de hierarquia das páginas é coletada.

1. Navegue até o modelo Raiz **do** SPA em: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   Clique no menu **Propriedades da** página > Política **** de página:

   ![Abrir a política de página para a raiz SPA](assets/navigation-routing/open-page-policy.png)

1. O modelo raiz **** SPA tem uma guia Estrutura **** hierárquica extra para controlar o conteúdo JSON coletado. A Profundidade **da** estrutura determina a profundidade na hierarquia do site para coletar páginas secundárias abaixo da **raiz**. Você também pode usar o campo Padrões **de** estrutura para filtrar páginas adicionais com base em uma expressão regular.

   Atualize a profundidade da **estrutura** para **2**:

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

   Observe que o caminho da **Página 3** foi removido: `/content/wknd-spa-react/us/en/home/page-2/page-3` do modelo JSON inicial.

   Posteriormente, observaremos como o AEM SPA Editor SDK pode carregar dinamicamente conteúdo adicional.

## Implementar a navegação

Em seguida, implemente o menu de navegação como parte do `Header`. Poderíamos adicionar o código diretamente, `Header.js` mas uma prática melhor é evitar componentes grandes. Em vez disso, implementaremos um componente `Navigation` SPA que poderá ser reutilizado posteriormente.

1. Examine o JSON exposto pelo `Header` componente AEM em [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json):

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

   A natureza hierárquica das páginas AEM é modelada no JSON que pode ser usada para preencher um menu de navegação. Lembre-se de que o `Header` componente herda toda a funcionalidade do Componente [principal de](https://docs.adobe.com/content/help/pt/experience-manager-core-components/using/components/navigation.html) navegação e que o conteúdo exposto por meio do JSON será automaticamente mapeado para React props.

1. Abra uma nova janela de terminal e navegue até a `ui.frontend` pasta do projeto SPA. Start o **webpack-dev-server** com o comando `npm start`.

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

1. Abra uma nova guia do navegador e navegue até [http://localhost:3000/](http://localhost:3000/).

   O **webpack-dev-server** deve ser configurado para proxy do modelo JSON de uma instância local do AEM (`ui.frontend/.env.development`). Isso permitirá codificar diretamente o conteúdo criado no AEM do exercício anterior. Certifique-se de estar autenticado em AEM na mesma sessão de navegação.

   ![alternância de menu funcionando](./assets/navigation-routing/nav-toggle-static.gif)

   A funcionalidade de alternância de menus já foi implementada no `Header` momento. Em seguida, implemente o menu de navegação.

1. Retorne ao IDE de sua escolha e abra o `Header.js` em `ui.frontend/src/components/Header/Header.js`.
1. Atualize o `homeLink()` método para remover a string codificada e use os props dinâmicos passados pelo componente AEM:

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

   O código acima preencherá um url com base no item de navegação raiz configurado pelo componente. `homeLink()` é usado para preencher o logotipo no `logo()` método e para determinar se o botão voltar deve ser exibido em `backButton()`.

   Save changes to `Header.js`.

1. Adicione uma linha na parte superior de `Header.js` para importar o `Navigation` componente abaixo das outras importações:

   ```js
   /* Header.js */
   ...
   import Navigation from '../Navigation/Navigation';
   ```

1. Em seguida, atualize o `get navigation()` método para instanciar o `Navigation` componente:

   ```js
   /* Header.js */
   ...
   get navigation() {
       //pass all the props to Navigation component
       return <Navigation {...this.props} />;
   }
   ...
   ```

   Como mencionado anteriormente, em vez de implementar a navegação dentro do `Header` componente, implementaremos a maior parte da lógica no `Navigation` componente.  As props do `Header` incluem a estrutura JSON necessária para criar o menu; nós passamos por todas as props.
1. Open the file `Navigation.js` at `ui.frontend/src/components/Navigation/Navigation.js`.
1. Implemente o `renderGroupNav(children)` método:

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

   Esse método pega uma matriz de itens de navegação `children`e cria uma lista não ordenada. Em seguida, ele repete o array e passa o item para o `renderNavItem`, que será implementado em seguida.

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

   Esse método renderiza um item de lista, com classes CSS com base em propriedades `level` e `active`. O método então chama `renderLink` para criar a tag de âncora. Como o `Navigation` conteúdo é hierárquico, uma estratégia recursiva é usada para chamar os filhos `renderGroupNav` do item atual.

1. Implemente o `renderLink` método:

   Adicione um método de importação para o componente [Link](https://reacttraining.com/react-router/web/api/Link) , parte do roteador React, na parte superior do arquivo com as outras importações:

   ```js
   import {Link} from "react-router-dom";
   ```

   Em seguida, conclua a implementação do `renderLink` método:

   ```js
   renderLink(item){
       return (
           <Link to={item.url} title={item.title} aria-current={item.active && 'page'}
              className={this.baseCss + '__item-link'}>{item.title}</Link>
       );
   }
   ```

   Observe que, em vez de uma tag de âncora normal, `<a>`o componente [Link](https://reacttraining.com/react-router/web/api/Link) é usado. Isso garante que a atualização completa da página não seja acionada e, em vez disso, aproveita o roteador React fornecido pelo SDK JS do Editor SPA AEM.

1. Salve as alterações `Navigation.js` e retorne ao **webpack-dev-server**: [http://localhost:3000](http://localhost:3000)

   ![Navegação de cabeçalho concluída](assets/navigation-routing/completed-header.png)

   Abra a navegação clicando no menu para alternar e você deverá ver os links de navegação preenchidos. Você deve ser capaz de navegar para diferentes visualizações do SPA.

## Inspect o Roteamento SPA

Agora que a navegação foi implementada, inspecione o roteamento em AEM.

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

   Observe que o componente `App` está embutido no `Router` Roteador [React](https://reacttraining.com/react-router/). O `ModelManager`, fornecido pelo SDK JS do Editor SPA AEM, adiciona as rotas dinâmicas às Páginas AEM com base na API do modelo JSON.

1. Abra um terminal, navegue até a raiz do projeto e implante o projeto para AEM usando suas habilidades Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Navegue até a página inicial do SPA em AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) e abra as ferramentas de desenvolvedor do seu navegador. As capturas de tela abaixo são capturadas do navegador Google Chrome.

   Atualize a página e você deverá ver uma solicitação XHR para `/content/wknd-spa-react/us/en.model.json`, que é a Raiz do SPA. Observe que apenas três páginas secundárias são incluídas com base na configuração de profundidade da hierarquia para o modelo Raiz do SPA feito anteriormente no tutorial. Isso não inclui a **Página 3**.

   ![Solicitação JSON inicial - Raiz do SPA](assets/navigation-routing/initial-json-request.png)

1. Com as ferramentas do desenvolvedor abertas, use a `Header` navegação para navegar até a **Página 3**:

   ![Página 3 Navegação](assets/navigation-routing/page-three-navigation.png)

   Observe que uma nova solicitação XHR é feita para: `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Página três Solicitação XHR](assets/navigation-routing/page-3-xhr-request.png)

   O AEM Model Manager entende que o conteúdo JSON da **Página 3** não está disponível e aciona automaticamente a solicitação XHR adicional.

1. Continue navegando no SPA usando os vários links de navegação do `Header` componente. Observe que nenhuma solicitação XHR adicional é feita e que nenhuma atualização de página completa ocorre. Isso torna o SPA rápido para o usuário final e reduz solicitações desnecessárias de volta para o AEM.

   ![Navegação implementada](assets/navigation-routing/final-navigation-implemented.gif)

1. Experimente links profundos navegando diretamente para: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). Observe que o botão Voltar do navegador continua funcionando.

## Parabéns! {#congratulations}

Parabéns, você aprendeu como várias visualizações no SPA podem ser suportadas pelo mapeamento para AEM páginas com o SDK do editor do SPA. A navegação dinâmica foi implementada usando o Roteador React e adicionada ao `Header` componente.

Você sempre pode visualização o código finalizado no [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) ou fazer check-out do código localmente ao alternar para a ramificação `React/navigation-routing-solution`.
