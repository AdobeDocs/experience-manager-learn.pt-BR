---
title: Visualização do fragmento de conteúdo
description: Saiba como usar a visualização do Fragmento de conteúdo para todos os autores para ver rapidamente como as alterações de conteúdo afetam suas experiências AEM headless.
version: Cloud Service
feature: Content Fragments
topic: Headless, Content Management, Development
role: Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-03-17T00:00:00Z
jira: KT-10841
thumbnail: 3416906.jpeg
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---


# Visualização do fragmento de conteúdo

AEM aplicativos headless oferecem suporte à visualização de criação integrada. A experiência de visualização vincula o editor de Fragmento de conteúdo do autor do AEM ao seu aplicativo personalizado (endereçável via HTTP), permitindo um deep link no aplicativo que renderiza o Fragmento de conteúdo que está sendo visualizado.

>[!VIDEO](https://video.tv.adobe.com/v/3416906?quality=12&learn=on)

Para usar a visualização do Fragmento de conteúdo, várias condições devem ser atendidas:

1. O aplicativo deve ser implantado em um URL acessível para autores
1. O aplicativo deve ser configurado para se conectar ao serviço de Autor do AEM (em vez do serviço de Publicação do AEM)
1. O aplicativo deve ser projetado com URLs ou rotas que possam ser usadas [Caminho ou ID do fragmento do conteúdo](#url-expressions) para selecionar os Fragmentos do conteúdo a serem exibidos para visualização na experiência do aplicativo.

## Visualizar URLs

Visualizar URLs, usando [Expressões de URL](#url-expressions), são definidas nas Propriedades do modelo de fragmento de conteúdo.

![URL de visualização do modelo de fragmento de conteúdo](./assets/preview/cf-model-preview-url.png)

1. Faça logon no AEM Author Service como Administrador
1. Navegar para __Ferramentas > Geral > Modelos de fragmento de conteúdo__
1. Selecione o __Modelo de fragmento de conteúdo__ e selecione __Propriedades__ forme a barra de ação superior.
1. Insira o URL de visualização do Modelo de fragmento de conteúdo usando [Expressões de URL](#url-expressions)
   + O URL de visualização deve apontar para uma implantação do aplicativo que se conecta ao serviço de autor do AEM.

### Expressões de URL

Cada Modelo de fragmento de conteúdo pode ter um URL de visualização definido. O URL de visualização pode ser parametrizado por Fragmento do conteúdo usando as expressões de URL listadas na tabela abaixo. Várias expressões de URL podem ser usadas em um único URL de visualização.

|  | Expressão de URL | Valor |
| --------------------------------------- | ----------------------------------- | ----------- |
| Caminho do fragmento do conteúdo | `${contentFragment.path}` | `/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` |
| ID do fragmento do conteúdo | `${contentFragment.id}` | `12c34567-8901-2aa3-45b6-d7890aa1c23c` |
| Variação do fragmento de conteúdo | `${contentFragment.variation}` | `main` |
| Caminho do modelo do fragmento do conteúdo | `${contentFragment.model.path}` | `/conf/wknd-shared/settings/dam/cfm/models/adventure` |
| Nome do modelo do fragmento de conteúdo | `${contentFragment.model.name}` | `adventure` |

Exemplo de URLs de visualização:

+ Um URL de visualização no __Aventura__ o modelo pode parecer `https://preview.app.wknd.site/adventure${contentFragment.path}` que resolve `https://preview.app.wknd.site/adventure/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali`
+ Um URL de visualização no __Artigo__ o modelo pode parecer `https://preview.news.wknd.site/${contentFragment.model.name}/${contentFragment.id}.html?variation=${contentFragment.variation}` o resolve `https://preview.news.wknd.site/article/99c34317-1901-2ab3-35b6-d7890aa1c23c.html?variation=main`

## Visualização no aplicativo

Qualquer Fragmento de conteúdo usando o Modelo de fragmento de conteúdo configurado tem um botão Visualizar . O botão Visualizar abre o URL de visualização do Modelo de fragmento de conteúdo e injeta os valores do Fragmento de conteúdo aberto no [Expressões de URL](#url-expressions).

![Botão Visualizar](./assets/preview/preview-button.png)

Execute uma atualização rígida (limpando o cache local do navegador) ao visualizar as alterações do Fragmento de conteúdo no aplicativo.

## Exemplo de reação

Vamos explorar o aplicativo WKND, um aplicativo React simples que exibe aventuras de AEM usando APIs AEM Headless GraphQL.

O código de exemplo está disponível em [Github.com](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/preview-tutorial).

## URLs e rotas

Os URLs ou as rotas usadas para visualizar um Fragmento de conteúdo devem ser compostas usando [Expressões de URL](#url-expressions). Nesta versão habilitada para visualização do aplicativo WKND, os Fragmentos de conteúdo de aventura são exibidos por meio do `AdventureDetail` componente vinculado à rota `/adventure<CONTENT FRAGMENT PATH>`. Assim, o URL de visualização do modelo de Aventura WKND deve ser definido como `https://preview.app.wknd.site:3000/adventure${contentFragment.path}` para resolver esta rota.

A visualização do Fragmento de conteúdo funciona somente se o aplicativo tiver uma rota endereçável, que pode ser preenchida com [Expressões de URL](#url-expressions) que renderiza esse Fragmento de conteúdo no aplicativo de maneira visualizável.

+ `src/App.js`

```javascript
...
function App() {
  return (
    <Router>
      <div className="App">
        <header>
            <Link to={"/"}>
                <img src={logo} className="logo" alt="WKND Logo"/>
            </Link>        
            <hr />
        </header>
        <Routes>
          {/* The route's path must match the Adventure Model's Preview URL expression. In React since the path has `/` you must use wildcards to match instead of the usual `:path` */}
          <Route path='/adventure/*' element={<AdventureDetail />}/>
          <Route path="/" element={<Home />}/>
        </Routes>
      </div>
    </Router>
  );
}

export default App;
```

### Exibir o conteúdo criado

O `AdventureDetail` O componente simplesmente analisa o caminho do Fragmento do conteúdo, inserido no URL de visualização por meio do `${contentFragment.path}` [Expressão de URL](#url-expressions), do URL da rota e o usa para coletar e renderizar a Aventura WKND.

+ `src/components/AdventureDetail.js`

```javascript
...
function AdventureDetail() {

    // Read the `path` value which is the parameter used to query for the adventure's details
    // since the params value captures the `*` wildcard in `/adventure/*`, or everything after the first `/` in the Content Fragment path.
    const params = useParams();
    const pathParam = params["*"];

    // Add the leading '/' back on 
    const path = '/' + pathParam;
    
    // Query AEM for the Adventures's details, using the Content Fragment's `path`
    const { adventure, references, error } = useAdventureByPath(path);

    // Handle error and loading conditions
    if (error) {
        return <Error errorMessage={error} />;
    } else if (!adventure) {
        return <Loading />;
    }

    return (<div className="adventure-detail">
        ...
        <AdventureDetailRender {...adventure} references={references} />
    </div>);
}
...
```
