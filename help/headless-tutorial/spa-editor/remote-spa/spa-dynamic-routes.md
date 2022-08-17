---
title: Adicionar componentes editáveis às rotas dinâmicas SPA remotas
description: Saiba como adicionar componentes editáveis a rotas dinâmicas em um SPA remoto.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '947'
ht-degree: 1%

---

# Rotas dinâmicas e componentes editáveis

Neste capítulo, habilitamos duas rotas de Detalhe do Aventura dinâmicas para suportar componentes editáveis; __Campo de Surf de Bali__ e __Beervana em Portland__.

![Rotas dinâmicas e componentes editáveis](./assets/spa-dynamic-routes/intro.png)

O SPA de Detalhes da Aventura é definido como `/adventure:path` em que `path` é o caminho para a Aventura WKND (Fragmento de conteúdo) para exibir detalhes.

## Mapear os URLs SPA para AEM páginas

Nos dois capítulos anteriores, mapeamos o conteúdo editável do componente da exibição Início do SPA para a página raiz Remota SPA correspondente em AEM em `/content/wknd-app/us/en/`.

A definição de mapeamento para componentes editáveis para as rotas dinâmicas de SPA é semelhante, no entanto, devemos criar um esquema de mapeamento 1:1 entre instâncias da rota e páginas de AEM.

Neste tutorial, pegamos o nome do Fragmento de conteúdo de aventura WKND, que é o último segmento do caminho, e o mapeamos para um caminho simples em `/content/wknd-app/us/en/adventure`.

| Rota de SPA remota | Caminho da página AEM |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure:/content/dam/wknd/en/adventures/bali-surf-camp/__campo dos bali-surf__ | /content/wknd-app/us/en/home/adventure/__campo dos bali-surf__ |
| /adventure:/content/dam/wknd/en/adventures/beervana-portland/__beervana-portland__ | /content/wknd-app/us/en/home/adventure/__beervana-in-portland__ |

Portanto, com base nesse mapeamento, devemos criar duas novas páginas de AEM em:

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Mapeamento de SPA remoto

O mapeamento de solicitações que saem do SPA Remoto é configurado por meio da variável `setupProxy` configuração feita em [Bootstrap do SPA](./spa-bootstrap.md).

## Mapeamento do editor de SPA

O mapeamento para solicitações de SPA quando o SPA é aberto por AEM Editor SPA são configurados por meio da configuração de Mapeamentos do Sling feita em [Configurar AEM](./aem-configure.md).

## Criar páginas de conteúdo em AEM

Primeiro, crie o intermediário `adventure` Segmento de página:

1. Faça logon no AEM Author
1. Navegar para __Sites > Aplicativo WKND > eua > pt > Página inicial do aplicativo WKND__
   + Essa página de AEM é mapeada como a raiz do SPA, então é aqui que começamos a criar a estrutura de página de AEM para outras rotas de SPA.
1. Toque __Criar__ e selecione __Página__
1. Selecione o __Página SPA Remota__ modelo e toque em __Próximo__
1. Preencha as propriedades da página
   + __Título__: Aventura
   + __Nome__: `adventure`
      + Esse valor define o URL da página AEM e, portanto, deve corresponder ao segmento de rota do SPA.
1. Toque __Concluído__

Em seguida, crie as páginas de AEM que correspondem a cada URLs de SPA que exigem áreas editáveis.

1. Navegar para o novo __Aventura__ no Administrador do site
1. Toque __Criar__ e selecione __Página__
1. Selecione o __Página SPA Remota__ modelo e toque em __Próximo__
1. Preencha as propriedades da página
   + __Título__: Campo de Surf de Bali
   + __Nome__: `bali-surf-camp`
      + Esse valor define o URL da página AEM e, portanto, deve corresponder ao último segmento da rota SPA
1. Toque __Concluído__
1. Repita as etapas 3 a 6 para criar a variável __Beervana em Portland__ página, com:
   + __Título__: Beervana em Portland
   + __Nome__: `beervana-in-portland`
      + Esse valor define o URL da página AEM e, portanto, deve corresponder ao último segmento da rota SPA

Essas duas páginas de AEM têm o conteúdo criado respectivo para suas rotas de SPA correspondentes. Se outras rotas de SPA exigirem criação, novas Páginas de AEM devem ser criadas em seu URL de SPA na página raiz da página SPA Remota (`/content/wknd-app/us/en/home`) em AEM.

## Atualizar o aplicativo WKND

Vamos colocar o `<AEMResponsiveGrid...>` componente criado na [último capítulo](./spa-container-component.md)em nossa `AdventureDetail` SPA componente, criando um contêiner editável.

### Coloque o componente AEMResponsiveGrid SPA

Colocando o `<AEMResponsiveGrid...>` no `AdventureDetail` cria um contêiner editável nessa rota. O truque é porque várias rotas usam a variável `AdventureDetail` para renderizar, precisamos ajustar dinamicamente o  `<AEMResponsiveGrid...>'s pagePath` atributo. O `pagePath` deve ser derivado para apontar para a página de AEM correspondente, com base na aventura que a instância da rota é exibida.

1. Abrir e editar `react-app/src/components/AdventureDetail.js`
1. Adicione a seguinte linha antes de `AdventureDetail(..)'s` Second `return(..)` , que deriva o nome da aventura do caminho do fragmento de conteúdo.

   ```
   ...
   // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
   const adventureName = _path.split('/').pop();
   ...
   ```

1. Importe o `AEMResponsiveGrid` e coloque-o acima do `<h2>Itinerary</h2>` componente.
1. Defina os seguintes atributos no `<AEMResponsiveGrid...>` componente
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${adventureName}'`
   + `itemPath = 'root/responsivegrid'`

   Isso instrui o `AEMResponsiveGrid` componente para recuperar o conteúdo do recurso AEM:

   + `/content/wknd-app/us/en/home/adventure/${adventureName}/jcr:content/root/responsivegrid`


Atualizar `AdventureDetail.js` com as seguintes linhas:

```
...
import AEMResponsiveGrid from '../components/aem/AEMResponsiveGrid';
...

function AdventureDetail(props) {
    ...
    // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
    const adventureName = _path.split('/').pop();

    return(
        ...
        <AEMResponsiveGrid 
            pagePath={`/content/wknd-app/us/en/home/adventure/${adventureName}`}
            itemPath="root/responsivegrid"/>
            
        <h2>Itinerary</h2>
        ...
    )
}
```

O `AdventureDetail.js` O arquivo deve ter a seguinte aparência:

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## Crie o contêiner no AEM

Com o `<AEMResponsiveGrid...>` em vigor e `pagePath` configurado dinamicamente com base na aventura que está sendo renderizada, tentamos criar conteúdo nela.

1. Faça logon no AEM Author
1. Navegar para __Sites > Aplicativo WKND > eua > pt__
1. __Editar__ o __Página inicial do aplicativo WKND__ página
   + Navegue até o __Campo de Surf de Bali__ na SPA para editá-la
1. Selecionar __Visualizar__ no seletor de modo no canto superior direito
1. Toque em __Campo de Surf de Bali__ no SPA para navegar até sua rota
1. Selecionar __Editar__ no seletor de modo
1. Localize a variável __Contêiner de layout__ área editável logo acima da __Itinerário__
1. Abra o __Barra lateral do Editor de páginas__ e selecione o __Exibição de componentes__
1. Arraste alguns dos componentes ativados para o __Contêiner de layout__
   + Imagem
   + Texto
   + Título

   E criar algum material promocional de marketing. Pode ser algo como isto:

   ![Criação de Detalhes da Aventura Bali](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __Visualizar__ suas alterações no Editor de páginas AEM
1. Atualize o aplicativo WKND executado localmente em [http://localhost:3000](http://localhost:3000), navegue até o __Campo de Surf de Bali__ para ver as alterações criadas!

   ![Bali de SPA Remoto](./assets/spa-dynamic-routes/remote-spa-final.png)

Ao navegar até uma rota de detalhes de aventura que não tem uma página de AEM mapeada, não há capacidade de criação nessa instância de rota. Para ativar a criação nessas páginas, basta criar uma Página de AEM com o nome correspondente na variável __Aventura__ página!

## Parabéns. 

Parabéns. Você adicionou capacidade de criação a rotas dinâmicas no SPA!

+ Adição do componente ResponsiveGrid do Componente Editável de Reação de AEM a uma rota dinâmica
+ Criadas AEM páginas para apoiar a criação de duas rotas específicas no SPA (Campo de Surfe de Bali e Beervana em Portland)
+ Conteúdo criado na rota dinâmica do Campo de Surfe de Bali!

Agora você concluiu a exploração das primeiras etapas de como AEM Editor SPA pode ser usado para adicionar áreas editáveis específicas a um SPA Remoto!


>[!NOTE]
>
>Fique ligado! Este tutorial será expandido para abranger as práticas recomendadas e recomendações do Adobe sobre como implantar a solução do Editor de SPA para AEM ambientes de produção e as a Cloud Service.
