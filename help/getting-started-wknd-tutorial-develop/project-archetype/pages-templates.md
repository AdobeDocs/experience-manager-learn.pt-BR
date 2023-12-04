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
duration: 1102
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2855'
ht-degree: 0%

---

# Páginas e modelos {#pages-and-template}

{{edge-delivery-services-and-page-editor}}

Neste capítulo, vamos explorar a relação entre um componente de página base e modelos editáveis. Saiba como criar um modelo de artigo sem estilo com base em alguns modelos de [Adobe XD](https://helpx.adobe.com/support/xd.html). No processo de criação do modelo, os Componentes principais e as configurações de política avançadas dos Modelos editáveis são abordados.

## Pré-requisitos {#prerequisites}

Analisar as ferramentas e instruções necessárias para a configuração de um [ambiente de desenvolvimento local](overview.md#local-dev-environment).

### Projeto inicial

>[!NOTE]
>
> Se você concluiu com êxito o capítulo anterior, é possível reutilizar o projeto e ignorar as etapas para verificar o projeto inicial.

Verifique o código de linha base no qual o tutorial se baseia:

1. Confira o `tutorial/pages-templates-start` ramificar de [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > Se estiver usando o AEM 6.5 ou 6.4, anexe o `classic` para qualquer comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Você sempre pode exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) ou confira o código localmente alternando para a ramificação `tutorial/pages-templates-solution`.

## Objetivo

1. O Inspect cria um design de página criado no Adobe XD e o mapeia para os Componentes principais.
1. Entenda os detalhes de Modelos editáveis e como as políticas podem ser usadas para impor o controle granular do conteúdo da página.
1. Saiba como os Modelos e as Páginas são vinculados

## O que você vai criar {#what-build}

Nesta parte do tutorial, você cria um novo Modelo de página de artigo que pode ser usado para criar páginas de artigo e se alinha a uma estrutura comum. O Modelo de página de artigo é baseado em designs e em um kit de interface do usuário produzido no Adobe XD. Este capítulo foca apenas na criação da estrutura ou esqueleto do template. Nenhum estilo é implementado, mas o modelo e as páginas são funcionais.

![Design da página de artigo e versão sem estilo](assets/pages-templates/what-you-will-build.png)

## Planejamento da interface do usuário com o Adobe XD {#adobexd}

Normalmente, o planejamento de um novo site começa com modelos e designs estáticos. [Adobe XD](https://helpx.adobe.com/support/xd.html) O é uma ferramenta de design que cria experiência do usuário. Em seguida, vamos inspecionar um Kit de interface do usuário e modelos para ajudar a planejar a estrutura do Modelo de página de artigo.

>[!VIDEO](https://video.tv.adobe.com/v/30214?quality=12&learn=on)

**Baixe o [Arquivo de design do artigo da WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> Um genérico [O kit de interface do usuário dos Componentes principais de AEM também está disponível](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) como ponto de partida para projetos personalizados.

## Criar o modelo da página de artigo

Ao criar uma página, você deve selecionar um modelo, que é usado como base para a criação da página. O modelo define a estrutura da página resultante, o conteúdo inicial e os componentes permitidos.

Existem três áreas principais de [Modelos editáveis](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=pt-BR):

1. **Estrutura** - define os componentes que fazem parte do modelo. Eles não podem ser editados por autores de conteúdo.
1. **Conteúdo inicial** - define os componentes com os quais o modelo começa; eles podem ser editados e/ou excluídos por autores de conteúdo
1. **Políticas** - define as configurações sobre como os componentes se comportam e quais opções os autores têm.

Em seguida, crie um modelo no AEM que corresponda à estrutura dos modelos. Isso ocorre em uma instância local de AEM. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/330991?quality=12&learn=on)

Etapas de alto nível para o vídeo acima:

### Configurações de estrutura

1. Criar um modelo usando o **Tipo de modelo de página**, nomeado **Página do artigo**.
1. Alterne para **Estrutura** modo.
1. Adicionar um **Fragmento de experiência** componente que atuará como o **Cabeçalho** na parte superior do modelo.
   * Configurar o componente para apontar para `/content/experience-fragments/wknd/us/en/site/header/master`.
   * Defina a política como **Cabeçalho da página** e assegurar que o **Elemento padrão** está definida como `header`. A variável `header`O elemento é direcionado com CSS no próximo capítulo.
1. Adicionar um **Fragmento de experiência** componente que atuará como o **Rodapé** na parte inferior do modelo.
   * Configurar o componente para apontar para `/content/experience-fragments/wknd/us/en/site/footer/master`.
   * Defina a política como **Rodapé da página** e assegurar que o **Elemento padrão** está definida como `footer`. A variável `footer` O elemento é direcionado com CSS no próximo capítulo.
1. Bloquear o **main** contêiner que foi incluído quando o modelo foi criado inicialmente.
   * Defina a política como **Página principal** e assegurar que o **Elemento padrão** está definida como `main`. A variável `main` O elemento é direcionado com CSS no próximo capítulo.
1. Adicionar um **Imagem** componente para a **main** recipiente.
   * Desbloqueie o **Imagem** componente.
1. Adicionar um **Navegação estrutural** componente abaixo de **Imagem** no contêiner principal.
   * Criar uma política para o **Navegação estrutural** componente nomeado **Página de artigo - Navegação estrutural**. Defina o **Nível inicial da navegação** para **4**.
1. Adicionar um **Container** componente abaixo de **Navegação estrutural** e dentro do **main** recipiente. Este atua como o **Contêiner de conteúdo** para o modelo.
   * Desbloqueie o **Conteúdo** recipiente.
   * Defina a política como **Conteúdo da página**.
1. Adicionar outro **Container** componente abaixo de **Contêiner de conteúdo**. Este atua como o **Trilho lateral** contêiner do modelo.
   * Desbloqueie o **Trilho lateral** recipiente.
   * Criar uma política chamada **Página de artigo - Painel lateral**.
   * Configure o **Componentes permitidos** em **Projeto de sites WKND - Conteúdo** para incluir: **Botão**, **Baixar**, **Imagem**, **Lista**, **Separador**, **Compartilhamento em redes sociais**, **Texto**, e **Título**.
1. Atualize a política do container Raiz da página. Este é o contêiner mais externo do modelo. Defina a política como **Raiz da página**.
   * Em **Configurações do contêiner**, defina o **Layout** para **Grade responsiva**.
1. Ativar modo de layout para o **Contêiner de conteúdo**. Arraste a alça da direita para a esquerda e reduza o contêiner para oito colunas de largura.
1. Ativar modo de layout para o **Contêiner do painel lateral**. Arraste a alça da direita para a esquerda e reduza o contêiner para ter quatro colunas de largura. Em seguida, arraste a alça esquerda da esquerda para a direita uma coluna para tornar o contêiner com 3 colunas de largura e deixar um intervalo de 1 coluna entre as **Contêiner de conteúdo**.
1. Abra o emulador móvel e alterne para um ponto de interrupção móvel. Ative o modo de layout novamente e faça com que o **Contêiner de conteúdo** e a variável **Contêiner do painel lateral** a largura total da página. Isso empilha os contêineres verticalmente no ponto de interrupção móvel.
1. Atualize a política do **Texto** componente no **Contêiner de conteúdo**.
   * Defina a política como **Texto do conteúdo**.
   * Em **Plug-ins** > **Estilos de parágrafo**, verificar **Ativar estilos de parágrafo** e assegurar que o **Bloco de cotação** está ativado.

### Configurações do conteúdo inicial

1. Alternar para **Conteúdo inicial** modo.
1. Adicionar um **Título** componente para a **Contêiner de conteúdo**. Atua como o título do Artigo. Quando deixado em branco, ele exibe automaticamente o Título da página atual.
1. Adicionar um segundo **Título** abaixo do primeiro componente de Título.
   * Configure o componente com o texto: &quot;Por autor&quot;. Este é um espaço reservado para texto.
   * Defina o tipo a ser `H4`.
1. Adicionar um **Texto** componente abaixo de **Por autor** Componente do título.
1. Adicionar um **Título** componente para a **Contêiner do painel lateral**.
   * Configure o componente com o texto: &quot;Compartilhar esta história&quot;.
   * Defina o tipo a ser `H5`.
1. Adicionar um **Compartilhamento em redes sociais** componente abaixo de **Compartilhar esta história** Componente do título.
1. Adicionar um **Separador** componente abaixo de **Compartilhamento em redes sociais** componente.
1. Adicionar um **Baixar** componente abaixo de **Separador** componente.
1. Adicionar um **Lista** componente abaixo de **Baixar** componente.
1. Atualize o **Propriedades da página inicial** para o modelo.
   * Em **Redes sociais** > **Compartilhamento em redes sociais**, verificar **Facebook** e **Pinterest**

### Ativar o modelo e adicionar uma miniatura

1. Visualize o modelo no console Modelo navegando até [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **Ativar** o modelo da Página de artigo.
1. Edite as propriedades do modelo Página de artigo e faça upload da seguinte miniatura para identificar rapidamente as páginas criadas usando o modelo Página de artigo:

   ![Miniatura do modelo de página de artigo](assets/pages-templates/article-page-template-thumbnail.png)

## Atualizar o cabeçalho e o rodapé com fragmentos de experiência {#experience-fragments}

Uma prática comum ao criar conteúdo global, como um cabeçalho ou rodapé, é usar um [Fragmento de experiência](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Fragmentos de experiência, permitem que os usuários combinem vários componentes para criar um único componente que pode ser referenciado. Os Fragmentos de experiência têm a vantagem de oferecer suporte ao gerenciamento de vários sites e [localização](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=en).

O Arquétipo de projeto AEM gerou um Cabeçalho e Rodapé. Em seguida, atualize os Fragmentos de experiência para corresponder aos modelos. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/330992?quality=12&learn=on)

Etapas de alto nível para o vídeo acima:

1. Baixe o pacote de conteúdo de amostra **[WKND-PáginasModelos-Conteúdo-Ativos.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**.
1. Faça upload e instale o pacote de conteúdo usando o Gerenciador de pacotes em [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Atualizar o modelo de variação da Web, que é o modelo usado para Fragmentos de experiência em [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)
   * Atualizar a política no **Container** no modelo.
   * Defina a política como **Raiz XF**.
   * Em, a variável **Componentes permitidos** selecionar o grupo de componentes **Projeto de sites WKND - Estrutura** para incluir **Navegação por idiomas**, **Navegação**, e **Pesquisa rápida** componentes.

### Atualizar cabeçalho do fragmento de experiência

1. Abra o Fragmento de experiência que renderiza o Cabeçalho em [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. Configurar a raiz **Container** do fragmento. Este é o mais externo **Container**.
   * Defina o **Layout** para **Grade responsiva**
1. Adicione o **Logotipo escuro da WKND** como uma imagem na parte superior do **Container**. O logotipo foi incluído no pacote instalado em uma etapa anterior.
   * Modifique o layout da **Logotipo escuro da WKND** para ser **dois** colunas de largura. Arraste as alças da direita para a esquerda.
   * Configure o logotipo com **Texto alternativo** do &quot;Logotipo da WKND&quot;.
   * Configure o logotipo para **Link** para `/content/wknd/us/en` a página inicial.
1. Configure o **Navegação** que já foi colocado na página.
   * Defina o **Excluir níveis de raiz** para **1**.
   * Defina o **Profundidade da estrutura de navegação** para **1**.
   * Modifique o layout da **Navegação** componente a ser **oito** colunas de largura. Arraste as alças da direita para a esquerda.
1. Remova o **Navegação por idiomas** componente.
1. Modifique o layout da **Pesquisar** componente a ser **dois** colunas de largura. Arraste as alças da direita para a esquerda. Todos os componentes agora devem ser alinhados horizontalmente em uma única linha.

### Atualizar fragmento de experiência do rodapé

1. Abrir o fragmento de experiência que renderiza o rodapé em [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. Configurar a raiz **Container** do fragmento. Este é o mais externo **Container**.
   * Defina o **Layout** para **Grade responsiva**
1. Adicione o **Logotipo leve WKND** como uma imagem na parte superior do **Container**. O logotipo foi incluído no pacote instalado em uma etapa anterior.
   * Modifique o layout da **Logotipo leve WKND** para ser **dois** colunas de largura. Arraste as alças da direita para a esquerda.
   * Configure o logotipo com **Texto alternativo** da &quot;WKND Logo Light&quot;.
   * Configure o logotipo para **Link** para `/content/wknd/us/en` a página inicial.
1. Adicionar um **Navegação** abaixo do logotipo. Configure o **Navegação** componente:
   * Defina o **Excluir níveis de raiz** para **1**.
   * Desmarcar **Coletar todas as páginas secundárias**.
   * Defina o **Profundidade da estrutura de navegação** para **1**.
   * Modifique o layout da **Navegação** componente a ser **oito** colunas de largura. Arraste as alças da direita para a esquerda.

## Criar uma página de artigo

Em seguida, crie uma página usando o modelo Página de artigo. Crie o conteúdo da página para corresponder aos modelos de site. Siga as etapas do vídeo abaixo:

>[!VIDEO](https://video.tv.adobe.com/v/330993?quality=12&learn=on)

Etapas de alto nível para o vídeo acima:

1. Navegue até o console Sites em [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. Criar uma página abaixo de **WKND** > **EUA** > **EN** > **Revista**.
   * Escolha o **Página do artigo** modelo.
   * Em **Propriedades** defina o **Título** para &quot;Ultimate Guide to LA Skateparks&quot;
   * Defina o **Nome** para &quot;guide-la-skateparks&quot;
1. Substituir **Por autor** Título com o texto &quot;Por Stacey Roswells&quot;.
1. Atualize o **Texto** para incluir um parágrafo para preencher o artigo. Você pode usar o seguinte arquivo de texto como cópia: [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. Adicionar outro **Texto** componente.
   * Atualize o componente para incluir a citação: &quot;Não há lugar melhor para destruir do que Los Angeles&quot;.
   * Editar o Editor de Rich Text no modo de tela cheia e modificar a cotação acima para usar a tag **Bloco de Cotação** elemento.
1. Continue preenchendo o corpo do artigo para corresponder aos modelos.
1. Configure o **Baixar** componente para usar uma versão PDF do artigo.
   * Em **Baixar** > **Propriedades**, clique na caixa de seleção para **Obter o título do ativo DAM**.
   * Defina o **Descrição** para: &quot;Obter a história completa&quot;.
   * Defina o **Texto de ação** para: &quot;Baixar PDF&quot;.
1. Configure o **Lista** componente.
   * Em **Configurações da lista** > **Criar lista usando**, selecione **Páginas filhas**.
   * Defina o **Página principal** para `/content/wknd/us/en/magazine`.
   * Em, a variável **Configurações do item** check **Vincular itens** e verificar **Mostrar data**.

## Inspect a estrutura do nó {#node-structure}

Nesse ponto, a página do artigo está claramente sem estilo. No entanto, a estrutura básica está em vigor. Em seguida, inspecione a estrutura do nó da página de artigo para entender melhor a função do modelo, da página e dos componentes.

Use a ferramenta CRXDE-Lite em uma instância de AEM local para visualizar a estrutura do nó subjacente.

1. Abertura [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) e use a navegação em árvore para navegar até `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Clique no link `jcr:content` abaixo do nó `la-skateparks` e exiba as propriedades:

   ![Propriedades de conteúdo JCR](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Observe o valor de `cq:template`, que aponta para `/conf/wknd/settings/wcm/templates/article-page`, o Modelo de página de artigo criado anteriormente.

   Observe também o valor de `sling:resourceType`, que aponta para `wknd/components/page`. É o componente Página criado pelo arquétipo de projeto AEM e é responsável pela renderização da página com base no modelo.

1. Expanda a `jcr:content` nó abaixo `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` e exiba a hierarquia do nó:

   ![JCR Content LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   Você deve ser capaz de mapear livremente cada um dos nós para componentes que foram criados. Veja se você pode identificar os diferentes Contêineres de layout usados ao inspecionar os nós com o prefixo `container`.

1. Em seguida, inspecione o componente de página em `/apps/wknd/components/page`. Veja as propriedades do componente no CRXDE Lite:

   ![Propriedades do componente de Página](assets/pages-templates/page-component-properties.png)

   Há apenas dois scripts HTL, `customfooterlibs.html` e `customheaderlibs.html` abaixo do componente página. *Então, como esse componente renderiza a página?*

   A variável `sling:resourceSuperType` a propriedade aponta para `core/wcm/components/page/v2/page`. Essa propriedade permite que o componente de página da WKND herde **all** a funcionalidade do componente de página, dos Componentes principais. Este é o primeiro exemplo de algo chamado [Padrão do componente proxy](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). Mais informações podem ser encontradas [aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect outro componente nos componentes WKND, o `Breadcrumb` componente de: `/apps/wknd/components/breadcrumb`. Observe que o mesmo `sling:resourceSuperType` propriedade pode ser encontrada, mas dessa vez aponta para `core/wcm/components/breadcrumb/v2/breadcrumb`. Este é outro exemplo de como usar o padrão do componente Proxy para incluir um Componente principal. Na verdade, todos os componentes na base de código WKND são proxies de componentes principais AEM (exceto para o componente de demonstração personalizada HelloWorld ). É uma prática recomendada reutilizar o máximo possível da funcionalidade dos Componentes principais *antes* gravando código personalizado.

1. Em seguida, verifique a página dos Componentes principais em `/libs/core/wcm/components/page/v2/page` usando o CRXDE Lite:

   >[!NOTE]
   >
   > No, AEM 6.5/6.4, os Componentes principais estão localizados em `/apps/core/wcm/components`. No, AEM as a Cloud Service, os Componentes principais estão localizados em `/libs` e são atualizados automaticamente.

   ![Página Componente principal](assets/pages-templates/core-page-component-properties.png)

   Observe que muitos arquivos de script estão incluídos abaixo dessa página. A página dos Componentes principais contém várias funcionalidades. Essa funcionalidade é dividida em vários scripts para facilitar a manutenção e a leitura. É possível rastrear a inclusão dos scripts HTL abrindo o `page.html` e procurando `data-sly-include`:

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

   O outro motivo para dividir o HTL em vários scripts é permitir que os componentes proxy substituam scripts individuais para implementar uma lógica de negócios personalizada. Os scripts HTL `customfooterlibs.html`, e `customheaderlibs.html`, são criados para que a finalidade explícita seja substituída pela implementação de projetos.

   Você pode saber mais sobre como o Modelo editável influencia a renderização da variável [página de conteúdo lendo este artigo](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=pt-BR).

1. Inspect é outro Componente principal, como a Navegação estrutural em `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Exibir o `breadcrumb.html` script para entender como a marcação do componente de Navegação estrutural é gerada.

## Salvando Configurações no Controle de Origem {#configuration-persistence}

Geralmente, especialmente no início de um projeto AEM, é valioso manter as configurações, como modelos e políticas de conteúdo relacionadas, no controle de origem. Isso garante que todos os desenvolvedores trabalhem com o mesmo conjunto de conteúdo e configurações e possa garantir consistência adicional entre os ambientes. Quando um projeto atinge um determinado nível de maturidade, a prática de gerenciar modelos pode ser transferida para um grupo especial de usuários avançados.


Por enquanto, os modelos são tratados como outras partes do código e sincronizam o **Modelo da página de artigo** como parte do projeto.
Até agora, o código é enviado do projeto AEM para uma instância local de AEM. A variável **Modelo da página de artigo** foi criado diretamente em uma instância local de AEM, portanto, é necessário **importar** o modelo no projeto AEM. A variável **ui.content** O módulo está incluído no projeto AEM para essa finalidade específica.

As próximas etapas são realizadas no VSCode IDE usando o [Sincronização VSCode com AEM](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) plug-in. Mas eles podem estar usando qualquer IDE que você tenha configurado para **importar** ou importe conteúdo de uma instância local do AEM.

1. No, o VSCode abre a variável `aem-guides-wknd` projeto.

1. Expanda a **ui.content** módulo no Gerenciador de projetos. Expanda a `src` e navegue até `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Clique com o botão direito] o `templates` e selecione **Importar do servidor AEM**:

   ![Modelo de importação de VSCode](assets/pages-templates/vscode-import-templates.png)

   A variável `article-page` devem ser importados e o `page-content`, `xf-web-variation` Os modelos de também devem ser atualizados.

   ![Modelos atualizados](assets/pages-templates/updated-templates.png)

1. Repita as etapas para importar conteúdo, mas selecione a variável **políticas** pasta de `/conf/wknd/settings/wcm/policies`.

   ![Políticas de importação do VSCode](assets/pages-templates/policies-article-page-template.png)

1. INSPECT o `filter.xml` arquivo de `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   A variável `filter.xml` O arquivo é responsável por identificar os caminhos dos nós instalados com o pacote. Observe a `mode="merge"` em cada filtro que indica que o conteúdo existente não deve ser modificado, somente o novo conteúdo será adicionado. Como os autores de conteúdo podem estar atualizando esses caminhos, é importante que uma implantação de código **não** substituir conteúdo. Consulte a [Documentação do FileVault](https://jackrabbit.apache.org/filevault/filter.html) para obter mais detalhes sobre como trabalhar com elementos de filtro.

   Comparar `ui.content/src/main/content/META-INF/vault/filter.xml` e `ui.apps/src/main/content/META-INF/vault/filter.xml` para entender os diferentes nós gerenciados por cada módulo.

   >[!WARNING]
   >
   > Para garantir implantações consistentes para o site de referência da WKND, algumas ramificações do projeto são configuradas de modo que `ui.content` substitui todas as alterações no JCR. Isso ocorre por design, ou seja, para Ramificações de solução, já que o código/estilos são criados para políticas específicas.

## Parabéns. {#congratulations}

Parabéns, você criou um modelo e uma página com o Adobe Experience Manager Sites.

### Próximas etapas {#next-steps}

Nesse ponto, a página do artigo está claramente sem estilo. Siga as [Bibliotecas do lado do cliente e fluxo de trabalho de front-end](client-side-libraries.md) tutorial para saber mais sobre as práticas recomendadas para incluir CSS e JavaScript para aplicar estilos globais ao site e integrar uma build de front-end dedicada.

Exibir o código concluído em [GitHub](https://github.com/adobe/aem-guides-wknd) ou revise e implante o código localmente em na ramificação Git `tutorial/pages-templates-solution`.

1. Clonar o [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) repositório.
1. Confira o `tutorial/pages-templates-solution` filial.
