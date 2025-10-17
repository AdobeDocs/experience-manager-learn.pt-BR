---
title: Introdução ao AEM Sites - Páginas e modelos
description: Saiba mais sobre a relação entre um componente de página básica e modelos editáveis. Entenda como os componentes principais são encaminhados por proxy ao projeto. Aprenda configurações avançadas de política de modelos editáveis para criar um modelo de página de artigo bem estruturado com base em um modelo do Adobe XD.
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-4082
thumbnail: 30214.jpg
doc-type: Tutorial
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
recommendations: noDisplay, noCatalog
duration: 2049
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '2898'
ht-degree: 100%

---

# Páginas e modelos {#pages-and-template}

{{edge-delivery-services-and-page-editor}}

Neste capítulo, vamos abordar a relação entre um componente de página básica e modelos editáveis. Saiba como criar um modelo de artigo não estilizado com base em alguns modelos do [Adobe XD](https://helpx.adobe.com/br/support/xd.html). Durante o processo de criação do modelo, falaremos sobre os componentes principais e as configurações de política avançadas de modelos editáveis.

## Pré-requisitos {#prerequisites}

Consulte as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Projeto inicial

>[!NOTE]
>
> Se você tiver concluído com sucesso o capítulo anterior, poderá reutilizar o projeto e ignorar as etapas para conferir o projeto inicial.

Confira o código de linha de base no qual o tutorial se baseia:

1. Confira a ramificação `tutorial/pages-templates-start` do [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
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

É sempre possível exibir o código concluído no [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) ou conferir o código localmente, alternando-se para a ramificação `tutorial/pages-templates-solution`.

## Objetivo

1. Inspecionar um design de página criado no Adobe XD e mapeá-lo para componentes principais.
1. Entender os detalhes dos modelos editáveis e como as políticas podem ser usadas para aplicar um controle granular do conteúdo da página.
1. Aprender sobre como os modelos e as páginas são vinculados

## O que você criará {#what-build}

Nesta parte do tutorial, você criará um novo modelo de página de artigo que pode ser usado para criar páginas de artigo e está alinhado a uma estrutura comum. O modelo de página de artigo é baseado em designs e em um kit de interface produzido no Adobe XD. Este capítulo foca-se apenas na criação da estrutura ou esqueleto do modelo. Nenhum estilo é implementado, mas o modelo e as páginas funcionam.

![Design da página de artigo e versão não estilizada](assets/pages-templates/what-you-will-build.png)

## Planejamento da IU com o Adobe XD {#adobexd}

Normalmente, o planejamento de um novo site começa com simulações e designs estáticos. O [Adobe XD](https://helpx.adobe.com/br/support/xd.html) é uma ferramenta de design que cria uma experiência do usuário. Em seguida, vamos inspecionar um kit da IU e modelos para ajudar a planejar a estrutura do modelo de página de artigo.

>[!VIDEO](https://video.tv.adobe.com/v/30214?quality=12&learn=on)

**Baixe o [arquivo de design de artigo da WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> Um kit da IU dos [Componentes principais do AEM também está disponível](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd?lang=pt-BR) como ponto de partida para projetos personalizados.

## Criar o modelo de página de artigo

Ao criar uma página, é necessário selecionar um modelo, que serve de base para a criação da página. O modelo define a estrutura da página resultante, o conteúdo inicial e os componentes permitidos.

Há três áreas principais nos [Modelos editáveis](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=pt-BR):

1. **Estrutura**: define os componentes que fazem parte do modelo. Não podem ser editados por criadores de conteúdo.
1. **Conteúdo inicial**: define os componentes com os quais o modelo começa; podem ser editados e/ou excluídos por criadores de conteúdo
1. **Políticas**: definem as configurações de como os componentes se comportam e quais opções estão disponíveis para os criadores.

Em seguida, crie um modelo no AEM que corresponda à estrutura das simulações. Isso ocorre em uma instância local do AEM. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/330991?quality=12&learn=on)

Principais etapas do vídeo acima:

### Configurações da estrutura

1. Crie um modelo com o **Tipo de modelo de página** chamado **Página de artigo**.
1. Alterne para o modo **Estrutura**.
1. Adicione um componente de **Fragmento de experiência** para agir como **Cabeçalho** na parte superior do modelo.
   * Configure o componente para apontar para `/content/experience-fragments/wknd/us/en/site/header/master`.
   * Defina a política como **Cabeçalho da página** e verifique se o **Elemento padrão** está definido como `header`. O elemento `header` será direcionado com o CSS no próximo capítulo.
1. Adicione um componente de **Fragmento de experiência** para atuar como **Rodapé** na parte inferior do modelo.
   * Configure o componente para apontar para `/content/experience-fragments/wknd/us/en/site/footer/master`.
   * Defina a política como **Rodapé da página** e verifique se o **Elemento padrão** está definido como `footer`. O elemento `footer` será direcionado com o CSS no próximo capítulo.
1. Bloqueie o container **principal** que foi incluído quando o modelo foi criado inicialmente.
   * Defina a política como **Página principal** e verifique se o **Elemento padrão** está definido como `main`. O elemento `main` será direcionado com o CSS no próximo capítulo.
1. Adicione um componente de **Imagem** ao **container principal**.
   * Desbloqueie o componente de **Imagem**.
1. Adicione um componente de **Navegação estrutural** abaixo do componente de **Imagem** no container principal.
   * Crie uma política para o componente de **Navegação estrutural** chamada **Página de artigo - navegação estrutural**. Defina o **Nível inicial da navegação** como **4**.
1. Adicione um componente de **Container** abaixo do componente de **Navegação estrutural** e dentro do container **principal**. Ele atuará como **Container de conteúdo** do modelo.
   * Desbloqueie o container de **Conteúdo**.
   * Defina a política como **Conteúdo da página**.
1. Adicione outro componente de **Container** abaixo do **Container de conteúdo**. Ele atuará como container do **Painel lateral** do modelo.
   * Desbloqueie o container do **Painel lateral**.
   * Crie uma política chamada **Página de artigo - painel lateral**.
   * Configure os **Componentes permitidos** em **Projeto de sites da WKND: conteúdo** para incluir: **Botão**, **Download**, **Imagem**, **Lista**, **Separador**, **Compartilhamento em redes sociais**, **Texto** e **Título**.
1. Atualize a política do container de raiz da página. Esse é o contêiner mais externo do modelo. Defina a política como **Raiz da página**.
   * Em **Configurações do container**, defina o **Layout** como **Grade responsiva**.
1. Ative modo de layout do **Container de conteúdo**. Arraste a alça da direita para a esquerda e reduza o container para oito colunas de largura.
1. Ative o modo de layout para o **Container do painel lateral**. Arraste a alça da direita para a esquerda e reduza o container para quatro colunas de largura. Em seguida, arraste a alça esquerda da esquerda para a direita por uma coluna para reduzir o container para três colunas de largura e deixe um intervalo de uma coluna entre o **Container de conteúdo**.
1. Abra o emulador móvel e alterne para um ponto de quebra móvel. Ative o modo de layout novamente e aumente o **Container de conteúdo** e o **Container do painel lateral** para ocuparem a largura total da página. Isso empilha os containers verticalmente no ponto de quebra móvel.
1. Atualize a política do componente **Texto** no **Container de conteúdo**.
   * Defina a política como **Texto do conteúdo**.
   * Em **Plug-ins** > **Estilos de parágrafo**, marque a opção **Habilitar estilos de parágrafo** e verifique se o **Bloco de aspas** está habilitado.

### Configurações do conteúdo inicial

1. Alterne para o modo **Conteúdo inicial**.
1. Adicione um componente de **Título** ao **Container de conteúdo**. Ele atuará como título do artigo. Se deixado em branco, ele exibirá automaticamente o título da página atual.
1. Adicione um segundo componente de **Título** abaixo do primeiro componente de título.
   * Configure o componente com o texto: “Pelo autor”. Esse é um espaço reservado para texto.
   * Defina o tipo como `H4`.
1. Adicione um componente de **Texto** abaixo do componente de título **Pelo autor**.
1. Adicione um componente de **Título** ao **Container do painel lateral**.
   * Configure o componente com o texto: “Compartilhar esta história”.
   * Defina o tipo como `H5`.
1. Adicione um componente de **Compartilhamento em redes sociais** abaixo do componente de título **Compartilhar esta história**.
1. Adicione um componente de **Separador** abaixo do componente de **Compartilhar em redes sociais**.
1. Adicione um componente de **Download** abaixo do componente de **Separador**.
1. Adicione um componente de **Lista** abaixo do componente de **Download**.
1. Atualize as **Propriedades da página inicial** do modelo.
   * Em **Redes sociais** > **Compartilhar em Redes Sociais**, marque **Facebook** e **Pinterest**.

### Habilitar o modelo e adicionar uma miniatura

1. Visualize o modelo no console “Modelo”, navegando até [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **Habilite** o modelo de página de artigo.
1. Edite as propriedades do modelo de página de artigo e carregue a seguinte miniatura para identificar rapidamente as páginas criadas com o modelo de página de artigo:

   ![Miniatura do modelo de página de artigo](assets/pages-templates/article-page-template-thumbnail.png)

## Atualizar o cabeçalho e o rodapé com fragmentos de experiência {#experience-fragments}

Uma prática comum ao criar um conteúdo global, como cabeçalho ou rodapé, é usar um [Fragmento de experiência](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=pt-BR). Os fragmentos de experiência permitem que os usuários combinem vários componentes para criar um componente unificado que pode ser referenciado. Os fragmentos de experiência têm a vantagem de permitir o gerenciamento de vários sites e a [localização](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=pt-br).

O arquétipo de projeto do AEM gerou um cabeçalho e um rodapé. Em seguida, atualize os fragmentos de experiência para corresponderem às simulações. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/330992?quality=12&learn=on)

Principais etapas do vídeo acima:

1. Baixe o pacote de conteúdo de exemplo **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**.
1. Carregue e instale o pacote de conteúdo com o gerenciador de pacotes em [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Atualize o modelo de variação da web, que é o modelo usado com fragmentos de experiência, em [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)
   * Atualize a política do componente de **Container** no modelo.
   * Defina a política como **Raiz XF**.
   * Os **Componentes permitidos** selecionam o grupo de componentes de **Projeto de sites da WKND: estrutura** para incluir componentes de **Navegação em idiomas**, **Navegação** e **Pesquisa rápida**.

### Atualizar fragmento de experiência de cabeçalho

1. Abra o fragmento de experiência que renderiza o cabeçalho em [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. Configure o **Container** de raiz do fragmento. É o **Container** mais externo.
   * Defina o **Layout** como **Grade responsiva**.
1. Adicione o **Logotipo escuro da WKND** como imagem à parte superior do **Container**. O logotipo foi incluído no pacote instalado em uma etapa anterior.
   * Modifique o layout do **Logotipo escuro da WKND** para ter **duas** colunas de largura. Arraste as alças da direita para a esquerda.
   * Configure o logotipo com o **Texto alternativo** “Logotipo da WKND”.
   * Configure o logotipo para **Vincular** `/content/wknd/us/en` à página inicial.
1. Configure o componente de **Navegação** que já foi colocado na página.
   * Defina os **Níveis exclusão de raiz** como **1**.
   * Defina a **Profundidade da estrutura de navegação** como **1**.
   * Modifique o layout do componente de **Navegação** para ter **oito** colunas de largura. Arraste as alças da direita para a esquerda.
1. Remova o componente de **Navegação em idiomas**.
1. Modifique o layout do componente de **Pesquisa** para ter **duas** colunas de largura. Arraste as alças da direita para a esquerda. Todos os componentes agora devem estar alinhados horizontalmente na mesma linha.

### Atualizar o fragmento de experiência de rodapé

1. Abra o fFragmento de experiência que renderiza o rodapé em [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. Configure o **Container** de raiz do fragmento. É o **Container** mais externo.
   * Defina o **Layout** como **Grade responsiva**.
1. Adicione o **Logotipo claro da WKND** como imagem à parte superior do **Container**. O logotipo foi incluído no pacote instalado em uma etapa anterior.
   * Modifique o layout do **Logotipo claro da WKND** para ter **duas** colunas de largura. Arraste as alças da direita para a esquerda.
   * Configure o logotipo com o **Texto alternativo** “Logotipo claro da WKND”.
   * Configure o logotipo para **Vincular** `/content/wknd/us/en` à página inicial.
1. Adicione um componente de **Navegação** abaixo do logotipo. Configure o componente de **Navegação**:
   * Defina os **Níveis de exclusão de raiz** como **1**.
   * Desmarque **Coletar todas as páginas secundárias**.
   * Defina a **Profundidade da estrutura de navegação** como **1**.
   * Modifique o layout do componente de **Navegação** para ter **oito** colunas de largura. Arraste as alças da direita para a esquerda.

## Criar uma página de artigo

Em seguida, crie uma página com o modelo de página de artigo. Crie o conteúdo da página para corresponder às simulações do site. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/330993?quality=12&learn=on)

Principais etapas do vídeo acima:

1. Navegue até o console “Sites” em [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. Crie uma página abaixo de **WKND** > **BR** > **PT** > **Revista**.
   * Escolha o modelo de **Página de artigo**.
   * Em **Propriedades**, defina o **Título** como “Ultimate Guide to LA Skateparks”
   * Defina o **Nome** como “guide-la-skateparks”.
1. Substitua o título **Pelo autor** pelo texto “Por Stacey Roswells”.
1. Atualize o componente de **Texto** para incluir um parágrafo para preencher o artigo. Você pode usar o seguinte arquivo de texto como fonte: [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. Adicione outro componente de **Texto**.
   * Atualize o componente para incluir a citação: “Não há lugar melhor para andar de skate que Los Angeles”.
   * Edite no editor de rich text no modo de tela cheia e modifique a citação acima para usar o elemento de **Bloco de aspas**.
1. Continue preenchendo o corpo do artigo conforme as simulações.
1. Configure o componente de **Download** para usar uma versão em PDF do artigo.
   * Em **Download** > **Propriedades**, clique na caixa de seleção para **Obter o título do ativo do DAM**.
   * Defina a **Descrição** como: “Leia a história completa”.
   * Defina o **Texto de ação** como: “Baixar o PDF”.
1. Configure o componente de **Lista**.
   * Em **Configurações da lista** > **Criar lista com**, selecione **Páginas secundárias**.
   * Defina a **Página primária** como `/content/wknd/us/en/magazine`.
   * Em **Configurações dos itens**, marque **Vincular itens** e **Mostrar data**.

## Inspecionar a estrutura de nós {#node-structure}

Neste ponto, a página do artigo claramente não está estilizada. No entanto, a estrutura básica está implementada. Em seguida, inspecione a estrutura de nós da página de artigo para entender melhor a função do modelo, da página e dos componentes.

Use a ferramenta CRXDE-Lite em uma instância do AEM local para exibir a estrutura de nós subjacente.

1. Abra o [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) e use a navegação em árvore para navegar até `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Clique no nó `jcr:content` abaixo da página `la-skateparks` e visualize as propriedades:

   ![Propriedades do conteúdo JCR](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Observe o valor de `cq:template`, que aponta para `/conf/wknd/settings/wcm/templates/article-page`, o modelo de página de artigo criado anteriormente.

   Observe, também, o valor de `sling:resourceType`, que aponta para `wknd/components/page`. É o componente de página criado pelo arquétipo de projeto do AEM, sendo responsável pela renderização da página com base no modelo.

1. Expanda o nó `jcr:content` abaixo de `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` e visualize a hierarquia de nós:

   ![Conteúdo JCR sobre skateparks de Los Angeles](assets/pages-templates/page-jcr-structure.png)

   Você deve ser capaz de mapear livremente cada um dos nós para os componentes criados. Confira se você é capaz de identificar os diferentes containers de layout usados ao inspecionar os nós com o prefixo `container`.

1. Em seguida, inspecione o componente da página em `/apps/wknd/components/page`. Visualize as propriedades do componente no CRXDE Lite:

   ![Propriedades do componente de página](assets/pages-templates/page-component-properties.png)

   Há apenas dois scripts HTL, `customfooterlibs.html` e `customheaderlibs.html`, abaixo do componente de página. *Como esse componente renderiza a página?*

   A propriedade `sling:resourceSuperType` aponta para `core/wcm/components/page/v2/page`. Essa propriedade permite que o componente de página da WKND herde **toda** a funcionalidade do componente de página dos componentes principais. Este é o primeiro exemplo de algo chamado de [Padrão de componente de proxy](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=pt-BR#ProxyComponentPattern). Mais informações podem ser encontradas [aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=pt-BR).

1. Inspecione outro componente dentro dos componentes da WKND, o componente `Breadcrumb` de: `/apps/wknd/components/breadcrumb`. Observe que a mesma propriedade `sling:resourceSuperType` pode ser encontrada, mas desta vez ela aponta para `core/wcm/components/breadcrumb/v2/breadcrumb`. Este é outro exemplo de como usar o padrão de componente de proxy para incluir um componente principal. Na verdade, todos os componentes na base de código da WKND são proxies dos componentes principais do AEM (exceto o componente de demonstração personalizada “Olá, mundo”). É uma prática recomendada reutilizar o máximo possível da funcionalidade dos componentes principais *antes* de gravar o código personalizado.

1. Em seguida, inspecione a página dos componentes principais em `/libs/core/wcm/components/page/v2/page` com o CRXDE Lite:

   >[!NOTE]
   >
   > No AEM 6.5/6.4, os componentes principais estão localizados em `/apps/core/wcm/components`. No AEM as a Cloud Service, os componentes principais estão localizados em `/libs` e são atualizados automaticamente.

   ![Página dos componentes principais](assets/pages-templates/core-page-component-properties.png)

   Observe que muitos arquivos de script estão incluídos abaixo dessa página. A página dos componentes principais contém várias funcionalidades. Elas se dividem em vários scripts para facilitar a manutenção e a leitura. Você pode rastrear a inclusão dos scripts HTL, abrindo o `page.html` e procurando por `data-sly-include`:

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

   O outro motivo para dividir o HTL em vários scripts é permitir que os componentes de proxy substituam scripts individuais para implementar uma lógica de negócios personalizada. Os scripts HTL `customfooterlibs.html` e `customheaderlibs.html` são criados para que a finalidade explícita seja substituída pela implementação de projetos.

   Você pode saber mais sobre como o modelo editável influencia a renderização da página de conteúdo [neste artigo](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=pt-BR).

1. Inspecione outro componente principal, como a navegação estrutural, em `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Visualize o script `breadcrumb.html` para entender como a marcação do componente de navegação estrutural é gerada.

## Salvar configurações no controle de origem {#configuration-persistence}

Geralmente, principalmente no início de um projeto do AEM, é útil manter as configurações, como modelos e políticas de conteúdo relacionadas, no controle de origem. Isso garante que todos os desenvolvedores trabalhem com o mesmo conjunto de conteúdo e configurações, garantindo mais consistência entre os ambientes. Quando um projeto atinge um determinado nível de maturidade, a prática de gerenciar modelos pode ser transferida para um grupo especial de usuários avançados.


Por enquanto, os modelos são tratados como outras partes do código e sincronizam o **Modelo de página de artigo** como parte do projeto.
Até agora, o código é enviado por push do projeto do AEM para uma instância local do AEM. O **Modelo de página de artigo** foi criado diretamente em uma instância local do AEM; portanto, é necessário **importar** o modelo para o projeto do AEM. O módulo **ui.content** está incluído no projeto do AEM para essa finalidade específica.

As próximas etapas são realizadas no VSCode IDE, usando-se o plug-in [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&ssr=false#overview). Porém, pode-se usar qualquer IDE que você tenha configurado para **importar** ou importar o conteúdo de uma instância local do AEM.

1. No VSCode, abra o projeto `aem-guides-wknd`.

1. Expanda o módulo **ui.content** no gerenciador de projetos. Expanda a pasta `src` e navegue até `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Clique com o botão direito do mouse] na pasta `templates` e selecione **Importar do servidor do AEM**:

   ![Modelo de importação do VSCode](assets/pages-templates/vscode-import-templates.png)

   O `article-page` deve ser importado, e os modelos `page-content`, `xf-web-variation` também devem ser atualizados.

   ![Modelos atualizados](assets/pages-templates/updated-templates.png)

1. Repita as etapas para importar o conteúdo, mas selecione a pasta de **políticas** de `/conf/wknd/settings/wcm/policies`.

   ![Políticas de importação do VSCode](assets/pages-templates/policies-article-page-template.png)

1. Inspecione o arquivo `filter.xml` de `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   O arquivo `filter.xml` é responsável por identificar os caminhos dos nós instalados com o pacote. Observe o `mode="merge"` em cada filtro, que indica que o conteúdo existente não deve ser modificado, e somente o novo conteúdo será adicionado. Como os criadores de conteúdo podem estar atualizando esses caminhos, é importante que uma implantação do código **não** substitua o conteúdo. Consulte a [documentação do FileVault](https://jackrabbit.apache.org/filevault/filter.html) para mais detalhes sobre como trabalhar com elementos de filtro.

   Compare `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` para entender os diferentes nós gerenciados por cada módulo.

   >[!WARNING]
   >
   > Para garantir implantações consistentes para o site de referência da WKND, algumas ramificações do projeto são configuradas de modo que `ui.content` substitua todas as alterações no JCR. Isso é intencional, ou seja, para ramificações de solução, já que o código/estilos são criados para políticas específicas.

## Parabéns! {#congratulations}

Parabéns, você criou um modelo e uma página com o Adobe Experience Manager Sites.

### Próximas etapas {#next-steps}

Neste ponto, a página do artigo claramente não está estilizada. Siga o tutorial de [Bibliotecas do lado do cliente e fluxo de trabalho de front-end](client-side-libraries.md) para conhecer as práticas recomendadas para incluir o CSS e o JavaScript a fim de aplicar estilos globais ao site e integrar uma compilação de front-end dedicada.

Veja o código concluído no [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente na ramificação do Git `tutorial/pages-templates-solution`.

1. Clone o repositório [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Confira a ramificação `tutorial/pages-templates-solution`.
