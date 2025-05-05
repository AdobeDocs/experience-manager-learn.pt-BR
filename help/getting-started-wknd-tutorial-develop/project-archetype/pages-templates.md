---
title: Introdução ao AEM Sites - Páginas e modelos
description: Saiba mais sobre a relação entre um componente de página base e modelos editáveis. Entenda como os Componentes principais são encaminhados por proxy para o projeto. Saiba mais sobre as configurações avançadas de política de modelos editáveis para criar um modelo de página de artigo bem estruturado com base em um modelo do Adobe XD.
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
workflow-type: tm+mt
source-wordcount: '2855'
ht-degree: 0%

---

# Páginas e modelos {#pages-and-template}

{{edge-delivery-services-and-page-editor}}

Neste capítulo, vamos explorar a relação entre um componente de página base e modelos editáveis. Saiba como criar um modelo de Artigo sem estilo com base em alguns modelos do [Adobe XD](https://helpx.adobe.com/br/support/xd.html). No processo de criação do modelo, os Componentes principais e as configurações de política avançadas dos Modelos editáveis são abordados.

## Pré-requisitos {#prerequisites}

Revise as ferramentas e instruções necessárias para configurar um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Projeto inicial

>[!NOTE]
>
> Se você concluiu com êxito o capítulo anterior, é possível reutilizar o projeto e ignorar as etapas para verificar o projeto inicial.

Verifique o código de linha base no qual o tutorial se baseia:

1. Confira a ramificação `tutorial/pages-templates-start` do [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
   ```

1. Implante a base de código em uma instância de AEM local usando suas habilidades de Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Se estiver usando AEM 6.5 ou 6.4, anexe o perfil `classic` a qualquer comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) ou conferir o código localmente alternando para a ramificação `tutorial/pages-templates-solution`.

## Objetivo

1. O Inspect cria um design de página criado no Adobe XD e o mapeia para os Componentes principais.
1. Entenda os detalhes de Modelos editáveis e como as políticas podem ser usadas para impor o controle granular do conteúdo da página.
1. Saiba como os Modelos e as Páginas são vinculados

## O que você vai criar {#what-build}

Nesta parte do tutorial, você cria um novo Modelo de página de artigo que pode ser usado para criar páginas de artigo e se alinha a uma estrutura comum. O Modelo de página de artigo é baseado em designs e em um kit de interface do usuário produzido no Adobe XD. Este capítulo foca apenas na criação da estrutura ou esqueleto do template. Nenhum estilo é implementado, mas o modelo e as páginas são funcionais.

![Design da Página de Artigo e versão sem estilo](assets/pages-templates/what-you-will-build.png)

## Planejamento da interface do usuário com o Adobe XD {#adobexd}

Normalmente, o planejamento de um novo site começa com modelos e designs estáticos. O [Adobe XD](https://helpx.adobe.com/br/support/xd.html) é uma ferramenta de design que cria uma experiência de usuário. Em seguida, vamos inspecionar um Kit de interface do usuário e modelos para ajudar a planejar a estrutura do Modelo de página de artigo.

>[!VIDEO](https://video.tv.adobe.com/v/36143?quality=12&learn=on&captions=por_br)

**Baixe o [Arquivo de Design do Artigo WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> Um Kit de Interface do Usuário dos [Componentes Principais do AEM também está disponível](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd?lang=pt-BR) como ponto de partida para projetos personalizados.

## Criar o modelo da página de artigo

Ao criar uma página, você deve selecionar um modelo, que é usado como base para a criação da página. O modelo define a estrutura da página resultante, o conteúdo inicial e os componentes permitidos.

Há três áreas principais de [Modelos Editáveis](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=pt-BR):

1. **Estrutura** - define os componentes que fazem parte do modelo. Eles não podem ser editados por autores de conteúdo.
1. **Conteúdo inicial** - define os componentes com os quais o modelo começa; eles podem ser editados e/ou excluídos por autores de conteúdo
1. **Políticas** - define as configurações sobre como os componentes se comportam e quais opções os autores têm.

Em seguida, crie um modelo no AEM que corresponda à estrutura dos modelos. Isso ocorre em uma instância local de AEM. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/36142?quality=12&learn=on&captions=por_br)

Etapas de alto nível para o vídeo acima:

### Configurações de estrutura

1. Crie um modelo usando o **Tipo de Modelo de Página**, chamado **Página de Artigo**.
1. Mudar para o modo **Estrutura**.
1. Adicione um componente **Fragmento de experiência** para agir como o **Cabeçalho** na parte superior do modelo.
   * Configure o componente para apontar para `/content/experience-fragments/wknd/us/en/site/header/master`.
   * Defina a política como **Cabeçalho da página** e verifique se o **Elemento Padrão** está definido como `header`. O elemento `header` é direcionado com CSS no próximo capítulo.
1. Adicione um componente **Fragmento de experiência** para atuar como **Rodapé** na parte inferior do modelo.
   * Configure o componente para apontar para `/content/experience-fragments/wknd/us/en/site/footer/master`.
   * Defina a política como **Rodapé da página** e verifique se o **Elemento Padrão** está definido como `footer`. O elemento `footer` é direcionado com CSS no próximo capítulo.
1. Bloqueie o contêiner **principal** que foi incluído quando o modelo foi criado inicialmente.
   * Defina a política como **Página principal** e verifique se o **Elemento padrão** está definido como `main`. O elemento `main` é direcionado com CSS no próximo capítulo.
1. Adicione um componente **Imagem** ao contêiner **principal**.
   * Desbloquear o componente **Imagem**.
1. Adicione um componente de **Navegação estrutural** abaixo do componente **Imagem** no contêiner principal.
   * Crie uma política para o componente **Navegação estrutural** chamado **Página de Artigo - Navegação estrutural**. Defina o **Nível de Início da Navegação** como **4**.
1. Adicione um componente **Contêiner** abaixo do componente **Navegação estrutural** e dentro do contêiner **principal**. Isto atua como o **Contêiner de conteúdo** do modelo.
   * Desbloqueie o contêiner **Conteúdo**.
   * Defina a política como **Conteúdo da Página**.
1. Adicione outro componente **Contêiner** abaixo do **Contêiner de conteúdo**. Funciona como o contêiner **Painel lateral** do modelo.
   * Desbloqueie o contêiner **Painel lateral**.
   * Crie uma política chamada **Página de Artigo - Painel Lateral**.
   * Configure os **Componentes Permitidos** em **Projeto de Sites WKND - Conteúdo** para incluir: **Botão**, **Download**, **Imagem**, **Lista**, **Separador**, **Compartilhamento em Redes Sociais**, **Texto** e **Título**.
1. Atualize a política do container Raiz da página. Este é o contêiner mais externo do modelo. Defina a política como **Raiz da página**.
   * Em **Configurações do Contêiner**, defina o **Layout** como **Grade Responsiva**.
1. Ativar Modo de Layout para o **Contêiner de conteúdo**. Arraste a alça da direita para a esquerda e reduza o contêiner para oito colunas de largura.
1. Ativar modo de layout para o **contêiner do painel lateral**. Arraste a alça da direita para a esquerda e reduza o contêiner para ter quatro colunas de largura. Em seguida, arraste a alça esquerda da esquerda para a direita uma coluna para tornar o contêiner de 3 colunas largo e deixe um intervalo de 1 coluna entre o **contêiner de conteúdo**.
1. Abra o emulador móvel e alterne para um ponto de interrupção móvel. Ative o modo de layout novamente e torne o **Contêiner de conteúdo** e o **Contêiner de painel lateral** a largura total da página. Isso empilha os contêineres verticalmente no ponto de interrupção móvel.
1. Atualize a política do componente **Texto** no **Contêiner de conteúdo**.
   * Defina a política como **Texto de conteúdo**.
   * Em **Plug-ins** > **Estilos de parágrafo**, marque a opção **Habilitar estilos de parágrafo** e verifique se o **Bloco de aspas** está habilitado.

### Configurações do conteúdo inicial

1. Alternar para o modo **Conteúdo Inicial**.
1. Adicione um componente **Título** ao **Contêiner de conteúdo**. Atua como o título do Artigo. Quando deixado em branco, ele exibe automaticamente o Título da página atual.
1. Adicione um segundo componente **Title** abaixo do primeiro componente de Título.
   * Configure o componente com o texto: &quot;Por autor&quot;. Este é um espaço reservado para texto.
   * Defina o tipo como `H4`.
1. Adicione um componente **Texto** abaixo do componente de Título **Por Autor**.
1. Adicione um componente **Título** ao **Contêiner do Painel Lateral**.
   * Configure o componente com o texto: &quot;Compartilhar esta história&quot;.
   * Defina o tipo como `H5`.
1. Adicione um componente **Compartilhamento em redes sociais** abaixo do componente de Título **Compartilhar esta história**.
1. Adicione um componente **Separador** abaixo do componente **Compartilhamento em Redes Sociais**.
1. Adicione um componente **Download** abaixo do componente **Separador**.
1. Adicione um componente **Lista** abaixo do componente **Download**.
1. Atualize as **Propriedades da Página Inicial** do modelo.
   * Em **Redes sociais** > **Compartilhamento em Redes Sociais**, marque **Facebook** e **Pinterest**

### Ativar o modelo e adicionar uma miniatura

1. Exiba o modelo no console Modelo navegando até [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **Habilitar** o modelo de Página de Artigo.
1. Edite as propriedades do modelo Página de artigo e faça upload da seguinte miniatura para identificar rapidamente as páginas criadas usando o modelo Página de artigo:

   ![Miniatura do modelo de página de artigo](assets/pages-templates/article-page-template-thumbnail.png)

## Atualizar o cabeçalho e o rodapé com fragmentos de experiência {#experience-fragments}

Uma prática comum ao criar conteúdo global, como cabeçalho ou rodapé, é usar um [Fragmento de experiência](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=pt-BR). Fragmentos de experiência, permitem que os usuários combinem vários componentes para criar um único componente que pode ser referenciado. Os Fragmentos de experiência têm a vantagem de oferecer suporte ao gerenciamento de vários sites e à [localização](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=pt-BR).

O Arquétipo de projeto AEM gerou um Cabeçalho e Rodapé. Em seguida, atualize os Fragmentos de experiência para corresponder aos modelos. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/3447494?quality=12&learn=on&captions=por_br)

Etapas de alto nível para o vídeo acima:

1. Baixe o pacote de conteúdo de exemplo **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**.
1. Carregue e instale o pacote de conteúdo usando o Gerenciador de Pacotes em [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Atualize o modelo de Variação da Web, que é o modelo usado para Fragmentos de experiência em [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)
   * Atualize a política para o componente **Contêiner** no modelo.
   * Defina a política como **Raiz XF**.
   * Em, os **Componentes Permitidos** selecionam o grupo de componentes **Projeto de Sites WKND - Estrutura** para incluir componentes de **Navegação de Idioma**, **Navegação** e **Pesquisa Rápida**.

### Atualizar cabeçalho do fragmento de experiência

1. Abra o Fragmento de experiência que renderiza o Cabeçalho em [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. Configure o **Contêiner** raiz do fragmento. Este é o mais externo **Contêiner**.
   * Definir o **Layout** como **Grade responsiva**
1. Adicione o **Logotipo Escuro WKND** como uma imagem à parte superior do **Contêiner**. O logotipo foi incluído no pacote instalado em uma etapa anterior.
   * Modifique o layout do **Logotipo Escuro do WKND** para ter **duas** colunas de largura. Arraste as alças da direita para a esquerda.
   * Configure o logotipo com **Texto alternativo** do &quot;Logotipo WKND&quot;.
   * Configure o logotipo para **Link** para `/content/wknd/us/en` a Home page.
1. Configure o componente de **Navegação** que já foi colocado na página.
   * Defina os **Excluir Níveis de Raiz** para **1**.
   * Defina a **Profundidade da Estrutura de Navegação** como **1**.
   * Modifique o layout do componente **Navegação** para ter **oito** colunas de largura. Arraste as alças da direita para a esquerda.
1. Remova o componente **Navegação por idiomas**.
1. Modifique o layout do componente **Pesquisa** para ter **duas** colunas de largura. Arraste as alças da direita para a esquerda. Todos os componentes agora devem ser alinhados horizontalmente em uma única linha.

### Atualizar fragmento de experiência do rodapé

1. Abra o Fragmento de experiência que renderiza o Rodapé em [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. Configure o **Contêiner** raiz do fragmento. Este é o mais externo **Contêiner**.
   * Definir o **Layout** como **Grade responsiva**
1. Adicione o **Logotipo de Luz WKND** como uma imagem à parte superior do **Contêiner**. O logotipo foi incluído no pacote instalado em uma etapa anterior.
   * Modifique o layout do **Logotipo Leve do WKND** para ter **duas** colunas de largura. Arraste as alças da direita para a esquerda.
   * Configure o logotipo com **Texto alternativo** de &quot;Luz de logotipo WKND&quot;.
   * Configure o logotipo para **Link** para `/content/wknd/us/en` a Home page.
1. Adicione um componente **Navegação** abaixo do logotipo. Configurar o componente de **Navegação**:
   * Defina os **Excluir Níveis de Raiz** para **1**.
   * Desmarcar **Coletar todas as páginas secundárias**.
   * Defina a **Profundidade da Estrutura de Navegação** como **1**.
   * Modifique o layout do componente **Navegação** para ter **oito** colunas de largura. Arraste as alças da direita para a esquerda.

## Criar uma página de artigo

Em seguida, crie uma página usando o modelo Página de artigo. Crie o conteúdo da página para corresponder aos modelos de site. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/3446447?quality=12&learn=on&captions=por_br)

Etapas de alto nível para o vídeo acima:

1. Navegue até o console Sites em [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. Crie uma página abaixo de **WKND** > **US** > **EN** > **Magazine**.
   * Escolha o modelo da **Página de Artigo**.
   * Em **Propriedades**, defina o **Título** como &quot;Guia Final para Skateparks de Los Angeles&quot;
   * Defina o **Nome** como &quot;guide-la-skateparks&quot;
1. Substituir o Título **Por Autor** pelo texto &quot;Por Stacey Roswells&quot;.
1. Atualize o componente **Texto** para incluir um parágrafo para preencher o artigo. Você pode usar o seguinte arquivo de texto como cópia: [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. Adicionar outro componente **Texto**.
   * Atualize o componente para incluir a citação: &quot;Não há lugar melhor para destruir do que Los Angeles&quot;.
   * Edite o Editor de Rich Text no modo de tela cheia e modifique a citação acima para usar o elemento **Bloco de citações**.
1. Continue preenchendo o corpo do artigo para corresponder aos modelos.
1. Configure o componente **Download** para usar uma versão PDF do artigo.
   * Em **Download** > **Propriedades**, clique na caixa de seleção para **Obter o título do ativo DAM**.
   * Defina a **Descrição** como: &quot;Obter a História Completa&quot;.
   * Defina o **Texto de Ação** como: &quot;Baixar PDF&quot;.
1. Configurar o componente **Lista**.
   * Em **Configurações da Lista** > **Criar Lista Usando**, selecione **Páginas Secundárias**.
   * Definir a **Página Pai** como `/content/wknd/us/en/magazine`.
   * Em, as **Configurações de Item** verificam **Vincular Itens** e verificam **Mostrar data**.

## Inspect a estrutura do nó {#node-structure}

Nesse ponto, a página do artigo está claramente sem estilo. No entanto, a estrutura básica está em vigor. Em seguida, inspecione a estrutura do nó da página de artigo para entender melhor a função do modelo, da página e dos componentes.

Use a ferramenta CRXDE-Lite em uma instância de AEM local para visualizar a estrutura do nó subjacente.

1. Abra o [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) e use a navegação em árvore para navegar até `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Clique no nó `jcr:content` abaixo da página `la-skateparks` e exiba as propriedades:

   ![Propriedades de conteúdo JCR](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Observe o valor de `cq:template`, que aponta para `/conf/wknd/settings/wcm/templates/article-page`, o Modelo de página de artigo criado anteriormente.

   Observe também o valor de `sling:resourceType`, que aponta para `wknd/components/page`. É o componente Página criado pelo arquétipo de projeto AEM e é responsável pela renderização da página com base no modelo.

1. Expanda o nó `jcr:content` abaixo de `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` e exiba a hierarquia do nó:

   ![Skateparks de Los Angeles de Conteúdo JCR](assets/pages-templates/page-jcr-structure.png)

   Você deve ser capaz de mapear livremente cada um dos nós para componentes que foram criados. Veja se você pode identificar os diferentes Contêineres de Layout usados ao inspecionar os nós com o prefixo `container`.

1. Em seguida, inspecione o componente da página em `/apps/wknd/components/page`. Veja as propriedades do componente no CRXDE Lite:

   ![Propriedades do componente de Página](assets/pages-templates/page-component-properties.png)

   Há apenas dois scripts HTL, `customfooterlibs.html` e `customheaderlibs.html` abaixo do componente página. *Como este componente renderiza a página?*

   A propriedade `sling:resourceSuperType` aponta para `core/wcm/components/page/v2/page`. Esta propriedade permite que o componente de página da WKND herde **all** a funcionalidade do componente de página, dos Componentes principais. Este é o primeiro exemplo de algo chamado de [Padrão de Componente Proxy](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=pt-BR#ProxyComponentPattern). Mais informações podem ser encontradas [aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=pt-BR).

1. Inspect é outro componente dentro dos componentes WKND, o componente `Breadcrumb` de: `/apps/wknd/components/breadcrumb`. Observe que a mesma propriedade `sling:resourceSuperType` pode ser encontrada, mas desta vez ela aponta para `core/wcm/components/breadcrumb/v2/breadcrumb`. Este é outro exemplo de como usar o padrão do componente Proxy para incluir um Componente principal. Na verdade, todos os componentes na base de código WKND são proxies de componentes principais AEM (exceto para o componente de demonstração personalizada HelloWorld ). É uma prática recomendada reutilizar o máximo possível da funcionalidade dos Componentes principais *antes* de gravar o código personalizado.

1. Em seguida, inspecione a página dos Componentes principais em `/libs/core/wcm/components/page/v2/page` usando o CRXDE Lite:

   >[!NOTE]
   >
   > No, AEM 6.5/6.4, os Componentes principais estão localizados em `/apps/core/wcm/components`. No, AEM as a Cloud Service, os Componentes principais estão localizados em `/libs` e são atualizados automaticamente.

   ![Página do Componente Principal](assets/pages-templates/core-page-component-properties.png)

   Observe que muitos arquivos de script estão incluídos abaixo dessa página. A página dos Componentes principais contém várias funcionalidades. Essa funcionalidade é dividida em vários scripts para facilitar a manutenção e a leitura. Você pode rastrear a inclusão dos scripts HTL abrindo o `page.html` e procurando por `data-sly-include`:

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

   O outro motivo para dividir o HTL em vários scripts é permitir que os componentes proxy substituam scripts individuais para implementar uma lógica de negócios personalizada. Os scripts HTL `customfooterlibs.html` e `customheaderlibs.html` são criados para que a finalidade explícita seja substituída pela implementação de projetos.

   Você pode saber mais sobre como o Modelo Editável influencia a renderização da página de conteúdo [lendo este artigo](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=pt-BR).

1. Inspect é outro Componente Principal, como a Navegação Estrutural em `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Visualize o script `breadcrumb.html` para entender como a marcação do componente de Navegação estrutural é gerada.

## Como salvar configurações no Source Control {#configuration-persistence}

Geralmente, especialmente no início de um projeto AEM, é valioso manter as configurações, como modelos e políticas de conteúdo relacionadas, no controle de origem. Isso garante que todos os desenvolvedores trabalhem com o mesmo conjunto de conteúdo e configurações e possa garantir consistência adicional entre os ambientes. Quando um projeto atinge um determinado nível de maturidade, a prática de gerenciar modelos pode ser transferida para um grupo especial de usuários avançados.


Por enquanto, os modelos são tratados como outras partes do código e sincronizam o **Modelo de página de artigo** como parte do projeto.
Até agora, o código é enviado do projeto AEM para uma instância local de AEM. O **Modelo de Página de Artigo** foi criado diretamente em uma instância local do AEM, portanto, é necessário **importar** o modelo para o projeto AEM. O módulo **ui.content** está incluído no projeto AEM para essa finalidade específica.

As próximas etapas são realizadas no VSCode IDE usando o plug-in [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview). Mas eles podem estar usando qualquer IDE que você tenha configurado para **importar** ou importar conteúdo de uma instância local do AEM.

1. No, o VSCode abre o projeto `aem-guides-wknd`.

1. Expanda o módulo **ui.content** no Gerenciador de projetos. Expanda a pasta `src` e navegue até `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Clique com o botão direito do mouse] na pasta `templates` e selecione **Importar do Servidor AEM**:

   ![Modelo de importação do VSCode](assets/pages-templates/vscode-import-templates.png)

   O `article-page` deve ser importado, e os modelos `page-content`, `xf-web-variation` também devem ser atualizados.

   ![Modelos atualizados](assets/pages-templates/updated-templates.png)

1. Repita as etapas para importar o conteúdo, mas selecione a pasta **políticas** de `/conf/wknd/settings/wcm/policies`.

   ![Políticas de importação do VSCode](assets/pages-templates/policies-article-page-template.png)

1. Inspect o arquivo `filter.xml` de `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   O arquivo `filter.xml` é responsável por identificar os caminhos dos nós instalados com o pacote. Observe o `mode="merge"` em cada filtro que indica que o conteúdo existente não deve ser modificado, somente o novo conteúdo será adicionado. Como os autores de conteúdo podem estar atualizando esses caminhos, é importante que uma implantação de código **não** substitua o conteúdo. Consulte a [documentação do FileVault](https://jackrabbit.apache.org/filevault/filter.html) para obter mais detalhes sobre como trabalhar com elementos de filtro.

   Compare `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` para entender os diferentes nós gerenciados por cada módulo.

   >[!WARNING]
   >
   > Para garantir implantações consistentes para o site de Referência WKND, algumas ramificações do projeto são configuradas de forma que `ui.content` substitua todas as alterações no JCR. Isso ocorre por design, ou seja, para Ramificações de solução, já que o código/estilos são criados para políticas específicas.

## Parabéns. {#congratulations}

Parabéns, você criou um modelo e uma página com o Adobe Experience Manager Sites.

### Próximas etapas {#next-steps}

Nesse ponto, a página do artigo está claramente sem estilo. Siga o tutorial de [Bibliotecas do lado do cliente e Fluxo de trabalho de front-end](client-side-libraries.md) para saber as práticas recomendadas para incluir CSS e JavaScript para aplicar estilos globais ao site e integrar uma compilação de front-end dedicada.

Visualize o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente em na ramificação Git `tutorial/pages-templates-solution`.

1. Clonar o repositório [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Confira a ramificação `tutorial/pages-templates-solution`.
