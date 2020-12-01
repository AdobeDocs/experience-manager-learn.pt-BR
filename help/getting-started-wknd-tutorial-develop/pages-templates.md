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
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '2226'
ht-degree: 2%

---


# Páginas e modelos {#pages-and-template}

Neste capítulo, exploraremos a relação entre um componente de página base e modelos editáveis. Criaremos um modelo de artigo sem estilo com base em alguns modelos do [AdobeXD](https://www.adobe.com/products/xd.html). No processo de criação do modelo, os Componentes principais e as configurações avançadas de política dos Modelos editáveis são abordados.

## Pré-requisitos {#prerequisites}

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Projeto inicial

Confira o código básico no qual o tutorial se baseia:

1. Clique no repositório [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd).
1. Confira a ramificação `pages-templates/start`.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout pages-templates/start
   ```

1. Implante a base de código para uma instância AEM local usando suas habilidades Maven:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

Você sempre pode visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution) ou fazer check-out do código localmente ao alternar para a ramificação `pages-templates/solution`.

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

Baixe o [Arquivo de design de artigo WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd).

## Criar um cabeçalho e rodapé com fragmentos de experiência {#experience-fragments}

Uma prática comum ao criar conteúdo global, como um cabeçalho ou rodapé, é usar um [Fragmento de experiência](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Fragmentos de experiência, permitem combinar vários componentes para criar um único componente, com capacidade de referência. Os Fragmentos de experiência têm a vantagem de suportar o gerenciamento de vários sites e permitem gerenciar diferentes cabeçalhos/rodapés por localidade.

Em seguida, atualizaremos o Fragmento de experiência destinado a ser usado como cabeçalho e rodapé para adicionar o logotipo WKND.

>[!VIDEO](https://video.tv.adobe.com/v/30215/?quality=12&learn=on)

>[!NOTE]
>
> Seus Fragmentos de experiência parecem diferentes do vídeo? Tente excluí-los e reinstalar a base de código do projeto inicial.

Abaixo estão as etapas de alto nível executadas no vídeo acima.

1. Atualize o cabeçalho do fragmento de experiência localizado em [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html) para incluir o logotipo escuro WKND.

   ![Logotipo Escuro WKND](assets/pages-templates/wknd-logo-dk.png)

   *Logotipo WKND Dark*

1. Atualize o cabeçalho do fragmento de experiência localizado em [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html) para incluir o logotipo de luz WKND.

   ![Logotipo leve WKND](assets/pages-templates/wknd-logo-light.png)

   *Logotipo WKND Light*

## Criar o modelo de página de artigo

Ao criar uma página, você deve selecionar um modelo, que será usado como a base de criação da nova página. O modelo define a estrutura da página resultante, o conteúdo inicial e os componentes permitidos.

Há três áreas principais de [Modelos editáveis](https://docs.adobe.com/content/help/pt-BR/experience-manager-65/developing/platform/templates/page-templates-editable.translate.html):

1. **Estrutura**  - define os componentes que fazem parte do modelo. Eles não serão editáveis pelos autores de conteúdo.
1. **Conteúdo**  inicial - define os componentes com os quais o modelo será start, que podem ser editados e/ou excluídos pelos autores do conteúdo
1. **Políticas**  - define configurações sobre como os componentes se comportarão e quais opções os autores terão disponíveis.

A próxima coisa que faremos é criar o Modelo de página de artigo. Isso ocorrerá em uma instância local do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30217/?quality=12&learn=on)

Abaixo estão as etapas de alto nível executadas no vídeo acima.

1. Navegue até a pasta Modelo de Sites WKND: **Ferramentas** > **Geral** > **Modelos** > **Site WKND**
1. Crie um novo modelo usando o **Site WKND Página Vazia** Tipo de Modelo com um título de **Modelo de Página do Artigo**
1. No modo **Estrutura**, configure o modelo para incluir os seguintes elementos:

   * Cabeçalho do fragmento da experiência
   * Imagem
   * Caminho
   * Container - Desktop de 8 colunas de largura, Tablet de 12 colunas de largura, Dispositivo móvel
   * Container - 4 colunas de largura, 12 colunas de largura Tablet, Dispositivo móvel
   * Rodapé do fragmento de experiência

   ![Modelo de página do artigo do modo Estrutura](assets/pages-templates/article-page-template-structure.png)

   *Estrutura - Modelo de página do artigo*

1. Alterne para o modo **Conteúdo inicial** e adicione os seguintes componentes como conteúdo inicial:

   * **Container principal**
      * Título - tamanho padrão de H1
      * Título - *&quot;Pelo nome do autor&quot;* com um tamanho H4
      * Texto - vazio
   * **Container lateral**
      * Título - *&quot;Compartilhar esta história&quot;* com um tamanho H5
      * Compartilhamento em mídia social
      * Separador
      * Download
      * Lista

   ![Modelo de página do artigo do modo de conteúdo inicial](assets/pages-templates/article-page-template-initialcontent.png)

   *Conteúdo inicial - Modelo da página do artigo*

1. Atualize as **Propriedades iniciais da página** para permitir o compartilhamento de usuários para **Facebook** e **Pinterest**.
1. Carregue uma imagem nas propriedades **Modelo de página do artigo** para identificá-la facilmente:

   ![Miniatura do modelo de página do artigo](assets/pages-templates/article-page-template-thumbnail.png)

   *Miniatura do modelo de página do artigo*

1. Ative os **Modelos de página do artigo** na pasta [Modelos de site WKND](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd/settings/wcm/templates).

## Criar uma página de artigo

Agora que temos um modelo, vamos criar uma nova página usando esse modelo.

1. Baixe o seguinte pacote zip, [WKND-PagesTemplates-DAM-Assets.zip](assets/pages-templates/WKND-PagesTemplates-DAM-Assets.zip) e instale-o por [Gerenciador de pacotes CRX](http://localhost:4502/crx/packmgr/index.jsp).

   O pacote acima instalará várias imagens e ativos abaixo de `/content/dam/wknd/en/magazine/la-skateparks` a serem usados para preencher uma página de artigo em etapas posteriores.

   *As imagens e os ativos no pacote acima são cortesia gratuita do  [Unsplash.com](https://unsplash.com/).*

   ![Amostra de ativos DAM](assets/pages-templates/sample-assets-la-skatepark.png)

1. Crie uma nova página, abaixo de **WKND** > **US** > **en**, chamada **Revista**. Use o modelo **Página de conteúdo**.

   Esta página adicionará alguma estrutura ao nosso site e permitirá que vejamos o componente de navegação estrutural em ação.

1. Em seguida, crie uma nova página, abaixo de **WKND** > **US** > **en** > **Revista**. Use o modelo **Página do artigo**. Use um título de **Guia Ultimate para LA Skatepark** e um nome de **guide-la-skatepark**.

   ![Página de artigo criada inicialmente](assets/pages-templates/create-article-page-nocontent.png)

1. Preencha a página com conteúdo para corresponder aos modelos inspecionados no [Planejamento de interface com a parte AdobeXD](#adobexd). O texto do artigo de amostra pode ser [baixado aqui](assets/pages-templates/la-skateparks-copy.txt). Você deve ser capaz de criar algo semelhante a isto:

   ![Página de artigo preenchida](assets/pages-templates/article-page-unstyled.png)

   >[!NOTE]
   >
   > O componente de Imagem na parte superior da página pode ser editado, mas não removido. O componente de navegação estrutural aparece na página, mas não pode ser editado ou removido.

## Inspect a estrutura de nó {#node-structure}

Nesse ponto, a página do artigo está claramente sem estilo. No entanto, a estrutura básica está em vigor. Em seguida, observaremos a estrutura de nós da página do artigo para obter uma melhor compreensão da função do modelo e do componente de página responsável pela renderização do conteúdo.

Podemos fazer isso usando a ferramenta CRXDE-Lite em uma instância AEM local.

1. Abra [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) e use a navegação em árvore para navegar até `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Clique no nó `jcr:content` abaixo da página `la-skateparks` e visualização as propriedades:

   ![Propriedades do conteúdo JCR](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Observe o valor de `cq:template`, que aponta para `/conf/wknd/settings/wcm/templates/article-page`, o Modelo de página de artigo que criamos anteriormente.

   Observe também o valor de `sling:resourceType`, que aponta para `wknd/components/structure/page`. Esse é o componente de página criado pelo arquétipo de projeto AEM e é responsável pela renderização da página com base no modelo.

1. Expanda o nó `jcr:content` abaixo de `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` e visualização a hierarquia do nó:

   ![JCR Conteúdo LA Skatepark](assets/pages-templates/page-jcr-structure.png)

   Você deve ser capaz de mapear livremente cada um dos nós para os componentes que foram criados. Verifique se você pode identificar os diferentes Container de layout usados inspecionando os nós com prefixo `responsivegrid`.

1. Em seguida, inspecione o componente de página em `/apps/wknd/components/structure/page`. Visualização as propriedades do componente no CRXDE Lite:

   ![Propriedades do componente de página](assets/pages-templates/page-component-properties.png)

   Observe que o componente de página está abaixo de uma pasta chamada **structure**. Esta é uma convenção que corresponde ao modo de estrutura do Editor de modelos e é usada para indicar que o componente da página não é algo com o qual os autores interagirão diretamente.

   Observe que há apenas 2 scripts HTL, `customfooterlibs.html` e `customheaderlibs.html` abaixo do componente de página. Então, como esse componente renderiza a página?

   Observe a propriedade `sling:resourceSuperType` e o valor de `core/wcm/components/page/v2/page`. Essa propriedade permite que o componente de página do WKND herde toda a funcionalidade do componente de página Componente principal. Este é o primeiro exemplo de algo chamado [Padrão do componente proxy](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). Mais informações podem ser encontradas [aqui.](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect outro componente dentro dos componentes WKND, o componente `Breadcrumb` localizado em: `/apps/wknd/components/content/breadcrumb`. Observe que a mesma propriedade `sling:resourceSuperType` pode ser encontrada, mas desta vez ela aponta para `core/wcm/components/breadcrumb/v2/breadcrumb`. Este é outro exemplo de uso do padrão do componente Proxy para incluir um Componente principal. Na verdade, todos os componentes na base de código WKND são proxies de AEM componentes principais (exceto o nosso famoso componente HelloWorld). É uma prática recomendada tentar reutilizar o máximo possível da funcionalidade dos Componentes principais *antes de* gravar o código personalizado.

1. Em seguida, inspecione a página do componente principal em `/apps/core/wcm/components/page/v2/page` usando o CRXDE Lite:

   ![Página do componente principal](assets/pages-templates/core-page-component-properties.png)

   Observe que muitos outros scripts estão incluídos abaixo desta página. A Página principal do componente contém muita funcionalidade. Essa funcionalidade é dividida em vários scripts para facilitar a manutenção e a leitura. Você pode rastrear a inclusão dos scripts HTL abrindo `page.html` e procurando `data-sly-include`:

   ```html
   <!--/* /apps/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
           data-sly-use.head="head.html"
           data-sly-use.footer="footer.html"
           data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}">
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   A outra razão para dividir o HTL em vários scripts é permitir que os componentes proxy substituam scripts individuais para implementar a lógica comercial personalizada. Os scripts HTL, `customfooterlibs.html` e `customheaderlibs.html`, são criados para a finalidade explícita a ser substituída pela implementação de projetos.

   Você pode saber mais sobre como o Modelo editável influencia a renderização da página de conteúdo [lendo este artigo](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/templates/page-templates-editable.html#resultant-content-pages).

1. Inspect o outro componente principal, como a navegação estrutural em `/apps/core/wcm/components/breadcrumb/v2/breadcrumb`. Visualização o script `breadcrumb.html` para entender como a marcação do componente de navegação estrutural é gerada.

## Salvando configurações no controle de origem {#configuration-persistence}

Em muitos casos, especialmente no início de um projeto AEM é importante persistir em configurações, como modelos e políticas de conteúdo relacionadas, para o controle de origem. Isso garante que todos os desenvolvedores estejam trabalhando em relação ao mesmo conjunto de conteúdo e configurações e pode garantir uma consistência adicional entre os ambientes. Quando um projeto atinge um certo nível de maturidade, a prática de gerenciar modelos pode ser transferida para um grupo especial de usuários avançados.

Por enquanto, trataremos os modelos como outros códigos e sincronizaremos o **Modelo de página do artigo** para baixo como parte do projeto. Até agora, temos o código **enviado** do nosso projeto AEM para uma instância local do AEM. O **Modelo de página de artigo** foi criado diretamente em uma instância local do AEM, portanto precisamos **puxar** ou importar o modelo para nosso projeto AEM. O módulo **ui.content** está incluído no projeto AEM para essa finalidade específica.

As próximas etapas serão executadas usando o Eclipse IDE, mas podem ser feitas usando qualquer IDE que você tenha configurado para **pull** ou importar conteúdo de uma instância local do AEM.

1. No Eclipse IDE, verifique se o servidor que conecta o plug-in da ferramenta para desenvolvedores AEM à instância local do AEM foi iniciado e se o módulo **ui.content** foi adicionado à configuração do Servidor.

   ![Conexão com o Eclipse Server](assets/pages-templates/eclipse-server-started.png)

1. Expanda o módulo **ui.content** no Explorador de projetos. Expanda a pasta `src` (a que tem o ícone pequeno em modo globalizado) e navegue até `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Clique com o botão direito do mouse ] no  `templates` nó e selecione  **Importar do servidor...**:

   ![Template de importação Eclipse](assets/pages-templates/eclipse-import-templates.png)

   Confirme a caixa de diálogo **Importar do repositório** e clique em **Concluir**. Agora você deve ver `article-page-template` abaixo da pasta `templates`.

1. Repita as etapas para importar conteúdo, mas selecione o nó **policies** localizado em `/conf/wknd/settings/wcm/policies`.

   ![Políticas de importação do Eclipse](assets/pages-templates/policies-article-page-template.png)

1. Inspect o arquivo `filter.xml` localizado em `src/main/content/META-INF/vault/filter.xml`.

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

Visualização o código finalizado em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente no bloco Git `pages-templates/solution`.

1. Clique no repositório [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Confira a ramificação `pages-templates/solution`.
