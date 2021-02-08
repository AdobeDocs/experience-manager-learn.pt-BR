---
title: Introdução ao AEM Sites - Páginas e modelos
seo-title: Introdução ao AEM Sites - Páginas e modelos
description: Saiba mais sobre a relação entre um componente de página base e modelos editáveis. Entenda como os Componentes principais são enviados em proxy no projeto e saiba mais sobre as configurações avançadas de política de modelos editáveis para criar um modelo bem estruturado de Página de artigo com base em um modelo da Adobe XD.
sub-product: sites
feature: template-editor, core-components
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
translation-type: tm+mt
source-git-commit: 76462bb75ceda1921db2fa37606ed7c5a1eadb81
workflow-type: tm+mt
source-wordcount: '3074'
ht-degree: 0%

---


# Páginas e modelos {#pages-and-template}

Neste capítulo, exploraremos a relação entre um componente de página base e modelos editáveis. Criaremos um modelo de artigo sem estilo com base em alguns modelos do [AdobeXD](https://www.adobe.com/products/xd.html). No processo de criação do modelo, os Componentes principais e as configurações avançadas de política dos Modelos editáveis são abordados.

## Pré-requisitos {#prerequisites}

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Projeto inicial

>[!NOTE]
>
> Se você tiver concluído com êxito o capítulo anterior, poderá reutilizar o projeto e ignorar as etapas para fazer check-out do projeto inicial.

Confira o código básico no qual o tutorial se baseia:

1. Verifique a ramificação `tutorial/pages-templates-start` de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
   ```

1. Implante a base de código para uma instância AEM local usando suas habilidades Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se estiver usando AEM 6.5 ou 6.4, anexe o perfil `classic` a qualquer comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution) ou fazer check-out do código localmente ao alternar para a ramificação `tutorial/pages-templates-solution`.

## Objetivo

1. Inspect um design de página criado no Adobe XD e mapeie-o para Componentes principais.
1. Entenda os detalhes de Modelos editáveis e como as políticas podem ser usadas para impor o controle granular do conteúdo da página.
1. Saiba como Modelos e páginas são vinculados

## O que você vai criar {#what-you-will-build}

Nesta parte do tutorial, você criará um novo Modelo de página de artigo que pode ser usado para criar novas páginas de artigo e se alinha a uma estrutura comum. O Modelo de página de artigo será baseado em designs e em um Kit de interface de usuário produzido no AdobeXD. Este capítulo está focado apenas na construção da estrutura ou esqueleto do modelo. Nenhum estilo será implementado, mas o modelo e as páginas estarão funcionais.

![Design da página do artigo e versão sem estilo](assets/pages-templates/what-you-will-build.png)

## Planejamento da interface com o Adobe XD {#adobexd}

Na maioria dos casos, o planejamento de novos start de site com modelos e designs estáticos. [Adobe ](https://www.adobe.com/products/xd.html) XD é uma ferramenta de design que constrói experiências do usuário. Em seguida, inspecionaremos um kit de interface do usuário e modelos para ajudar a planejar a estrutura do modelo de página do artigo.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**Baixe o arquivo [ de design de artigo ](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)** WKND.

## Criar o modelo de página de artigo

Ao criar uma página, você deve selecionar um modelo, que será usado como a base de criação da nova página. O modelo define a estrutura da página resultante, o conteúdo inicial e os componentes permitidos.

Há três áreas principais de [Modelos editáveis](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html):

1. **Estrutura**  - define os componentes que fazem parte do modelo. Eles não serão editáveis pelos autores de conteúdo.
1. **Conteúdo**  inicial - define os componentes com os quais o modelo será start, que podem ser editados e/ou excluídos pelos autores do conteúdo
1. **Políticas**  - define configurações sobre como os componentes se comportarão e quais opções os autores terão disponíveis.

Em seguida, crie um novo modelo em AEM que corresponda à estrutura dos modelos. Isso ocorrerá em uma instância local do AEM. Siga as etapas no vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/330991/?quality=12&learn=on)

Etapas de alto nível para o vídeo abaixo:

### Configurações de estrutura

1. Crie um novo modelo usando o **Tipo de modelo de página**, chamado **Página do artigo**.
1. Alterne para o modo **Estrutura**.
1. Adicione um componente **Fragmento de experiência** para agir como o **Cabeçalho** na parte superior do modelo.
   * Configure o componente para apontar para `/content/experience-fragments/wknd/us/en/site/header/master`.
   * Defina a política como **Cabeçalho da página** e certifique-se de que **Elemento padrão** esteja definido como `header`. O elemento `header`será direcionado com CSS no próximo capítulo.
1. Adicione um componente **Fragmento de experiência** para agir como o **rodapé** na parte inferior do modelo.
   * Configure o componente para apontar para `/content/experience-fragments/wknd/us/en/site/footer/master`.
   * Defina a política como **Rodapé da página** e certifique-se de que **Elemento padrão** esteja definido como `footer`. O elemento `footer` será direcionado com CSS no próximo capítulo.
1. Bloqueie o container **main** que foi incluído quando o modelo foi criado pela primeira vez.
   * Defina a política como **Página principal** e certifique-se de que **Elemento padrão** esteja definido como `main`. O elemento `main` será direcionado com CSS no próximo capítulo.
1. Adicione um componente **Image** ao container **main**.
   * Desbloqueie o componente **Image**.
1. Adicione um componente **Breadcrumb** abaixo do componente **Image** no container principal.
   * Crie uma nova política para o componente **Trilha de navegação** chamado **Página do artigo - Trilha de navegação**. Defina **Nível do Start de navegação** como **4**.
1. Adicione um componente **Container** abaixo do componente **Trilha de navegação** e dentro do container **main**. Isso atuará como o **container de conteúdo** para o modelo.
   * Desbloqueie o container **Content**.
   * Defina a política como **Conteúdo da página**.
1. Adicione outro componente **Container** abaixo de **container de conteúdo**. Isso atuará como o container **Side Rail** do modelo.
   * Destrave o container **Side Rail**.
   * Crie uma nova política chamada **Página do artigo - Painel lateral**.
   * Configure **Componentes permitidos** em **Projeto de sites WKND - Conteúdo** para incluir: **Botão**, **Transferir**, **Imagem**, **Lista**, **Separador**, **Partilha de Mídia Social**, &lt;a 6/>Texto **e** Título **.**
1. Atualize a política do container raiz da página. Este é o container mais externo do modelo. Defina a política para **Raiz da página**.
   * Em **Configurações do Container**, defina **Layout** como **Grade Responsiva**.
1. Ative o Modo de layout para o **container de conteúdo**. Arraste a alça da direita para a esquerda e diminua o container para ter 8 colunas de largura.
1. Ative o modo Layout para o **container do painel lateral**. Arraste a alça da direita para a esquerda e reduza o container para ter 4 colunas de largura. Em seguida, arraste a alça esquerda para a direita 1 coluna para tornar o container 3 colunas largo e deixar uma lacuna de 1 coluna entre **container de conteúdo**.
1. Abra o emulador móvel e alterne para um ponto de interrupção móvel. Ative o modo de layout novamente e torne o **container de conteúdo** e o **container do painel lateral** a largura total da página. Isso empilhará os container verticalmente no ponto de interrupção móvel.
1. Atualize a política do componente **Text** no **container de conteúdo**.
   * Defina a política para **Texto do conteúdo**.
   * Em **Plug-ins** > **Estilos de parágrafo**, marque **Ativar estilos de parágrafo** e certifique-se de que **bloco de citação** esteja ativado.

### Configurações iniciais do conteúdo

1. Alterne para o modo **Conteúdo inicial**.
1. Adicione um componente **Title** ao **container de conteúdo**. Isso atuará como o título do artigo. Quando deixado vazio, exibirá automaticamente o Título da página atual.
1. Adicione um segundo componente **Title** abaixo do primeiro componente de Título.
   * Configure o componente com o texto: &quot;Por autor&quot;. Esse será um espaço reservado para texto.
   * Defina o tipo como `H4`.
1. Adicione um componente **Text** abaixo do componente **By Author** Title.
1. Adicione um componente **Title** ao Container **Side Rail**.
   * Configure o componente com o texto: &quot;Compartilhe esta história&quot;.
   * Defina o tipo como `H5`.
1. Adicione um componente **Compartilhamento de mídia social** abaixo do componente **Compartilhar esta história** Título.
1. Adicione um componente **Separator** abaixo do componente **Compartilhamento de mídia social**.
1. Adicione um componente **Download** abaixo do componente **Separador**.
1. Adicione um componente **Lista** abaixo do componente **Download**.
1. Atualize **Propriedades da página inicial** para o modelo.
   * Em **Mídia social** > **Compartilhamento de mídia social**, marque **Facebook** e **Pinterest**

### Ative o modelo e adicione uma miniatura

1. Visualização o modelo no console Modelo navegando até [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **** Habilite o modelo Página de artigo.
1. Edite as propriedades do modelo Página do artigo e carregue a seguinte miniatura para identificar rapidamente as páginas criadas usando o modelo Página do artigo:

   ![Miniatura do modelo de página do artigo](assets/pages-templates/article-page-template-thumbnail.png)

## Atualize o cabeçalho e o rodapé com fragmentos de experiência {#experience-fragments}

Uma prática comum ao criar conteúdo global, como um cabeçalho ou rodapé, é usar um [Fragmento de experiência](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Fragmentos de experiência, permite que os usuários combinem vários componentes para criar um único componente com capacidade de referência. Os Fragmentos de experiência têm a vantagem de suportar o gerenciamento de vários sites e [localização](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

O AEM Project Archetype gerou um Cabeçalho e rodapé. Em seguida, atualize os Fragmentos de experiência para corresponder aos modelos. Siga as etapas no vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/330992/?quality=12&learn=on)

Etapas de alto nível para o vídeo abaixo:

1. Baixe o pacote de conteúdo de amostra **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets.zip)**.
1. Carregue e instale o pacote de conteúdo usando o Gerenciador de pacotes em [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Atualize o modelo de Variação da Web, que é o modelo usado para Fragmentos de experiência em [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)
   * Atualize a política do componente **Container** no modelo.
   * Defina a política para **Raiz XF**.
   * Em **Componentes permitidos** selecione o grupo de componentes **Projeto de Sites WKND - Estrutura** para incluir **Navegação de idioma**, **Navegação** e **Componentes de Pesquisa rápida**.

### Atualizar fragmento da experiência do cabeçalho

1. Abra o fragmento de experiência que renderiza o cabeçalho em [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. Configure a raiz **Container** do fragmento. Este é o Container mais externo ****.
   * Defina **Layout** como **Grade responsiva**
1. Adicione o **logotipo escuro WKND** como uma imagem na parte superior do Container **a3/>.** O logotipo foi incluído no pacote instalado em uma etapa anterior.
   * Modifique o layout do logotipo escuro **WKND** para ter **2** colunas de largura. Arraste as alças da direita para a esquerda.
   * Configure o logotipo com **Texto alternativo** de &quot;Logotipo WKND&quot;.
   * Configure o logotipo para **Link** para `/content/wknd/us/en` o Home page.
1. Configure o componente **Navigation** que já está colocado na página.
   * Defina **Excluir níveis raiz** como **1**.
   * Defina **Profundidade da estrutura de navegação** como **1**.
   * Modifique o layout do componente **Navigation** para ter **8** colunas de largura. Arraste as alças da direita para a esquerda.
1. Remova o componente **Navegação de idioma**.
1. Modifique o layout do componente **Search** para ter **2** colunas de largura. Arraste as alças da direita para a esquerda. Todos os componentes agora devem ser alinhados horizontalmente em uma única linha.

### Atualizar fragmento de experiência do rodapé

1. Abra o Fragmento de experiência que renderiza o rodapé em [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. Configure a raiz **Container** do fragmento. Este é o Container mais externo ****.
   * Defina **Layout** como **Grade responsiva**
1. Adicione o **logotipo de luz WKND** como uma imagem à parte superior do Container **a3/>.** O logotipo foi incluído no pacote instalado em uma etapa anterior.
   * Modifique o layout do **logotipo claro WKND** para ter **2** colunas de largura. Arraste as alças da direita para a esquerda.
   * Configure o logotipo com **Texto alternativo** de &quot;Luz do logotipo WKND&quot;.
   * Configure o logotipo para **Link** para `/content/wknd/us/en` o Home page.
1. Adicione um componente **Navigation** abaixo do logotipo. Configure o componente **Navigation**:
   * Defina **Excluir níveis raiz** como **1**.
   * Desmarque **Coletar todas as páginas secundárias**.
   * Defina **Profundidade da estrutura de navegação** como **1**.
   * Modifique o layout do componente **Navigation** para ter **8** colunas de largura. Arraste as alças da direita para a esquerda.

## Criar uma página de artigo

Em seguida, crie uma nova página usando o modelo Página do artigo. Crie o conteúdo da página para corresponder aos modelos do site. Siga as etapas no vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/330993/?quality=12&learn=on)

Etapas de alto nível para o vídeo abaixo:

1. Navegue até o console Sites em [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. Crie uma nova página abaixo de **WKND** > **US** > **EN** > **Revista**.
   * Escolha o modelo **Página do artigo**.
   * Em **Propriedades** defina **Título** como &quot;Ultimate Guide to LA Skatepark&quot; (Guia Ultimate para os Skatepark)
   * Defina **Name** como &quot;guide-la-skatepark&quot;
1. Substitua o título **Por autor** pelo texto &quot;By Stacey Roswells&quot;.
1. Atualize o componente **Texto** para incluir um parágrafo para preencher o artigo. Você pode usar o seguinte arquivo de texto como cópia: [la-skate-park-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. Adicione outro componente **Text**.
   * Atualize o componente para incluir a cotação: &quot;Não há lugar melhor para destruir que Los Angeles.&quot;
   * Edite o Editor de Rich Text no modo de tela cheia e modifique a citação acima para usar o elemento **Bloco de aspas**.
1. Continue preenchendo o corpo do artigo para corresponder aos modelos.
1. Configure o componente **Download** para usar uma versão PDF do artigo.
   * Em **Download** > **Propriedades**, clique na caixa de seleção para **Obter o título do ativo DAM**.
   * Defina **Description** como: &quot;Obtenha a história completa&quot;.
   * Defina **Texto de ação** como: &quot;Download de PDF&quot;.
1. Configure o componente **Lista**.
   * Em **Configurações de Lista** > **Criar Lista usando**, selecione **Páginas secundárias**.
   * Defina **Página principal** como `/content/wknd/us/en/magazine`.
   * Em **Configurações do item** marque **Vincular itens** e marque **Mostrar data**.

## Inspect a estrutura de nó {#node-structure}

Nesse ponto, a página do artigo está claramente sem estilo. No entanto, a estrutura básica está em vigor. Em seguida, inspecione a estrutura de nós da página do artigo para obter uma melhor compreensão da função do modelo, da página e dos componentes.

Use a ferramenta CRXDE-Lite em uma instância AEM local para visualização da estrutura do nó subjacente.

1. Abra [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) e use a navegação em árvore para navegar até `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Clique no nó `jcr:content` abaixo da página `la-skateparks` e visualização as propriedades:

   ![Propriedades do conteúdo JCR](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Observe o valor de `cq:template`, que aponta para `/conf/wknd/settings/wcm/templates/article-page`, o Modelo de página de artigo que criamos anteriormente.

   Observe também o valor de `sling:resourceType`, que aponta para `wknd/components/page`. Esse é o componente de página criado pelo arquétipo de projeto AEM e é responsável pela renderização da página com base no modelo.

1. Expanda o nó `jcr:content` abaixo de `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` e visualização a hierarquia do nó:

   ![JCR Conteúdo LA Skatepark](assets/pages-templates/page-jcr-structure.png)

   Você deve ser capaz de mapear livremente cada um dos nós para os componentes que foram criados. Verifique se você pode identificar os diferentes Container de layout usados inspecionando os nós com prefixo `container`.

1. Em seguida, inspecione o componente de página em `/apps/wknd/components/page`. Visualização as propriedades do componente no CRXDE Lite:

   ![Propriedades do componente de página](assets/pages-templates/page-component-properties.png)

   Observe que há apenas 2 scripts HTL, `customfooterlibs.html` e `customheaderlibs.html` abaixo do componente de página. *Então, como esse componente renderiza a página?*

   A propriedade `sling:resourceSuperType` aponta para `core/wcm/components/page/v2/page`. Essa propriedade permite que o componente de página do WKND herde **all** da funcionalidade do componente de página Componente Principal. Este é o primeiro exemplo de algo chamado [Padrão do componente proxy](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). Mais informações podem ser encontradas [aqui.](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect outro componente dentro dos componentes WKND, o componente `Breadcrumb` localizado em: `/apps/wknd/components/breadcrumb`. Observe que a mesma propriedade `sling:resourceSuperType` pode ser encontrada, mas desta vez ela aponta para `core/wcm/components/breadcrumb/v2/breadcrumb`. Este é outro exemplo de uso do padrão do componente Proxy para incluir um Componente principal. Na verdade, todos os componentes na base de código WKND são proxies de AEM componentes principais (exceto o nosso famoso componente HelloWorld). É uma prática recomendada tentar reutilizar o máximo possível da funcionalidade dos Componentes principais *antes de* gravar o código personalizado.

1. Em seguida, inspecione a página do componente principal em `/libs/core/wcm/components/page/v2/page` usando o CRXDE Lite:

   >[!NOTE]
   >
   > No AEM 6.5/6.4, os Componentes principais estão localizados em `/apps/core/wcm/components`. No AEM como um Cloud Service, os Componentes principais estão localizados em `/libs` e são atualizados automaticamente.

   ![Página do componente principal](assets/pages-templates/core-page-component-properties.png)

   Observe que muitos outros scripts estão incluídos abaixo desta página. A Página principal do componente contém muita funcionalidade. Essa funcionalidade é dividida em vários scripts para facilitar a manutenção e a leitura. Você pode rastrear a inclusão dos scripts HTL abrindo `page.html` e procurando `data-sly-include`:

   ```html
   <!--/* /libs/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
       data-sly-use.head="head.html"
       data-sly-use.footer="footer.html"
       data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}"
           id="${page.id}"
           data-cmp-data-layer-enabled="${page.data ? true : false}">
           <script data-sly-test.dataLayerEnabled="${page.data}">
           window.adobeDataLayer = window.adobeDataLayer || [];
           adobeDataLayer.push({
               page: JSON.parse("${page.data.json @ context='scriptString'}"),
               event:'cmp:show',
               eventInfo: {
                   path: 'page.${page.id @ context="scriptString"}'
               }
           });
           </script>
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.skiptomaincontent.html"></sly>
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   A outra razão para dividir o HTL em vários scripts é permitir que os componentes proxy substituam scripts individuais para implementar a lógica comercial personalizada. Os scripts HTL, `customfooterlibs.html` e `customheaderlibs.html`, são criados para a finalidade explícita a ser substituída pela implementação de projetos.

   Você pode saber mais sobre como o Modelo editável influencia a renderização da página de conteúdo [lendo este artigo](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html).

1. Inspect o outro componente principal, como a navegação estrutural em `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Visualização o script `breadcrumb.html` para entender como a marcação do componente de navegação estrutural é gerada.

## Salvando configurações no controle de origem {#configuration-persistence}

Em muitos casos, especialmente no início de um projeto AEM é importante persistir em configurações, como modelos e políticas de conteúdo relacionadas, para o controle de origem. Isso garante que todos os desenvolvedores estejam trabalhando em relação ao mesmo conjunto de conteúdo e configurações e pode garantir uma consistência adicional entre os ambientes. Quando um projeto atinge um certo nível de maturidade, a prática de gerenciar modelos pode ser transferida para um grupo especial de usuários avançados.

Por enquanto, trataremos os modelos como outros códigos e sincronizaremos o **Modelo de página do artigo** para baixo como parte do projeto. Até agora, temos o código **enviado** do nosso projeto AEM para uma instância local do AEM. O **Modelo de página de artigo** foi criado diretamente em uma instância local do AEM, portanto precisamos **importar** o modelo para nosso projeto AEM. O módulo **ui.content** está incluído no projeto AEM para essa finalidade específica.

As próximas etapas serão executadas usando o IDE VSCode usando o plug-in [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview), mas podem ser feitas usando qualquer IDE configurado para **importar** ou importar conteúdo de uma instância local do AEM.

1. No VSCode, abra o projeto `aem-guides-wknd`.

1. Expanda o módulo **ui.content** no Explorador de projetos. Expanda a pasta `src` e navegue até `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Clique com o botão direito do mouse ] na  `templates` pasta e selecione  **Importar do servidor** AEM:

   ![Template de importação VSCode](assets/pages-templates/vscode-import-templates.png)

   Os modelos `article-page` devem ser importados e `page-content`, `xf-web-variation` também devem ser atualizados.

   ![Modelos atualizados](assets/pages-templates/updated-templates.png)

1. Repita as etapas para importar conteúdo, mas selecione a pasta **policies** localizada em `/conf/wknd/settings/wcm/policies`.

   ![Políticas de importação de VSCode](assets/pages-templates/policies-article-page-template.png)

1. Inspect o arquivo `filter.xml` localizado em `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd" mode="merge"/>
       <filter root="/content/wknd" mode="merge"/>
       <filter root="/content/dam/wknd" mode="merge"/>
       <filter root="/content/experience-fragments/wknd" mode="merge"/>
   </workspaceFilter>
   ```

   O arquivo `filter.xml` é responsável por identificar os caminhos dos nós que serão instalados com o pacote. Observe `mode="merge"` em cada um dos filtros que indica que o conteúdo existente não será modificado, somente o novo conteúdo será adicionado. Como os autores de conteúdo podem estar atualizando esses caminhos, é importante que uma implantação de código **e não** substitua o conteúdo. Consulte a [documentação do FileVault](https://jackrabbit.apache.org/filevault/filter.html) para obter mais detalhes sobre como trabalhar com elementos de filtro.

   Compare `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` para entender os diferentes nós gerenciados por cada módulo.

   >[!WARNING]
   >
   > Para garantir implantações consistentes para o site de referência WKND, alguns ramos do projeto são configurados de modo que `ui.content` substituam quaisquer alterações no JCR. Isso é feito por design, isto é, para Ramificações de soluções, já que o código/estilos serão escritos para políticas específicas.

## Parabéns! {#congratulations}

Parabéns, você acabou de criar um novo modelo e página com a Adobe Experience Manager Sites.

### Próximas etapas {#next-steps}

Nesse ponto, a página do artigo está claramente sem estilo. Siga o tutorial [Bibliotecas do lado do cliente e Fluxo de trabalho do front-end](client-side-libraries.md) para aprender as práticas recomendadas para incluir CSS e Javascript para aplicar estilos globais ao site e integrar uma compilação de front-end dedicada.

Visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente no bloco Git `tutorial/pages-templates-solution`.

1. Clique no repositório [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Confira a ramificação `tutorial/pages-templates-solution`.
