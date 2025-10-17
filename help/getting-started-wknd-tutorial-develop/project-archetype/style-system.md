---
title: Desenvolver com o sistema de estilos
description: Saiba como implementar estilos individuais e reutilizar os componentes principais com o sistema de estilos do Experience Manager. Este tutorial abrange o desenvolvimento do sistema de estilos, a fim de estender os componentes principais com o CSS específico da marca e as configurações de política avançadas do editor de modelos.
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
workflow-type: ht
source-wordcount: '1585'
ht-degree: 100%

---

# Desenvolver com o sistema de estilos {#developing-with-the-style-system}

Saiba como implementar estilos individuais e reutilizar os componentes principais com o sistema de estilos do Experience Manager. Este tutorial abrange o desenvolvimento do sistema de estilos, a fim de estender os componentes principais com o CSS específico da marca e as configurações de política avançadas do editor de modelos.

## Pré-requisitos {#prerequisites}

Consulte as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

Também é recomendável conferir o tutorial [Bibliotecas do lado do cliente e fluxo de trabalho de front-end](client-side-libraries.md) para entender os fundamentos das bibliotecas do lado do cliente e as várias ferramentas de front-end integradas ao projeto do AEM.

### Projeto inicial

>[!NOTE]
>
> Se você tiver concluído com sucesso o capítulo anterior, poderá reutilizar o projeto e ignorar as etapas para conferir o projeto inicial.

Confira o código de linha de base no qual o tutorial se baseia:

1. Confira a ramificação `tutorial/style-system-start` do [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
   ```

1. Implante a base de código em uma instância do AEM local, usando as suas habilidades do Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se estiver usando o AEM 6.5 ou 6.4, anexe o perfil `classic` a qualquer comando do Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

É sempre possível exibir o código concluído no [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution) ou conferir o código localmente, alternando-se para a ramificação `tutorial/style-system-solution`.

## Objetivo

1. Entender como usar o sistema de estilos para aplicar o CSS específico da marca aos componentes principais do AEM.
1. Aprender sobre a notação BEM e como ela pode ser usada para definir estilos com cuidado.
1. Aplicar configurações de política avançadas com os modelos editáveis.

## O que você criará {#what-build}

Este capítulo usará o [recurso do sistema de estilos](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html) para criar variações dos componentes **Título** e **Texto** usados na página “Artigo”.

![Estilos disponíveis para o título](assets/style-system/styles-added-title.png)

*Estilo de sublinhado disponível para uso com o componente de título*

## Fundo {#background}

O [Sistema de estilos](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html) permite que desenvolvedores e editores de modelos criem várias variações visuais de um componente. Os criadores podem decidir qual estilo usar ao compor uma página. O sistema de estilos é usado no restante do tutorial para obter vários estilos únicos ao usar os componentes principais em uma abordagem de código baixo.

A ideia geral do sistema de estilos é que os criadores possam escolher vários estilos de exibição para um componente. Os “estilos” são apoiados por classes CSS adicionais, que são inseridas na div externa de um componente. Nas bibliotecas de clientes, as regras de CSS são adicionadas com base nessas classes de estilo, para que a aparência do componente seja alterada.

Você pode encontrar a [documentação detalhada do sistema de estilos aqui](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/features/style-system.html?lang=pt-BR). Também há um excelente vídeo técnico [ para entender o sistema de estilos](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html).

## Estilo de sublinhado: título {#underline-style}

O [Componente de título](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/title.html) foi encaminhado por proxy para o projeto em `/apps/wknd/components/title` como parte do módulo **ui.apps**. Os estilos padrão de elementos de cabeçalho (`H1`, `H2`, `H3`...) já foram implementados no módulo **ui.frontend**.

Os [designs do artigo da WKND](assets/pages-templates/wknd-article-design.xd) contêm um estilo exclusivo para o componente de título com um sublinhado. Em vez de criar dois componentes ou modificar a caixa de diálogo de componentes, o sistema de estilos pode ser usado para permitir que os criadores adicionem uma opção de estilo de sublinhado.

![Estilo de sublinhado: componente de título](assets/style-system/title-underline-style.png)

### Adicionar um política de título

Vamos adicionar uma política para os componentes de título, a fim de permitir que os criadores de conteúdo escolham o estilo de sublinhado a ser aplicado a componentes específicos. Isso é feito com o editor de modelos no AEM.

1. Navegue até o modelo **Página de artigo** de: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. No modo **Estrutura**, no **Container de layout** principal, selecione o ícone **Política** ao lado do componente **Título** listado em *Componentes permitidos*:

   ![Configuração da política de título](assets/style-system/article-template-title-policy-icon.png)

1. Crie uma política para o componente de título com os seguintes valores:

   *Título da política&#42;*: **Título da WKND**

   *Propriedades* > guia *Estilos* > *Adicionar um novo estilo*

   **Sublinhado** : `cmp-title--underline`

   ![Configuração da política de estilo para o título](assets/style-system/title-style-policy.png)

   Clique em **Concluído** para salvar as alterações na política de título.

   >[!NOTE]
   >
   > O valor `cmp-title--underline` preenche a classe de CSS na div externa da marcação HTML do componente.

### Aplicar o estilo de sublinhado

Como criador, vamos aplicar o estilo de sublinhado a determinados componentes de título.

1. Navegue até o artigo **La Skateparks** no editor do AEM Sites, em: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. No modo **Editar**, escolha um componente de título. Clique no ícone de **pincel** e selecione o estilo de **Sublinhado**:

   ![Aplicar o estilo de sublinhado](assets/style-system/apply-underline-style-title.png)

   >[!NOTE]
   >
   > Neste ponto, nenhuma alteração visível ocorre, pois o estilo `underline` não foi implementado. No próximo exercício, esse estilo será implementado.

1. Clique no ícone **Informações da página** > **Visualizar como publicada** para inspecionar a página fora do editor do AEM.
1. Use as ferramentas de desenvolvedor do seu navegador para verificar se a marcação ao redor do componente de título está com a classe de CSS `cmp-title--underline` aplicada à div externa.

   ![Div com classe de sublinhado aplicada](assets/style-system/div-underline-class-applied.png)

   ```html
   <div class="title cmp-title--underline">
       <div data-cmp-data-layer="{&quot;title-b6450e9cab&quot;:{&quot;@type&quot;:&quot;wknd/components/title&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-23T17:34:42Z&quot;,&quot;dc:title&quot;:&quot;Vans Off the Wall Skatepark&quot;}}" 
       id="title-b6450e9cab" class="cmp-title">
           <h2 class="cmp-title__text">Vans Off the Wall Skatepark</h2>
       </div>
   </div>
   ```

### Implementar o estilo de sublinhado: ui.frontend

Em seguida, implemente o estilo de sublinhado com o módulo **ui.frontend** do projeto do AEM. Utiliza-se o servidor de desenvolvimento do webpack fornecido com o módulo **ui.frontend** para visualizar os estilos *antes* de implantar em uma instância local do AEM.

1. Inicie o processo `watch` de dentro do módulo **ui.frontend**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

   Isso inicia um processo que monitora as alterações no módulo `ui.frontend` e sincroniza as alterações com a instância do AEM.


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
   >Considera-se uma prática recomendada sempre definir bem os estilos do componente de destino. Isso garante que estilos extras não afetem outras áreas da página.
   >
   >Todos os componentes principais seguem a **[notação BEM](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**. É uma prática recomendada direcionar a classe de CSS externa ao criar um estilo padrão para um componente. Outra prática recomendada é direcionar nomes de classes especificados pela notação BEM dos componentes principais em vez de elementos em HTML.

1. Retorne ao navegador e à página do AEM. Você verá que o estilo de sublinhado foi adicionado:

   ![Estilo de sublinhado visível no servidor de desenvolvimento do webpack](assets/style-system/underline-implemented-webpack.png)

1. No editor do AEM, agora você pode ativar e desativar o estilo de **Sublinhado**, e verá que as alterações são refletidas visualmente.

## Estilo de bloco de aspas: texto {#text-component}

Em seguida, repita etapas semelhantes para aplicar um estilo exclusivo ao [componente de texto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/text.html). O componente de texto foi enviado por proxy para o projeto em `/apps/wknd/components/text` como parte do módulo **ui.apps**. Os estilos padrão de elementos de parágrafo já foram implementados no **ui.frontend**.

Os [designs do artigo da WKND](assets/pages-templates/wknd-article-design.xd) contêm um estilo exclusivo para o componente de texto com um bloco de aspas:

![Estilo de bloco de aspas: componente de texto](assets/style-system/quote-block-style.png)

### Adicionar uma política de texto

Em seguida, adicione uma política para os componentes de texto.

1. Navegue até o **Modelo da página de artigo** de: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html).

1. No modo **Estrutura**, no **Container de layout** principal, selecione o ícone **Política** ao lado do componente **Texto** listado em *Componentes permitidos*:

   ![Configuração da política de texto](assets/style-system/article-template-text-policy-icon.png)

1. Atualize a política do componente de texto com os seguintes valores:

   *Título da política&#42;*: **Texto do conteúdo**

   *Plug-ins* > *Estilos de parágrafo* > *Habilitar estilos de parágrafo*

   *Guia Estilos* > *Adicionar um novo estilo*

   **Bloco de aspas** : `cmp-text--quote`

   ![Política do componente de texto](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![Política 2 do componente de texto](assets/style-system/text-policy-enable-quotestyle.png)

   Clique em **Concluído** para salvar as alterações nas política de texto.

### Aplicar o estilo de bloco de aspas

1. Navegue até o artigo **La Skateparks** no editor do AEM Sites, em: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. No modo **Editar**, escolha um componente de texto. Edite o componente para incluir um elemento de aspas:

   ![Configuração do componente de texto](assets/style-system/configure-text-component.png)

1. Selecione o componente de texto, clique no ícone de **pincel** e selecione o estilo do **Bloco de aspas**:

   ![Aplicar o estilo de bloco de aspas](assets/style-system/quote-block-style-applied.png)

1. Use as ferramentas do desenvolvedor do navegador para inspecionar a marcação. Você deve ver que o nome de classe `cmp-text--quote` foi adicionado à div externa do componente:

   ```html
   <!-- Quote Block style class added -->
   <div class="text cmp-text--quote">
       <div data-cmp-data-layer="{&quot;text-60910f4b8d&quot;:{&quot;@type&quot;:&quot;wknd/components/text&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-24T00:55:26Z&quot;,&quot;xdm:text&quot;:&quot;<blockquote>&amp;nbsp; &amp;nbsp; &amp;nbsp;&amp;quot;There is no better place to shred then Los Angeles&amp;quot;</blockquote>\r\n<p>- Jacob Wester, Pro Skater</p>\r\n&quot;}}" id="text-60910f4b8d" class="cmp-text">
           <blockquote>&nbsp; &nbsp; &nbsp;"There is no better place to shred then Los Angeles"</blockquote>
           <p>- Jacob Wester, Pro Skater</p>
       </div>
   </div>
   ```

### Implementar o estilo de bloco de aspas: ui.frontend

A seguir, vamos implementar o estilo de bloco de aspas, usando o módulo **ui.frontend** do projeto do AEM.

1. Se ainda não estiver em execução, inicie o processo `watch` de dentro do módulo **ui.frontend**:

   ```shell
   $ npm run watch
   ```

1. Atualize o arquivo `text.scss` de: `ui.frontend/src/main/webpack/components/_text.scss`:

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
   > Neste caso, elementos HTML brutos são direcionados pelos estilos. Isso ocorre porque o componente de texto fornece um editor de rich text para criadores de conteúdo. A criação de estilos diretamente relacionados ao conteúdo do RTE deve ser feita com cuidado, sendo ainda mais importante definir precisamente os estilos.

1. Retorne ao navegador e verá que o estilo de bloco de aspas foi adicionado:

   ![Estilo de bloco de aspas visível](assets/style-system/quoteblock-implemented.png)

1. Pare o servidor de desenvolvimento do webpack.

## Largura fixa: container (bônus) {#layout-container}

Os componentes do container foram usados para criar a estrutura básica do modelo de página de artigo e fornecer as zonas de pouso, para que os criadores de conteúdo adicionem conteúdo a uma página. Os containers também podem usar o sistema de estilos, fornecendo aos criadores de conteúdo ainda mais opções para projetar layouts.

O **Container principal** do modelo de página de artigo contém os dois containers que podem ser criados e tem uma largura fixa.

![Container principal](assets/style-system/main-container-article-page-template.png)

*Container principal no modelo de página de artigo*.

A política do **Container principal** define o elemento padrão como `main`:

![Política do container principal](assets/style-system/main-container-policy.png)

O CSS que corrige o **Container principal** é definido no módulo **ui.frontend**, em `ui.frontend/src/main/webpack/site/styles/container_main.scss`:

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

Em vez de direcionar o elemento HTML `main`, o sistema de estilos pode ser usado para criar um estilo de **Largura fixa** como parte da política de container. O sistema de estilos pode dar aos usuários a opção de alternar entre containers de **Largura fixa** e **Largura fluida**.

1. **Desafio de bônus**: com base nas lições aprendidas com os exercícios anteriores, use o sistema de estilos para implementar estilos de **Largura fixa** e **Largura fluida** para o componente de container.

## Parabéns! {#congratulations}

Parabéns, a página de artigo está quase estilizada, e você obteve experiência prática com o uso do sistema de estilos do AEM.

### Próximas etapas {#next-steps}

Saiba mais sobre as etapas de ponta a ponta para criar um [componente personalizado do AEM](custom-component.md) que exiba conteúdo criado em uma caixa de diálogo e confira o desenvolvimento de um modelo do Sling para encapsular a lógica de negócios que preenche o HTL do componente.

Veja o código concluído no [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente na ramificação do Git `tutorial/style-system-solution`.

1. Clone o repositório [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Confira a ramificação `tutorial/style-system-solution`.
