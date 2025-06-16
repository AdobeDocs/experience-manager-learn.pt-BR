---
title: Adicionar componentes editáveis às rotas dinâmicas do SPA remoto
description: Saiba como adicionar componentes editáveis a rotas dinâmicas em um SPA remoto.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7636
thumbnail: kt-7636.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
duration: 202
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# Rotas dinâmicas e componentes editáveis

{{spa-editor-deprecation}}

Neste capítulo, ativamos duas rotas dinâmicas de Detalhe de Aventura para suportar componentes editáveis: __Campo de Surfe de Bali__ e __Beervana em Portland__.

![Rotas dinâmicas e componentes editáveis](./assets/spa-dynamic-routes/intro.png)

A rota SPA Adventure Detail é definida como `/adventure/:slug`, onde `slug` é uma propriedade de identificador exclusivo no Fragmento de Conteúdo Adventure.

## Mapear os URLs de SPA para páginas do AEM

Nos dois capítulos anteriores, mapeamos o conteúdo do componente editável da exibição Início do SPA para a página raiz do SPA remoto correspondente no AEM em `/content/wknd-app/us/en/`.

A definição de mapeamento para componentes editáveis para as rotas dinâmicas do SPA é semelhante, no entanto, devemos criar um esquema de mapeamento 1:1 entre as instâncias da rota e as páginas do AEM.

Neste tutorial, pegamos o nome do Fragmento de conteúdo WKND Adventure, que é o último segmento do caminho, e o mapeamos para um caminho simples em `/content/wknd-app/us/en/adventure`.

| Rota SPA remota | Caminho da página do AEM |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /aventura/__bali-surf-camp__ | /content/wknd-app/us/en/home/Adventure/__bali-surf-camp__ |
| /aventura/__beervana-portland__ | /content/wknd-app/us/en/home/Adventure/__beervana-in-portland__ |

Portanto, com base nesse mapeamento, devemos criar duas novas páginas do AEM em:

* `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
* `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Mapeamento de SPA remoto

O mapeamento de solicitações que deixam o SPA Remoto é configurado por meio da configuração `setupProxy` feita no [Bootstrap do SPA](./spa-bootstrap.md).

## Mapeamento do Editor SPA

O mapeamento de solicitações de SPA quando o SPA é aberto por meio do AEM SPA Editor é configurado por meio da configuração Mapeamentos do Sling realizada em [Configurar AEM](./aem-configure.md).

## Criar páginas de conteúdo no AEM

Primeiro, crie o segmento de página `adventure` intermediário:

1. Faça logon no AEM Author
1. Navegue até __Sites > Aplicativo WKND > us > en > Página inicial do aplicativo WKND__
   1. Essa página do AEM é mapeada como a raiz do SPA, portanto, é aqui que começamos a criar a estrutura da página do AEM para outras rotas de SPA.
1. Toque em __Criar__ e selecione __Página__
1. Selecione o modelo da __Página de SPA Remota__ e toque em __Avançar__
1. Preencher as propriedades da página
   1. __Título__: Aventura
   1. __Nome__: `adventure`
      1. Esse valor define o URL da página do AEM e, portanto, deve corresponder ao segmento de rota do SPA.
1. Toque em __Concluído__

Em seguida, crie as páginas do AEM que correspondem a cada um dos URLs do SPA que exigem áreas editáveis.

1. Navegue até a nova página __Aventura__ no Administrador do Site
1. Toque em __Criar__ e selecione __Página__
1. Selecione o modelo da __Página de SPA Remota__ e toque em __Avançar__
1. Preencher as propriedades da página
   1. __Título__: Campo de Surf em Bali
   1. __Nome__: `bali-surf-camp`
      1. Esse valor define o URL da página do AEM e, portanto, deve corresponder ao último segmento da rota do SPA
1. Toque em __Concluído__
1. Repita as etapas 3 a 6 para criar a página __Beervana in Portland__, com:
   1. __Título__: Beervana em Portland
   1. __Nome__: `beervana-in-portland`
      1. Esse valor define o URL da página do AEM e, portanto, deve corresponder ao último segmento da rota do SPA

Essas duas páginas do AEM contêm o respectivo conteúdo criado para suas rotas de SPA correspondentes. Se outras rotas de SPA exigirem criação, as novas Páginas do AEM deverão ser criadas na URL do SPA na página raiz da página do SPA Remoto (`/content/wknd-app/us/en/home`) no AEM.

## Atualizar o aplicativo WKND

Vamos colocar o componente `<ResponsiveGrid...>` criado no [último capítulo](./spa-container-component.md), em nosso componente SPA `AdventureDetail`, criando um contêiner editável.

### Coloque o componente SPA ResponsiveGrid

Colocar o `<ResponsiveGrid...>` no componente `AdventureDetail` cria um contêiner editável nessa rota. O truque é porque várias rotas usam o componente `AdventureDetail` para renderizar, devemos ajustar dinamicamente o atributo `<ResponsiveGrid...>'s pagePath`. O `pagePath` deve ser derivado para apontar para a página do AEM correspondente, com base na aventura que a instância da rota exibe.

1. Abrir e editar `react-app-/src/components/AdventureDetail.js`
1. Importe o componente `ResponsiveGrid` e coloque-o acima do componente `<h2>Itinerary</h2>`.
1. Defina os seguintes atributos no componente `<ResponsiveGrid...>`. Observe que o atributo `pagePath` adiciona a `slug` atual, que mapeia para a página de aventura de acordo com o mapeamento definido acima.
   1. `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   1. `itemPath = 'root/responsivegrid'`

   Isso instrui o componente `ResponsiveGrid` a recuperar seu conteúdo do recurso AEM:

   1. `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`

Atualizar `AdventureDetail.js` com as seguintes linhas:

```javascript
...
import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
...

function AdventureDetailRender(props) {
    ...
    // Get the slug from the React route parameter, this will be used to specify the AEM Page to store/read editable content from
    const { slug } = useParams();

    return(
        ...
        // Pass the slug in
        function AdventureDetailRender({ title, primaryImage, activity, adventureType, tripLength, 
                groupSize, difficulty, price, description, itinerary, references, slug }) {
            ...
            return (
                ...
                <ResponsiveGrid 
                    pagePath={`/content/wknd-app/us/en/home/adventure/${slug}`}
                    itemPath="root/responsivegrid"/>
                    
                <h2>Itinerary</h2>
                ...
            )
        }
    )
}
```

O arquivo `AdventureDetail.js` deve ser semelhante a:

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## Criar o contêiner no AEM

Com o `<ResponsiveGrid...>` em vigor e seu `pagePath` definido dinamicamente com base na aventura que está sendo renderizada, tentamos criar conteúdo nele.

1. Faça logon no AEM Author
1. Navegue até __Sites > Aplicativo WKND > us > en__
1. __Editar__ a __Página inicial do Aplicativo WKND__
   1. Navegue até a rota __Campo de Surf de Bali__ no SPA para editá-la
1. Selecione __Visualizar__ no seletor de modo no canto superior direito
1. Toque no cartão __Campo de Surf de Bali__ no SPA para navegar até sua rota
1. Selecione __Editar__ no seletor de modo
1. Localize a área editável __Contêiner de layout__ logo acima do __Itinerário__
1. Abra a __barra lateral do Editor de páginas__ e selecione a __exibição de Componentes__
1. Arraste alguns dos componentes habilitados para o __Contêiner de layout__
   1. Imagem
   1. Texto
   1. Título

   E criar algum material de marketing promocional. Pode ser semelhante a:

   ![Criação de Detalhes de Aventura de Bali](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __Visualizar__ suas alterações no Editor de páginas do AEM
1. Atualize o aplicativo WKND em execução localmente em [http://localhost:3000](http://localhost:3000), navegue até a rota __Campo de Surf de Bali__ para ver as alterações criadas!

   ![Bali de SPA Remoto](./assets/spa-dynamic-routes/remote-spa-final.png)

Ao navegar para uma rota de detalhes de aventura que não tem uma Página do AEM mapeada, não há capacidade de criação nessa instância de rota. Para habilitar a criação nessas páginas, basta criar uma Página do AEM com o nome correspondente na página __Aventura__!

## Parabéns!

Parabéns! Você adicionou a capacidade de criação a rotas dinâmicas no SPA!

* Adição do componente ResponsiveGrid do componente editável do AEM React a uma rota dinâmica
* Páginas do AEM criadas para dar suporte à criação de duas rotas específicas no SPA (Campo de surf de Bali e Beervana em Portland)
* Conteúdo criado na rota dinâmica do Campo de Surf de Bali!

Agora você concluiu a exploração das primeiras etapas de como o Editor SPA do AEM pode ser usado para adicionar áreas editáveis específicas a um SPA remoto!
