---
title: Extensões dos fragmentos de conteúdo do AEM
description: Saiba como criar e implantar extensões de fragmento de conteúdo as a Cloud Service para AEM
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
jira: KT-11603
thumbnail: KT-11603.png
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 9164423b-a609-4bc5-9777-112d229ae748
duration: 343
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '757'
ht-degree: 0%

---

# Extensibilidade de fragmentos de conteúdo do AEM

A interface do usuário de fragmentos de conteúdo do AEM é uma interface extensível e poderosa para gerenciar, criar, gerenciar e editar fragmentos de conteúdo. Há vários pontos de extensão disponíveis para personalizar a interface do usuário de acordo com suas necessidades. Diferentes pontos de extensão estão disponíveis com base na interface do usuário que você está estendendo.

## Pontos de extensão do Console de fragmentos de conteúdo

O Console do fragmento de conteúdo no AEM (Adobe Experience Manager) é uma interface de usuário que fornece um local centralizado para gerenciar e organizar fragmentos de conteúdo. Ele oferece um conjunto abrangente de ferramentas e recursos para criar, editar, publicar e rastrear fragmentos de conteúdo, permitindo que os usuários gerenciem com eficiência o conteúdo estruturado em vários canais e pontos de contato.

![Console de fragmentos de conteúdo](./assets/overview/cfc.png)

[Console de fragmentos de conteúdo do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=pt-BR) O é a interface do usuário extensível para listar e gerenciar fragmentos de conteúdo. [Extensões do console de fragmentos de conteúdo do AEM são criadas](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) usando o `@adobe/aem-cf-admin-ui-ext-tpl` Modelo do App Builder.

Os seguintes pontos de extensão do Console de fragmentos de conteúdo estão disponíveis:

<div class="columns is-multiline">
      <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action bar">
        <div class="card" style="height: 100%">
          <div class="card-image">
            <figure class="image is-16by9">
              <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" title="Barra de ação" tabindex="-1" target="_blank" rel="referrer">
                <img class="is-bordered-r-small" src="./assets/overview/cfc-action-bar.png" alt="Barra de ação">
              </a>
            </figure>
          </div>
          <div class="card-content is-padded-small">
            <div class="content">
              <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" title="Barra de ação" target="_blank" rel="referrer">Barra de ação</a></p>
              <p class="is-size-6">Personalize ações para quando um ou mais Fragmentos de conteúdo forem selecionados.</p>
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
          <p class="is-size-6">Personalize ações para quando nenhum Fragmento de conteúdo for selecionado.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir os documentos</span>
          </a>
        </div>
      </div>
    </div>
  </div>  
</div>

## Pontos de extensão do Editor de fragmentos de conteúdo

O Editor de fragmento de conteúdo no AEM (Adobe Experience Manager) é um componente da interface do usuário que permite aos usuários criar, editar e gerenciar fragmentos de conteúdo. Ele fornece um ambiente visualmente intuitivo e amigável para trabalhar com conteúdo estruturado, permitindo que os usuários definam e organizem elementos de conteúdo, apliquem modelos, gerenciem variações e visualizem como o conteúdo aparece em diferentes canais. O Editor de fragmento de conteúdo simplifica o processo de criação de conteúdo reutilizável e modular que pode ser facilmente distribuído e publicado em várias experiências digitais.

![Editor de fragmentos de conteúdo](./assets/overview/cfe.png)

O Editor de fragmentos de conteúdo do AEM é a interface do usuário extensível para editar fragmentos de conteúdo. [Extensões do Editor de Fragmento de conteúdo do AEM são criadas](https://developer.adobe.com/uix/docs/services/aem-cf-editor/code-generation/) usando o `@adobe/aem-cf-editor-ui-ext-tpl` Modelo do App Builder.

Os seguintes pontos de extensão do Editor de fragmentos de conteúdo estão disponíveis:

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
            <p class="is-size-6">Personalize ações no menu de cabeçalho do Editor de fragmento de conteúdo.</p>
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
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="Barra de ferramentas do Editor de Rich Text" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-toolbar.png" alt="Barra de ferramentas do Editor de Rich Text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="Barra de ferramentas do Editor de Rich Text"  target="_blank" rel="referrer">Barra de ferramentas do Editor de Rich Text</a></p>
          <p class="is-size-6">Adicione o botão personalizado ao Editor de Rich Text (RTE) do Editor de fragmento de conteúdo.</p>
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
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="Widgets do Editor de Rich Text" tabindex="-1"  target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-widgets.png" alt="Widgets do Editor de Rich Text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="Widgets do Editor de Rich Text" target="_blank" rel="referrer">Widgets do Editor de Rich Text</a></p>
          <p class="is-size-6">Personalize ações no RTE vinculadas a pressionamentos de tecla.</p>
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
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" title="Medalhas do editor de rich text" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-badges.png" alt="Medalhas do editor de rich text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/ " title="Medalhas do editor de rich text" target="_blank" rel="referrer">Medalhas do editor de rich text</a></p>
          <p class="is-size-6">Personalize blocos com estilo não editáveis dentro do RTE.</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exibir os documentos</span>
          </a>
        </div>
      </div>
    </div>
  </div>
</div>

## Exemplos de extensão

Bem-vindo a uma coleção de exemplos de código de extensibilidade da interface do usuário para AEM! Esse recurso foi projetado para fornecer demonstrações práticas e insights sobre a extensão da interface do usuário do Adobe Experience Manager (AEM). Se você é um desenvolvedor que busca aprimorar a funcionalidade do AEM, esses exemplos de código servem como uma referência valiosa.

<div class="columns is-multiline">
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/console-bulk-property-update.md" title="Atualização de propriedade em massa" tabindex="-1">
            <img class="is-bordered-r-small" src="./assets/../examples/assets/bulk-property-update/card.png" alt="Atualização de propriedade em massa">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-bulk-property-update.md" title="Atualização de propriedade em massa">Atualização em massa da propriedade do fragmento de conteúdo</a></p>
          <p class="is-size-6">Uma extensão da Barra de ação do console de Fragmento de conteúdo com ações modais e do Adobe I/O Runtime.</p>
          <a href="./examples/console-bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Veja o exemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="OpenAI-based image generation and upload to AEM extension">
        <div class="card" style="height: 100%">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./examples/console-image-generation-and-image-upload.md" title="Geração de imagens com base em OpenAI e upload para a extensão AEM" tabindex="-1">
                        <img class="is-bordered-r-small" src="./examples/assets/digital-image-generation/card.png" alt="Geração de imagens com base em OpenAI e upload para a extensão AEM">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-image-generation-and-image-upload.md" title="Geração de imagens com base em OpenAI e upload para a extensão AEM">Geração de imagem OpenAPI</a></p>
                    <p class="is-size-6">Explore um exemplo de extensão da barra de ação que gera uma imagem usando OpenAI, faz o upload para AEM e atualiza a propriedade da imagem no Fragmento de conteúdo selecionado.</p>
                    <a href="./examples/console-image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Veja o exemplo</span>
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
          <p class="is-size-6">Adicione uma coluna personalizada ao Console de fragmentos de conteúdo.</p>
          <a href="./examples/custom-grid-columns.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Veja o exemplo</span>
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
          <p class="is-size-6">Exporte um fragmento de conteúdo como XML do editor de fragmentos de conteúdo.</p>
          <a href="./examples/editor-export-to-xml.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Veja o exemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor toolbar button">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-toolbar.md" title="Botão da barra de ferramentas do Editor de Rich Text" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-toolbar-card.png" alt="Botão da barra de ferramentas do Editor de Rich Text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="Botão da barra de ferramentas do Editor de Rich Text">Botão da barra de ferramentas do Editor de Rich Text</a></p>
          <p class="is-size-6">Adicionar botões personalizados da barra de ferramentas aos campos do RTE no Editor de fragmento de conteúdo.</p>
          <a href="./examples/editor-rte-toolbar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Veja o exemplo</span>
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
          <p class="is-size-6">Adicionar widgets ao Editor de Rich Text no Editor de fragmento de conteúdo.</p>
          <a href="./examples/editor-rte-widget.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Veja o exemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div>   
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor Badge">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-badges.md" title="Medalha do Editor de Rich Text" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-badge-card.png" alt="Medalha do Editor de Rich Text">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-badges.md" title="Medalha do Editor de Rich Text">Medalha do Editor de Rich Text</a></p>
          <p class="is-size-6">Adicionar selos ao Editor de Rich Text no Editor de fragmento de conteúdo.</p>
          <a href="./examples/editor-rte-badges.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Veja o exemplo</span>
          </a>
        </div>
      </div>
    </div>
  </div> 
</div>
