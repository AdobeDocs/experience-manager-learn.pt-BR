---
title: Extensões de fragmentos de conteúdo do AEM
description: Saiba como criar e implantar extensões de fragmento de conteúdo do AEM as a Cloud Service
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
jira: KT-11603
thumbnail: KT-11603.png
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 9164423b-a609-4bc5-9777-112d229ae748
duration: 126
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '773'
ht-degree: 100%

---

# Extensibilidade de fragmentos de conteúdo do AEM

A interface de fragmentos de conteúdo do AEM é uma interface extensível poderosa para gerenciar a criação, o gerenciamento e a edição de fragmentos de conteúdo. Há vários pontos de extensão disponíveis para personalizar a interface de acordo com suas necessidades. Diferentes pontos de extensão estão disponíveis com base na interface que você está estendendo.

## Pontos de extensão do console de fragmentos de conteúdo

O console de fragmento de conteúdo no AEM (Adobe Experience Manager) é uma interface que fornece um local centralizado para gerenciar e organizar fragmentos de conteúdo. Ele oferece um conjunto abrangente de ferramentas e recursos para criar, editar, publicar e rastrear fragmentos de conteúdo, permitindo que os usuários gerenciem com eficiência o conteúdo estruturado em vários canais e pontos de contato.

![Console de fragmentos de conteúdo](./assets/overview/cfc.png)

O [Console de fragmentos de conteúdo do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=pt-BR) é a interface extensível para listar e gerenciar fragmentos de conteúdo. [As extensões do console de fragmentos de conteúdo do AEM são criadas](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) usando o modelo `@adobe/aem-cf-admin-ui-ext-tpl` do App Builder.

Os seguintes pontos de extensão do console de fragmentos de conteúdo estão disponíveis:

<div class="columns is-multiline">
      <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action bar">
        <div class="card" style="height: 100%">
          <div class="card-image">
            <figure class="image is-16by9">
              <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" title="Barra de ações" tabindex="-1" target="_blank" rel="referrer">
                <img class="is-bordered-r-small" src="./assets/overview/cfc-action-bar.png" alt="Barra de ações">
              </a>
            </figure>
          </div>
          <div class="card-content is-padded-small">
            <div class="content">
              <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" title="Barra de ações" target="_blank" rel="referrer">Barra de ações</a></p>
              <p class="is-size-6">Personalize ações para quando um ou mais fragmentos de conteúdo forem selecionados.</p>
              <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir os documentos</span>
              </a>
            </div>
          </div>
        </div>
      </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Grid columns">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" title="Colunas de grade" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfc-grid-columns.png" alt="Colunas de grade">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" title="Colunas de grade" target="_blank" rel="referrer">Colunas de grade</a></p>
          <p class="is-size-6">Personalize os dados que aparecem na lista Fragmentos de conteúdo.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir os documentos</span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" title="Menu de cabeçalho" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfc-header-menu.png" alt="Menu de cabeçalho">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" title="Menu de cabeçalho" target="_blank" rel="referrer">Menu de cabeçalho</a></p>
          <p class="is-size-6">Personalize ações para quando nenhum fragmento de conteúdo for selecionado.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir os documentos</span>
          </a>
        </div>
      </div>
    </div>
  </div>  
</div>

## Pontos de extensão do editor de fragmentos de conteúdo

O editor de fragmento de conteúdo no AEM (Adobe Experience Manager) é um componente da interface que permite aos usuários criar, editar e gerenciar fragmentos de conteúdo. Ele fornece um ambiente visualmente intuitivo e amigável para trabalhar com conteúdo estruturado, permitindo que os usuários definam e organizem elementos de conteúdo, apliquem modelos, gerenciem variações e visualizem como o conteúdo aparece em diferentes canais. O editor de fragmentos de conteúdo simplifica o processo de criação de conteúdo reutilizável e modular, que pode ser facilmente distribuído e publicado em várias experiências digitais.

![Editor de fragmentos de conteúdo](./assets/overview/cfe.png)

O editor de fragmentos de conteúdo do AEM é a interface extensível para editar fragmentos de conteúdo. [As extensões do editor de fragmento de conteúdo do AEM são criadas](https://developer.adobe.com/uix/docs/services/aem-cf-editor/code-generation/) usando o modelo `@adobe/aem-cf-editor-ui-ext-tpl` do App Builder.

Os seguintes pontos de extensão do editor de fragmentos de conteúdo estão disponíveis:

<div class="columns is-multiline">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
      <div class="card" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu" title="Menu de cabeçalho" tabindex="-1" target="_blank" rel="referrer">
              <img class="is-bordered-r-small" src="./assets/overview/cfe-header-menu.png" alt="Menu de cabeçalho">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu/" title="Menu de cabeçalho" target="_blank" rel="referrer">Menu de cabeçalho</a></p>
            <p class="is-size-6">Personalize ações no menu do cabeçalho do editor de fragmentos de conteúdo.</p>
            <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
              <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir os documentos</span>
            </a>
          </div>
        </div>
      </div>
    </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor toolbar">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="Barra de ferramentas do editor de rich text" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-toolbar.png" alt="Barra de ferramentas do editor de rich text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="Barra de ferramentas do editor de rich text"  target="_blank" rel="referrer">Barra de ferramentas do editor de rich text</a></p>
          <p class="is-size-6">Adicione o botão personalizado ao editor de rich text (RTE) do editor de fragmentos de conteúdo.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir os documentos</span>
          </a>
        </div>
      </div>
    </div>
  </div>

<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor widgets">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="Widgets do editor de rich text" tabindex="-1"  target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-widgets.png" alt="Widgets do editor de rich text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="Widgets do editor de rich text" target="_blank" rel="referrer">Widgets do editor de rich text</a></p>
          <p class="is-size-6">Personalize ações no RTE vinculadas a teclas pressionadas.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir os documentos</span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor badges">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" title="Selos do editor de rich text" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-badges.png" alt="Selos do editor de rich text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/ " title="Selos do editor de rich text" target="_blank" rel="referrer">Selos do editor de rich text</a></p>
          <p class="is-size-6">Personalize blocos estilizados não editáveis dentro do RTE.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir os documentos</span>
          </a>
        </div>
      </div>
    </div>
  </div>
</div>

## Exemplos de extensão

Bem-vindo a uma coleção de exemplos de código de extensibilidade da interface do AEM. Este recurso foi projetado para fornecer demonstrações e insights práticos sobre a extensão da interface do usuário do Adobe Experience Manager (AEM). Se você for um(a) desenvolvedor(a) que deseja aprimorar a funcionalidade do AEM, estes exemplos de código serão uma referência valiosa.

<div class="columns is-multiline">
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/console-bulk-property-update.md" title="Atualização de propriedades em massa" tabindex="-1">
            <img class="is-bordered-r-small" src="./assets/../examples/assets/bulk-property-update/card.png" alt="Atualização de propriedades em massa">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-bulk-property-update.md" title="Atualização de propriedades em massa">Atualização em massa de propriedades dos fragmentos de conteúdo</a></p>
          <p class="is-size-6">Uma extensão da barra de ação do console de fragmentos de conteúdo com ações modais e do Adobe I/O Runtime.</p>
          <a href="./examples/console-bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir o exemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="OpenAI-based image generation and upload to AEM extension">
        <div class="card" style="height: 100%">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./examples/console-image-generation-and-image-upload.md" title="Geração de imagens baseada na OpenAI e upload para a extensão do AEM" tabindex="-1">
                        <img class="is-bordered-r-small" src="./examples/assets/digital-image-generation/card.png" alt="Geração de imagens baseada na OpenAI e upload para a extensão do AEM">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-image-generation-and-image-upload.md" title="Geração de imagens baseada na OpenAI e upload para a extensão do AEM">Geração de imagens pela OpenAPI</a></p>
                    <p class="is-size-6">Confira um exemplo de extensão da barra de ação que gera uma imagem pela OpenAI, faz seu upload no AEM e atualiza a propriedade da imagem no fragmento de conteúdo selecionado.</p>
                    <a href="./examples/console-image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir o exemplo</span>
                    </a>
                </div>
            </div>
        </div>
    </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Custom columns">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/custom-grid-columns.md" title="Colunas personalizadas" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/custom-grid-columns/card.png" alt="Colunas personalizadas">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/custom-grid-columns.md" title="Colunas personalizadas">Colunas personalizadas</a></p>
          <p class="is-size-6">Adicione uma coluna personalizada ao console de fragmentos de conteúdo.</p>
          <a href="./examples/custom-grid-columns.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir o exemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Export to XML">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-export-to-xml.md" title="Exportar para XML" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/export-to-xml/card.png" alt="Exportar para XML">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-export-to-xml.md" title="Exportar para XML">Exportar para XML</a></p>
          <p class="is-size-6">Exporte um fragmento de conteúdo como XML a partir do editor de fragmentos de conteúdo.</p>
          <a href="./examples/editor-export-to-xml.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir o exemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor toolbar button">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-toolbar.md" title="Botão da barra de ferramentas do editor de rich text" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-toolbar-card.png" alt="Botão da barra de ferramentas do editor de rich text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="Botão da barra de ferramentas do editor de rich text">Botão da barra de ferramentas do editor de rich text</a></p>
          <p class="is-size-6">Adicione botões personalizados da barra de ferramentas aos campos do RTE no editor de fragmentos de conteúdo.</p>
          <a href="./examples/editor-rte-toolbar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir o exemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div>   
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor Widget">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-widget.md" title="Widget do editor de rich text" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-widget-card.png" alt="Widget do editor de rich text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="Widget do editor de rich text">Widget do editor de rich text</a></p>
          <p class="is-size-6">Adicionar widgets ao editor de rich text no editor de fragmentos de conteúdo.</p>
          <a href="./examples/editor-rte-widget.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir o exemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div>   
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor Badge">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-badges.md" title="Selo do editor de rich text" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-badge-card.png" alt="Selo do editor de rich text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-badges.md" title="Selo do editor de rich text">Selo do editor de rich text</a></p>
          <p class="is-size-6">Adicione selos ao editor de rich text no editor de fragmentos de conteúdo.</p>
          <a href="./examples/editor-rte-badges.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir o exemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div>

<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Custom fields">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-custom-field.md" title="Campos personalizados" tabindex="-1">
            <img class="is-bordered-r-small" src="https://video.tv.adobe.com/v/3427585?format=jpeg" alt="Campos personalizados">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-custom-field.md" title="Campos personalizados">Campos personalizados</a></p>
          <p class="is-size-6">Crie campos personalizados de fragmentos de conteúdo.</p>
          <a href="./examples/editor-custom-field.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir o exemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div> 
</div>
