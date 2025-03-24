---
title: Adicionar navegação e roteamento | Introdução ao AEM SPA Editor e React
description: Saiba como várias exibições no SPA podem ser compatíveis, mapeando para Páginas do AEM com o SPA Editor SDK. A navegação dinâmica é implementada usando o Roteador React e os Componentes principais do React.
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 9c3d47c7-1bb9-441c-a0e6-85887a32c817
duration: 337
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1481'
ht-degree: 0%

---

# Adicionar navegação e roteamento {#navigation-routing}

Saiba como várias exibições no SPA podem ser compatíveis, mapeando para Páginas do AEM com o SPA Editor SDK. A navegação dinâmica é implementada usando o Roteador React e os Componentes principais do React.

## Objetivo

1. Entenda as opções de roteamento de modelo SPA disponíveis ao usar o Editor SPA.
1. Saiba como usar o [Roteador de reação](https://reacttraining.com/react-router) para navegar entre diferentes exibições do SPA.
1. Use os Componentes principais do AEM React para implementar uma navegação dinâmica orientada pela hierarquia de páginas do AEM.

## O que você vai criar

Este capítulo adicionará a navegação a um SPA no AEM. O menu de navegação é orientado pela hierarquia de páginas do AEM e usará o modelo JSON fornecido pelo [Componente principal de navegação](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/navigation.html).

![Navegação adicionada](assets/navigation-routing/navigation-added.png)

## Pré-requisitos

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment). Este capítulo é uma continuação do capítulo [Componentes de Mapa](map-components.md). No entanto, basta seguir um projeto do AEM habilitado para SPA implantado em uma instância do AEM local para que você possa seguir.

## Adicionar a navegação ao modelo {#add-navigation-template}

1. Abra um navegador e faça logon no AEM, [http://localhost:4502/](http://localhost:4502/). A base de código inicial já deve estar implantada.
1. Navegue até o **Modelo de página do SPA**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Selecione o **Contêiner de Layout Raiz** mais externo e clique em seu ícone **Política**. Tenha cuidado **não** para selecionar o **Contêiner de layout** desbloqueado para criação.

   ![Selecione o ícone da política de contêiner de layout raiz](assets/navigation-routing/root-layout-container-policy.png)

1. Crie uma nova política chamada **Estrutura SPA**:

   ![Política de Estrutura do SPA](assets/navigation-routing/spa-policy-update.png)

   Em **Componentes Permitidos** > **Geral** > selecione o componente **Contêiner de Layout**.

   Em **Componentes Permitidos** > **WKND SPA REACT - STRUCTURE** > selecione o componente **Navigation**:

   ![Selecionar componente de Navegação](assets/navigation-routing/select-navigation-component.png)

   Em **Componentes Permitidos** > **WKND SPA REACT - Conteúdo** > selecione os componentes **Imagem** e **Texto**. Você deve ter um total de quatro componentes selecionados.

   Clique em **Concluído** para salvar as alterações.

1. Atualize a página e adicione o componente **Navegação** acima do **Contêiner de Layout** desbloqueado:

   ![adicionar componente de Navegação ao modelo](assets/navigation-routing/add-navigation-component.png)

1. Selecione o componente de **Navegação** e clique em seu ícone de **Política** para editar a política.
1. Crie uma nova política com um **Título da Política** de **Navegação de SPA**.

   Em **Propriedades**:

   * Defina a **Raiz de Navegação** como `/content/wknd-spa-react/us/en`.
   * Defina os **Excluir Níveis de Raiz** para **1**.
   * Desmarcar **Coletar todas as páginas secundárias**.
   * Defina a **Profundidade da Estrutura de Navegação** como **3**.

   ![Configurar Política de Navegação](assets/navigation-routing/navigation-policy.png)

   Isso coletará os 2 níveis de navegação abaixo de `/content/wknd-spa-react/us/en`.

1. Depois de salvar as alterações, você deverá ver o `Navigation` preenchido como parte do modelo:

   ![Componente de navegação preenchido](assets/navigation-routing/populated-navigation.png)

## Criar páginas secundárias

Em seguida, crie páginas adicionais no AEM que servirão como as diferentes exibições no SPA. Também inspecionaremos a estrutura hierárquica do modelo JSON fornecido pelo AEM.

1. Navegue até o console **Sites**: [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Selecione a **Página Inicial do WKND SPA React** e clique em **Criar** > **Página**:

   ![Criar nova página](assets/navigation-routing/create-new-page.png)

1. Em **Modelo**, selecione **Página de SPA**. Em **Propriedades**, digite **Página 1** para o **Título** e a **página-1** como o nome.

   ![Insira as propriedades da página inicial](assets/navigation-routing/initial-page-properties.png)

   Clique em **Criar** e, no pop-up da caixa de diálogo, clique em **Abrir** para abrir a página no Editor SPA do AEM.

1. Adicionar um novo componente **Texto** ao **Contêiner de Layout** principal. Edite o componente e insira o texto: **Página 1** usando o RTE e o elemento **H2**.

   ![Exemplo de página de conteúdo 1](assets/navigation-routing/page-1-sample-content.png)

   Fique à vontade para adicionar mais conteúdo, como uma imagem.

1. Retorne ao console do AEM Sites e repita as etapas acima, criando uma segunda página denominada **Página 2** como irmã da **Página 1**.
1. Por fim, crie uma terceira página, **Página 3**, mas como **criança** da **Página 2**. Depois de concluída, a hierarquia do site deve ser semelhante ao seguinte:

   ![Hierarquia do Site de Exemplo](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. O componente de Navegação agora pode ser usado para navegar para diferentes áreas do SPA.

   ![Navegação e roteamento](assets/navigation-routing/navigation-working.gif)

1. Abra a página fora do Editor do AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html). Use o componente de **Navegação** para navegar para diferentes modos de exibição do aplicativo.

1. Use as ferramentas de desenvolvedor do seu navegador para inspecionar as solicitações de rede enquanto você navega. As capturas de tela abaixo são feitas no navegador Google Chrome.

   ![Observar solicitações de rede](assets/navigation-routing/inspect-network-requests.png)

   Observe que após o carregamento da página inicial, a navegação subsequente não causa uma atualização completa da página e que o tráfego de rede é minimizado ao retornar às páginas visitadas anteriormente.

## Modelo JSON da página de hierarquia {#hierarchy-page-json-model}

Em seguida, inspecione o Modelo JSON que direciona a experiência de várias visualizações do SPA.

1. Em uma nova guia, abra a API do modelo JSON fornecida pelo AEM: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Pode ser útil usar uma extensão de navegador para [formatar o JSON](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa).

   Esse conteúdo JSON é solicitado quando o SPA é carregado pela primeira vez. A estrutura externa tem a seguinte aparência:

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

   Em `:children` você deve ver uma entrada para cada uma das páginas criadas. O conteúdo de todas as páginas está nesta solicitação JSON inicial. Com o roteamento de navegação, as exibições subsequentes do SPA são carregadas rapidamente, pois o conteúdo já está disponível no lado do cliente.

   Não é recomendável carregar **TODOS** do conteúdo de um SPA na solicitação JSON inicial, pois isso retardaria o carregamento da página inicial. Em seguida, vamos ver como a profundidade da hierarquia de páginas é coletada.

1. Navegue até o modelo **Raiz do SPA** em: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   Clique no menu **Propriedades da página** > **Política da página**:

   ![Abrir a política de página para a Raiz do SPA](assets/navigation-routing/open-page-policy.png)

1. O modelo **Raiz de SPA** tem uma guia **Estrutura Hierárquica** extra para controlar o conteúdo JSON coletado. A **Profundidade da Estrutura** determina a profundidade na hierarquia do site para coletar páginas secundárias abaixo da **raiz**. Você também pode usar o campo **Padrões de estrutura** para filtrar páginas adicionais com base em uma expressão regular.

   Atualize a **Profundidade da Estrutura** para **2**:

   ![Atualizar profundidade da estrutura](assets/navigation-routing/update-structure-depth.png)

   Clique em **Concluído** para salvar as alterações na política.

1. Reabra o modelo JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

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

   Observe que o caminho **Página 3** foi removido: `/content/wknd-spa-react/us/en/home/page-2/page-3` do modelo JSON inicial. Isso ocorre porque a **Página 3** está em um nível 3 na hierarquia e atualizamos a política para incluir conteúdo somente em uma profundidade máxima de nível 2.

1. Abra novamente a página inicial do SPA: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) e abra as ferramentas de desenvolvedor do seu navegador.

   Atualize a página e você verá a solicitação XHR para `/content/wknd-spa-react/us/en.model.json`, que é a Raiz do SPA. Observe que apenas três páginas secundárias são incluídas com base na configuração de profundidade da hierarquia para o modelo Raiz de SPA definido anteriormente no tutorial. Isso não inclui a **Página 3**.

   ![Solicitação JSON inicial - Raiz SPA](assets/navigation-routing/initial-json-request.png)

1. Com as ferramentas do desenvolvedor abertas, use o componente `Navigation` para navegar diretamente para a **Página 3**:

   Observe que uma nova solicitação XHR é feita para: `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Solicitação XHR da página três](assets/navigation-routing/page-3-xhr-request.png)

   O Gerenciador de Modelos do AEM entende que o conteúdo JSON da **Página 3** não está disponível e aciona automaticamente a solicitação XHR adicional.

1. Experimente os deep links navegando diretamente para: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). Observe também que o botão Voltar do navegador continua funcionando.

## Inspecionar Roteamento React  {#react-routing}

A navegação e o roteamento são implementados com o [Roteador React](https://reactrouter.com/en/main). O Roteador React é uma coleção de componentes de navegação para aplicativos React. Os [Componentes Principais do AEM React](https://github.com/adobe/aem-react-core-wcm-components-base) usam recursos do Roteador React para implementar o componente de **Navegação** usado nas etapas anteriores.

Em seguida, verifique como o Roteador React está integrado ao SPA e experimente usando o componente [Link](https://reactrouter.com/en/main/components/link) do Roteador React.

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

   Observe que `App` está encapsulado no componente `Router` do [Roteador React](https://reacttraining.com/react-router). O `ModelManager`, fornecido pelo AEM SPA Editor JS SDK, adiciona as rotas dinâmicas às Páginas do AEM com base na API do modelo JSON.

1. Abrir o arquivo `Page.js` em `ui.frontend/src/components/Page/Page.js`

   ```js
   class AppPage extends Page {
     get containerProps() {
       let attrs = super.containerProps;
       attrs.className =
         (attrs.className || '') + ' page ' + (this.props.cssClassNames || '');
       return attrs;
     }
   }
   
   export default MapTo('wknd-spa-react/components/page')(
     withComponentMappingContext(withRoute(AppPage))
   );
   ```

   O componente de SPA `Page` usa a função `MapTo` para mapear **Páginas** no AEM para um componente de SPA correspondente. O utilitário `withRoute` ajuda a rotear dinamicamente o SPA para a página secundária apropriada do AEM com base na propriedade `cqPath`.

1. Abra o componente `Header.js` em `ui.frontend/src/components/Header/Header.js`.
1. Atualize o `Header` para envolver a marca `<h1>` em um [Link](https://reactrouter.com/en/main/components/link) para a página inicial:

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + import {Link} from 'react-router-dom';
     require('./Header.css');
   
   export default class Header extends Component {
   
       render() {
           return (
               <header className="Header">
               <div className="Header-container">
   +              <Link to="/content/wknd-spa-react/us/en/home.html">
                       <h1>WKND</h1>
   +              </Link>
               </div>
               </header>
           );
       }
   ```

   Em vez de usar uma marca de âncora `<a>` padrão, usamos `<Link>` fornecida pelo Roteador React. Desde que `to=` aponte para uma rota válida, o SPA mudará para essa rota e **não** executará uma atualização de página inteira. Aqui, simplesmente codificamos o link para a página inicial para ilustrar o uso de `Link`.

1. Atualizar o teste em `App.test.js` em `ui.frontend/src/App.test.js`.

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   Como estamos usando recursos do Roteador React em um componente estático referenciado em `App.js`, precisamos atualizar o teste de unidade para levar em conta.

1. Abra um terminal, navegue até a raiz do projeto e implante o projeto no AEM usando suas habilidades em Maven:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Navegue até uma das páginas no SPA no AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   Em vez de usar o componente `Navigation` para navegar, use o link no `Header`.

   ![Link do cabeçalho](assets/navigation-routing/header-link.png)

   Observe que uma atualização de página inteira **não** foi acionada e que o roteamento de SPA está funcionando.

1. Opcionalmente, experimente o arquivo `Header.js` usando uma marca de âncora `<a>` padrão:

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   Isso pode ajudar a ilustrar a diferença entre o roteamento de SPA e os links de página da Web regulares.

## Parabéns. {#congratulations}

Parabéns, você aprendeu como várias exibições no SPA podem ser compatíveis, mapeando para Páginas do AEM com o SPA Editor SDK. A navegação dinâmica foi implementada com o Roteador React e adicionada ao componente `Header`.
