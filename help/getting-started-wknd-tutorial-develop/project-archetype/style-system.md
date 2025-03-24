---
title: Desenvolvimento com o sistema de estilo
description: Saiba como implementar estilos individuais e reutilizar os Componentes principais usando o Sistema de estilos do Experience Manager. Este tutorial aborda o desenvolvimento para o Sistema de estilos a fim de estender os Componentes principais com CSS específico da marca e configurações de política avançadas do Editor de modelos.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, Style System
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4128
mini-toc-levels: 1
thumbnail: 30386.jpg
doc-type: Tutorial
exl-id: 5b490132-cddc-4024-92f1-e5c549afd6f1
recommendations: noDisplay, noCatalog
duration: 358
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1555'
ht-degree: 0%

---

# Desenvolvimento com o sistema de estilo {#developing-with-the-style-system}

Saiba como implementar estilos individuais e reutilizar os Componentes principais usando o Sistema de estilos do Experience Manager. Este tutorial aborda o desenvolvimento para o Sistema de estilos a fim de estender os Componentes principais com CSS específico da marca e configurações de política avançadas do Editor de modelos.

## Pré-requisitos {#prerequisites}

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

Também é recomendável revisar o tutorial [Bibliotecas do lado do cliente e Fluxo de trabalho de front-end](client-side-libraries.md) para entender os fundamentos das bibliotecas do lado do cliente e as várias ferramentas de front-end integradas ao projeto do AEM.

### Projeto inicial

>[!NOTE]
>
> Se você concluiu com êxito o capítulo anterior, é possível reutilizar o projeto e ignorar as etapas para verificar o projeto inicial.

Verifique o código de linha base no qual o tutorial se baseia:

1. Confira a ramificação `tutorial/style-system-start` do [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
   ```

1. Implante a base de código em uma instância do AEM local usando suas habilidades do Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se estiver usando o AEM 6.5 ou 6.4, anexe o perfil `classic` a qualquer comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution) ou conferir o código localmente alternando para a ramificação `tutorial/style-system-solution`.

## Objetivo

1. Entenda como usar o Sistema de estilos para aplicar CSS específico da marca aos Componentes principais do AEM.
1. Saiba mais sobre a notação BEM e como ela pode ser usada para definir estilos com cuidado.
1. Aplique configurações avançadas de política com Modelos editáveis.

## O que você vai criar {#what-build}

Este capítulo usa o [recurso Sistema de Estilos](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=pt-BR) para criar variações dos componentes **Título** e **Texto** usados na página Artigo.

![Estilos disponíveis para o Título](assets/style-system/styles-added-title.png)

*Estilo de sublinhado disponível para uso para o Componente de Título*

## Fundo {#background}

O [Sistema de Estilos](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html) permite que desenvolvedores e editores de modelo criem várias variações visuais de um componente. Os autores podem decidir qual estilo usar ao compor uma página. O Sistema de estilos é usado no restante do tutorial para obter vários estilos únicos ao usar os Componentes principais em uma abordagem de código baixo.

A ideia geral com o Sistema de estilos é que os autores podem escolher vários estilos de como um componente deve ser exibido. Os &quot;estilos&quot; são apoiados por classes CSS adicionais que são inseridas na div externa de um componente. Nas bibliotecas de clientes, as regras CSS são adicionadas com base nessas classes de estilo para que o componente altere a aparência.

Você pode encontrar [documentação detalhada para o Sistema de Estilos aqui](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/features/style-system.html?lang=pt-BR). Também há um excelente vídeo técnico [ para entender o Sistema de Estilos](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html).

## Estilo do Sublinhado - Título {#underline-style}

O [Componente de Título](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/title.html) foi encaminhado por proxy para o projeto em `/apps/wknd/components/title` como parte do módulo **ui.apps**. Os estilos padrão de elementos de cabeçalho (`H1`, `H2`, `H3`...) já foram implementados no módulo **ui.frontend**.

Os [designs do Artigo da WKND](assets/pages-templates/wknd-article-design.xd) contêm um estilo exclusivo para o componente de Título com um sublinhado. Em vez de criar dois componentes ou modificar a caixa de diálogo de componentes, o Sistema de estilos pode ser usado para permitir que os autores adicionem uma opção de estilo sublinhado.

![Estilo do Sublinhado - Componente de Título](assets/style-system/title-underline-style.png)

### Adicionar uma política de título

Vamos adicionar uma política para os componentes de Título para permitir que os autores de conteúdo escolham o estilo Sublinhado a ser aplicado a componentes específicos. Isso é feito usando o Editor de modelos no AEM.

1. Navegue até o modelo **Página de Artigo** de: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. No modo **Estrutura**, no **Contêiner de Layout** principal, selecione o ícone **Política** ao lado do componente **Título** listado em *Componentes Permitidos*:

   ![Configuração de Política de Título](assets/style-system/article-template-title-policy-icon.png)

1. Crie uma política para o componente de Título com os seguintes valores:

   *Título da Política&#42;*: **Título da WKND**

   *Propriedades* > *Guia Estilos* > *Adicionar um novo estilo*

   **Sublinhado** : `cmp-title--underline`

   ![Configuração da política de estilo para o título](assets/style-system/title-style-policy.png)

   Clique em **Concluído** para salvar as alterações na política de Título.

   >[!NOTE]
   >
   > O valor `cmp-title--underline` preenche a classe CSS na div externa da marcação HTML do componente.

### Aplicar o estilo de sublinhado

Como autor, vamos aplicar o estilo sublinhado a determinados Componentes de título.

1. Navegue até o artigo **La Skateparks** no editor do AEM Sites em: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. No modo de **Edição**, escolha um componente de Título. Clique no ícone de **pincel** e selecione o estilo **Sublinhado**:

   ![Aplicar o Estilo de Sublinhado](assets/style-system/apply-underline-style-title.png)

   >[!NOTE]
   >
   > Neste ponto, nenhuma alteração visível ocorre, pois o estilo `underline` não foi implementado. No próximo exercício, esse estilo será implementado.

1. Clique no ícone **Informações da página** > **Exibir como publicada** para inspecionar a página fora do editor do AEM.
1. Use as ferramentas de desenvolvedor do seu navegador para verificar se a marcação ao redor do componente de Título tem a classe CSS `cmp-title--underline` aplicada ao div externo.

   ![Div com classe sublinhada aplicada](assets/style-system/div-underline-class-applied.png)

   ```html
   <div class="title cmp-title--underline">
       <div data-cmp-data-layer="{&quot;title-b6450e9cab&quot;:{&quot;@type&quot;:&quot;wknd/components/title&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-23T17:34:42Z&quot;,&quot;dc:title&quot;:&quot;Vans Off the Wall Skatepark&quot;}}" 
       id="title-b6450e9cab" class="cmp-title">
           <h2 class="cmp-title__text">Vans Off the Wall Skatepark</h2>
       </div>
   </div>
   ```

### Implementar o estilo de sublinhado - ui.frontend

Em seguida, implemente o estilo Underline usando o módulo **ui.frontend** do projeto do AEM. O servidor de desenvolvimento do webpack que é fornecido com o módulo **ui.frontend** para visualizar os estilos *antes* de implantar em uma instância local do AEM é usado.

1. Iniciar o processo `watch` no módulo **ui.frontend**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

   Isso inicia um processo que monitora as alterações no módulo `ui.frontend` e sincroniza as alterações na instância do AEM.


1. Retorne ao IDE e abra o arquivo `_title.scss` de: `ui.frontend/src/main/webpack/components/_title.scss`.
1. Introduza uma nova regra direcionada à classe `cmp-title--underline`:

   ```scss
   /* Default Title Styles */
   .cmp-title {}
   .cmp-title__text {}
   .cmp-title__link {}
   
   /* Add Title Underline Style */
   .cmp-title--underline {
       .cmp-title__text {
           &:after {
           display: block;
               width: 84px;
               padding-top: 8px;
               content: '';
               border-bottom: 2px solid $brand-primary;
           }
       }
   }
   ```

   >[!NOTE]
   >
   >É considerada uma prática recomendada definir sempre os estilos como escopo para o componente de destino. Isso garante que estilos extras não afetem outras áreas da página.
   >
   >Todos os Componentes principais aderem à **[notação BEM](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**. É uma prática recomendada direcionar a classe CSS externa ao criar um estilo padrão para um componente. Outra prática recomendada é direcionar nomes de classe especificados pela notação BEM dos Componentes principais em vez de elementos do HTML.

1. Retorne ao navegador e à página do AEM. Você verá que o estilo Underline foi adicionado:

   ![Estilo de sublinhado visível no servidor de desenvolvimento do webpack](assets/style-system/underline-implemented-webpack.png)

1. No Editor do AEM, agora você pode ativar e desativar o estilo **Sublinhado** e ver que as alterações são refletidas visualmente.

## Estilo do bloco de aspas - Texto {#text-component}

Em seguida, repita etapas semelhantes para aplicar um estilo exclusivo ao [Componente de texto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/text.html). O componente de Texto foi enviado por proxy para o projeto em `/apps/wknd/components/text` como parte do módulo **ui.apps**. Os estilos padrão de elementos de parágrafo já foram implementados no **ui.frontend**.

Os [designs do Artigo da WKND](assets/pages-templates/wknd-article-design.xd) contêm um estilo exclusivo para o componente de Texto com um bloco de aspas:

![Estilo do Bloco de Cotas - Componente de Texto](assets/style-system/quote-block-style.png)

### Adicionar uma política de texto

Em seguida, adicione uma política para os componentes de Texto.

1. Navegue até o **Modelo da Página de Artigo** de: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html).

1. No modo **Estrutura**, no **Contêiner de Layout** principal, selecione o ícone **Política** ao lado do componente **Texto** listado em *Componentes Permitidos*:

   ![Configuração da Política de Texto](assets/style-system/article-template-text-policy-icon.png)

1. Atualize a política do componente de Texto com os seguintes valores:

   *Título da Política&#42;*: **Texto do Conteúdo**

   *Plug-ins* > *Estilos de parágrafo* > *Habilitar estilos de parágrafo*

   *Guia Estilos* > *Adicionar um novo estilo*

   **Bloco de Cotação** : `cmp-text--quote`

   ![Política de Componente de Texto](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![Política 2](assets/style-system/text-policy-enable-quotestyle.png) do componente de Texto

   Clique em **Concluído** para salvar as alterações na política de texto.

### Aplicar o Estilo de Bloco de Cotação

1. Navegue até o artigo **La Skateparks** no editor do AEM Sites em: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. No modo **Editar**, escolha um componente de Texto. Edite o componente para incluir um elemento de aspas:

   ![Configuração do componente de Texto](assets/style-system/configure-text-component.png)

1. Selecione o componente de texto, clique no ícone de **pincel** e selecione o estilo de **Bloco de Aspas**:

   ![Aplicar o Estilo de Bloco de Cotação](assets/style-system/quote-block-style-applied.png)

1. Use as ferramentas de desenvolvedor do navegador para inspecionar a marcação. Você deve ver que o nome de classe `cmp-text--quote` foi adicionado à div externa do componente:

   ```html
   <!-- Quote Block style class added -->
   <div class="text cmp-text--quote">
       <div data-cmp-data-layer="{&quot;text-60910f4b8d&quot;:{&quot;@type&quot;:&quot;wknd/components/text&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-24T00:55:26Z&quot;,&quot;xdm:text&quot;:&quot;<blockquote>&amp;nbsp; &amp;nbsp; &amp;nbsp;&amp;quot;There is no better place to shred then Los Angeles&amp;quot;</blockquote>\r\n<p>- Jacob Wester, Pro Skater</p>\r\n&quot;}}" id="text-60910f4b8d" class="cmp-text">
           <blockquote>&nbsp; &nbsp; &nbsp;"There is no better place to shred then Los Angeles"</blockquote>
           <p>- Jacob Wester, Pro Skater</p>
       </div>
   </div>
   ```

### Implementar o estilo de bloco de cotação - ui.frontend

A seguir, vamos implementar o estilo de Bloco de Cotação usando o módulo **ui.frontend** do projeto do AEM.

1. Se ainda não estiver em execução, inicie o processo `watch` de dentro do módulo **ui.frontend**:

   ```shell
   $ npm run watch
   ```

1. Atualizar o arquivo `text.scss` de: `ui.frontend/src/main/webpack/components/_text.scss`:

   ```css
   /* Default text style */
   .cmp-text {}
   .cmp-text__paragraph {}
   
   /* WKND Text Quote style */
   .cmp-text--quote {
       .cmp-text {
           background-color: $brand-third;
           margin: 1em 0em;
           padding: 1em;
   
           blockquote {
               border: none;
               font-size: $font-size-large;
               font-family: $font-family-serif;
               padding: 14px 14px;
               margin: 0;
               margin-bottom: 0.5em;
   
               &:after {
                   border-bottom: 2px solid $brand-primary; /*yellow border */
                   content: '';
                   display: block;
                   position: relative;
                   top: 0.25em;
                   width: 80px;
               }
           }
           p {
               font-family:  $font-family-serif;
           }
       }
   }
   ```

   >[!CAUTION]
   >
   > Nesse caso, elementos HTML brutos são direcionados pelos estilos. Isso ocorre porque o componente de Texto fornece um Editor de Rich Text para autores de conteúdo. A criação de estilos diretamente em relação ao conteúdo do RTE deve ser feita com cuidado, e é ainda mais importante definir o escopo mais preciso dos estilos.

1. Retorne ao navegador mais uma vez e você verá que o estilo de bloco Cotação foi adicionado:

   ![Estilo do bloco de aspas visível](assets/style-system/quoteblock-implemented.png)

1. Interrompa o servidor de desenvolvimento do webpack.

## Largura fixa - Contêiner (Bônus) {#layout-container}

Os componentes do container foram usados para criar a estrutura básica do Modelo de página de artigo e fornecer as zonas de lançamento para que os autores de conteúdo adicionem conteúdo a uma página. Os containers também podem usar o Sistema de estilos, fornecendo aos autores de conteúdo ainda mais opções para projetar layouts.

O **Contêiner Principal** do modelo de Página de Artigo contém os dois contêineres que podem ser criados e tem uma largura fixa.

![Contêiner principal](assets/style-system/main-container-article-page-template.png)

*Contêiner principal no Modelo de página de artigo*.

A política do **Contêiner principal** define o elemento padrão como `main`:

![Política do Contêiner Principal](assets/style-system/main-container-policy.png)

O CSS que corrige o **Contêiner principal** está definido no módulo **ui.frontend** em `ui.frontend/src/main/webpack/site/styles/container_main.scss`:

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

Em vez de direcionar o elemento HTML `main`, o Sistema de Estilos pode ser usado para criar um estilo de **Largura fixa** como parte da política de Contêiner. O Sistema de Estilos pode dar aos usuários a opção de alternar entre contêineres de **Largura fixa** e **Largura fluida**.

1. **Desafio de Bônus** - use as lições aprendidas com os exercícios anteriores e use o Sistema de Estilos para implementar estilos de **Largura fixa** e **Largura fluida** para o componente de Contêiner.

## Parabéns. {#congratulations}

Parabéns, a página do artigo está quase no estilo, e você ganhou experiência prática usando o Sistema de Estilos do AEM.

### Próximas etapas {#next-steps}

Saiba mais sobre as etapas completas para criar um [componente personalizado do AEM](custom-component.md) que exiba conteúdo criado em uma caixa de diálogo e explore o desenvolvimento de um Modelo do Sling para encapsular a lógica de negócios que preencha o HTL do componente.

Visualize o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente em na ramificação Git `tutorial/style-system-solution`.

1. Clonar o repositório [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Confira a ramificação `tutorial/style-system-solution`.
