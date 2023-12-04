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
duration: 286
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# Rotas dinâmicas e componentes editáveis

Neste capítulo, ativamos duas rotas de Detalhe de aventura dinâmicas para suportar componentes editáveis; __Campo de Surf de Bali__ e __Beervana in Portland__.

![Rotas dinâmicas e componentes editáveis](./assets/spa-dynamic-routes/intro.png)

A rota do SPA Adventure Detail é definida como `/adventure/:slug` onde `slug` é uma propriedade de identificador exclusivo no Fragmento de conteúdo de aventura.

## Mapear os URLs do SPA para páginas do AEM

Nos dois capítulos anteriores, mapeamos o conteúdo de componentes editáveis da visualização inicial do SPA para a página raiz do SPA remoto correspondente no AEM em `/content/wknd-app/us/en/`.

A definição de mapeamento para componentes editáveis para as rotas dinâmicas do SPA é semelhante, no entanto, devemos criar um esquema de mapeamento 1:1 entre as instâncias das páginas de rota e AEM.

Neste tutorial, pegamos o nome do Fragmento de conteúdo WKND Adventure, que é o último segmento do caminho, e o mapeamos para um caminho simples em `/content/wknd-app/us/en/adventure`.

| Rota remota do SPA | Caminho da página AEM |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /aventura/__bali-surf-camp__ | /content/wknd-app/us/en/home/Adventure/__bali-surf-camp__ |
| /aventura/__beervana-portland__ | /content/wknd-app/us/en/home/Adventure/__beervana-in-portland__ |

Então, com base nesse mapeamento, devemos criar duas novas páginas de AEM em:

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Mapeamento remoto do SPA

O mapeamento de solicitações que deixam o SPA remoto é configurado por meio da variável `setupProxy` configuração feita em [Bootstrap do SPA](./spa-bootstrap.md).

## Mapeamento do editor de SPA

O mapeamento das solicitações do SPA quando o SPA é aberto por meio do editor AEM SPA é configurado por meio da configuração Mapeamentos do Sling feita em [Configurar AEM](./aem-configure.md).

## Criar páginas de conteúdo no AEM

Primeiro, crie o intermediário `adventure` Segmento de página:

1. Faça logon no AEM Author
1. Navegue até __Sites > Aplicativo WKND > br > pt > Página inicial do aplicativo WKND__
   + Essa página do AEM é mapeada como a raiz do SPA, então é aqui que começamos a construir a estrutura da página do AEM para outras rotas do SPA.
1. Toque __Criar__ e selecione __Página__
1. Selecione o __Página remota do SPA__ modelo e toque em __Próxima__
1. Preencher as propriedades da página
   + __Título__: Aventura
   + __Nome__: `adventure`
      + Esse valor define o URL da página AEM e, portanto, deve corresponder ao segmento de rota do SPA.
1. Toque __Concluído__

Em seguida, crie as páginas do AEM que correspondem a cada um dos URLs do SPA que exigem áreas editáveis.

1. Navegue até o novo __Aventura__ página no Administrador do site
1. Toque __Criar__ e selecione __Página__
1. Selecione o __Página remota do SPA__ modelo e toque em __Próxima__
1. Preencher as propriedades da página
   + __Título__: Campo de Surf em Bali
   + __Nome__: `bali-surf-camp`
      + Esse valor define o URL da página AEM e, portanto, deve corresponder ao último segmento da rota do SPA
1. Toque __Concluído__
1. Repita as etapas 3 a 6 para criar a variável __Beervana in Portland__ página, com:
   + __Título__: Beervana em Portland
   + __Nome__: `beervana-in-portland`
      + Esse valor define o URL da página AEM e, portanto, deve corresponder ao último segmento da rota do SPA

Essas duas páginas AEM contêm o respectivo conteúdo criado para suas rotas SPA correspondentes. Se outras rotas de SPA exigirem criação, as novas Páginas AEM deverão ser criadas no URL do SPA na página raiz da página remota do SPA (`/content/wknd-app/us/en/home`) no AEM.

## Atualizar o aplicativo WKND

Vamos colocar o `<ResponsiveGrid...>` componente criado na [último capítulo](./spa-container-component.md), em nosso `AdventureDetail` Componente do SPA, criação de um contêiner editável.

### Inserir o componente ResponsiveGrid SPA

Colocar o `<ResponsiveGrid...>` no `AdventureDetail` o componente cria um contêiner editável nessa rota. O truque é porque várias rotas usam o `AdventureDetail` para renderizar, devemos ajustar dinamicamente a variável  `<ResponsiveGrid...>'s pagePath` atributo. A variável `pagePath` deve ser derivado para apontar para a página AEM correspondente, com base na aventura que a instância da rota exibe.

1. Abrir e editar `react-app-/src/components/AdventureDetail.js`
1. Importe o `ResponsiveGrid` e coloque-o acima do `<h2>Itinerary</h2>` componente.
1. Defina os seguintes atributos no `<ResponsiveGrid...>` componente. Observe que `pagePath` o atributo adiciona o atual `slug` que mapeia para a página de aventura de acordo com o mapeamento definido acima.
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   + `itemPath = 'root/responsivegrid'`

   Isso instrui o `ResponsiveGrid` para recuperar seu conteúdo do recurso AEM:

   + `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`

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

A variável `AdventureDetail.js` O arquivo deve ter a seguinte aparência:

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## Criar o contêiner no AEM

Com o `<ResponsiveGrid...>` em vigor, e `pagePath` dinamicamente definido com base na aventura que está sendo renderizada, tentamos criar conteúdo nele.

1. Faça logon no AEM Author
1. Navegue até __Sites > Aplicativo WKND > br > pt-BR__
1. __Editar__ o __Página inicial do aplicativo WKND__ página
   + Navegue até a __Campo de Surf de Bali__ roteamento no SPA para editá-lo
1. Selecionar __Visualizar__ no seletor de modo no canto superior direito
1. Toque no __Campo de Surf de Bali__ no SPA para navegar até a sua rota
1. Selecionar __Editar__ no seletor de modo
1. Localize o __Contêiner de layout__ área editável logo acima da __Itinerário__
1. Abra o __Barra lateral do Editor de páginas__ e selecione a variável __Visualização Componentes__
1. Arraste alguns dos componentes ativados para a __Contêiner de layout__
   + Imagem
   + Texto
   + Título

   E criar algum material de marketing promocional. Pode ser semelhante a:

   ![Criação de detalhes de aventura de Bali](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __Visualizar__ suas alterações no Editor de página AEM
1. Atualizar o aplicativo WKND em execução localmente em [http://localhost:3000](http://localhost:3000), navegue até o __Campo de Surf de Bali__ rota para ver as alterações criadas!

   ![Bali SPA remoto](./assets/spa-dynamic-routes/remote-spa-final.png)

Ao navegar para uma rota de detalhes de aventura que não tem uma página AEM mapeada, não há capacidade de criação nessa instância de rota. Para ativar a criação nessas páginas, basta criar uma página AEM com o nome correspondente sob o __Aventura__ página!

## Parabéns.

Parabéns! Você adicionou a capacidade de criação a rotas dinâmicas no SPA!

+ Adição do componente ResponsiveGrid do componente editável AEM React a uma rota dinâmica
+ Páginas do AEM criadas para apoiar a criação de duas rotas específicas no SPA (Campo de Surfe de Bali e Beervana em Portland)
+ Conteúdo criado na rota dinâmica do Campo de Surf de Bali!

Você concluiu a exploração dos primeiros passos de como o Editor SPA AEM pode ser usado para adicionar áreas editáveis específicas a um SPA remoto!
