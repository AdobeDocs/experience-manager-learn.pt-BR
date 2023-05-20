---
title: Extensões do console de Fragmento de conteúdo do AEM
description: Saiba como criar e implantar extensões do console de Fragmento de conteúdo as a Cloud Service para AEM
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2022-12-09T00:00:00Z
exl-id: 093c8d83-2402-4feb-8a56-267243d229dd
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 4%

---

# Extensão do console de fragmentos de conteúdo do AEM

[Console de fragmentos de conteúdo do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=pt-BR) extensões podem ser adicionadas por meio de dois pontos de extensão: um botão na [Console do fragmento de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=pt-BR) menu de cabeçalho ou barra de ação. As extensões são escritas em JavaScript, executadas como aplicativos do App Builder, e podem implementar uma interface do usuário personalizada da Web e ações do Adobe I/O Runtime sem servidor para realizar trabalhos mais intensos e de longa duração.

![Extensão do console de fragmentos de conteúdo do AEM](./assets/overview/example.png){align="center"}

| Tipo de extensão | Descrição | Parâmetro(s) |
| :--- | :--- | :--- |
| Menu de cabeçalho | Adiciona um botão ao cabeçalho que é exibido quando __zero__ Fragmentos de conteúdo são selecionados. | Nenhum. |
| Barra de ação | Adiciona um botão à barra de ações que é exibido quando __um ou mais__ Fragmentos de conteúdo são selecionados. | Uma matriz dos caminhos dos fragmentos de conteúdo selecionados. |

Uma única extensão do Console de fragmentos de conteúdo AEM pode incluir zero ou um menu de Cabeçalho e zero ou um tipo de extensão da barra de ação. Se vários tipos de extensão do mesmo tipo forem necessários, várias extensões do Console de fragmentos de conteúdo do AEM deverão ser criadas.

Extensões do console de fragmentos de conteúdo AEM, exigem uma [Projeto do console do Adobe Developer](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console) e uma [aplicativo App Builder](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) usando o `@adobe/aem-cf-admin-ui-ext-tpl` associado ao projeto do Adobe Developer Console.

Selecione entre os seguintes recursos ao gerar o aplicativo App Builder, com base no que a extensão faz. É possível usar qualquer combinação de opções em uma extensão.

|  | Adicionar botão a [Menu de cabeçalho](./header-menu.md) | Adicionar botão a [Barra de ação](./action-bar.md) | Mostrar [Modal](./modal.md) | Adicionar [manipulador do lado do servidor](./runtime-action.md) |
| ------------------------------------------ | :-----------------------: | :----------------------: | :--------: | :--------------------:  |
| Disponível quando os fragmentos de conteúdo não estão selecionados | ✔ |  |  |  |
| Disponível quando um ou mais Fragmentos de conteúdo são selecionados |  | ✔ |  |  |
| Coleta uma entrada personalizada do usuário |  |  | ✔️ |  |
| Exibe feedback personalizado para o usuário |  |  | ✔️ |  |
| Invoca solicitações HTTP para AEM |  |  |  | ✔ |
| Invoca solicitações HTTP para serviços de Adobe/terceiros |  |  |  | ✔ |


## Documentação do Adobe Developer

O Adobe Developer contém detalhes do desenvolvedor sobre as extensões do Console de fragmentos de conteúdo AEM. Revise o [Conteúdo do Adobe Developer para obter mais detalhes técnicos](https://developer.adobe.com/uix/docs/).

## Desenvolver uma extensão

Siga as etapas descritas abaixo para saber como gerar, desenvolver e implantar uma extensão do Console do Fragmento de Conteúdo AEM para AEM as a Cloud Service.

<div class="columns is-multiline">
    <!-- Create Adobe Developer Project -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create Adobe Developer Project">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./adobe-developer-console-project.md" title="Criar projeto do Adobe Developer" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/project/card.png" alt="Criar projeto do Adobe Developer">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">1. Criar um projeto</p>
                    <p class="is-size-6">Crie um projeto do Adobe Developer Console que defina seu acesso a outros serviços da Adobe e gerencie suas implantações.</p>
                    <a href="./adobe-developer-console-project.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Criar um projeto do Adobe Developer</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Generate an Extension app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Generate an Extension app">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./app-initialization.md" title="Gerar um aplicativo de extensão" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/initialize-app/card.png" alt="Inicializar um aplicativo de extensão">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">2. Inicializar um aplicativo de extensão</p>
                    <p class="is-size-6">Inicialize um aplicativo Construtor de aplicativos da extensão Console do fragmento de conteúdo do AEM que defina onde a extensão é exibida e o trabalho que ela realiza.</p>
                    <a href="./app-initialization.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Inicializar um aplicativo de extensão</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension registration -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension registration">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./extension-registration.md" title="Registro de extensão" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/extension-registration/card.png" alt="Registro de extensão">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">3. Registro da extensão</p>
                    <p class="is-size-6">Registre a extensão no Console de fragmentos de conteúdo do AEM como um menu de cabeçalho ou tipo de extensão da barra de ação.</p>
                    <a href="./extension-registration.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Registrar a extensão</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Header Menu -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./header-menu.md" title="Menu de cabeçalho" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/header-menu/card.png" alt="Menu de cabeçalho">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4a. Menu de cabeçalho</p>
                    <p class="is-size-6">Saiba como criar uma extensão de menu do Console de fragmentos de conteúdo do AEM.</p>
                    <a href="./header-menu.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Estender o menu de cabeçalho</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Action Bar -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action Bar">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./action-bar.md" title="Barra de ações" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/action-bar/card.png" alt="Barra de ações">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4b. Barra de ações</p>
                    <p class="is-size-6">Saiba como criar uma extensão de barra de ação do Console de fragmentos de conteúdo do AEM.</p>
                    <a href="./action-bar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Estender a barra de ações</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Modal -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modal">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./modal.md" title="Modal" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/modal/card.png" alt="Modal">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">5. Modal</p>
                    <p class="is-size-6">Adicione um modal personalizado à extensão que pode ser usado para criar experiências personalizadas para os usuários. Os modais geralmente coletam informações de usuários e exibem os resultados de uma operação.</p>
                    <a href="./modal.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Adicionar um modal</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Adobe I/O Runtime action -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adobe I/O Runtime action">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./runtime-action.md" title="Ação do Adobe I/O Runtime" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/runtime-action/card.png" alt="Ação do Adobe I/O Runtime">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">6. Ação do Adobe I/O Runtime</p>
                    <p class="is-size-6">Adicione uma ação do Adobe I/O Runtime sem servidor que a extensão possa invocar para interagir com Fragmentos de conteúdo e AEM para executar operações de negócios personalizadas.</p>
                    <a href="./runtime-action.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Adicionar uma ação do Adobe I/O Runtime</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Test -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Test">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./test.md" title="Testar" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/test/card.png" alt="Testar">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">7. Ensaio</p>
                    <p class="is-size-6">Teste as extensões durante o desenvolvimento e o compartilhamento de extensões concluídas para testadores de controle de qualidade ou UAT usando um URL especial.</p>
                    <a href="./test.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Testar a extensão</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension deployment -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension deployment">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./deploy.md" title="Implantação de extensão" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/deploy/card.png" alt="Implantação de extensão">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">8. Implantação de produção</p>
                    <p class="is-size-6">Implante a extensão no Adobe I/O para disponibilizá-la aos usuários do AEM. As extensões também podem ser atualizadas e removidas.</p>
                    <a href="./deploy.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Implantar para produção</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
</div>

## Extensões de exemplo

Exemplo de extensões do Console de fragmentos de conteúdo do AEM.

<div class="columns is-multiline">
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/bulk-property-update.md" title="Extensão de atualização de propriedade em massa" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/bulk-property-update/card.png" alt="Extensão de atualização de propriedade em massa">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Extensão de atualização de propriedade em massa</p>
                    <p class="is-size-6">Explore um exemplo de extensão da barra de ação que atualiza em massa uma propriedade nos Fragmentos de conteúdo selecionados.</p>
                    <a href="./example-extensions/bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Explore a extensão de exemplo</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Image Generartion update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="OpenAI-based image generation and upload to AEM extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/image-generation-and-image-upload.md" title="Geração de imagens com base em OpenAI e upload para a extensão AEM" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/digital-image-generation/screenshot.png" alt="Geração de imagens com base em OpenAI e upload para a extensão AEM">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Geração de imagens com base em OpenAI e upload para a extensão AEM</p>
                    <p class="is-size-6">Explore um exemplo de extensão da barra de ação que gera uma imagem usando OpenAI, faz o upload para AEM e atualiza a propriedade da imagem no Fragmento de conteúdo selecionado.</p>
                    <a href="./example-extensions/image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Explore a extensão de exemplo</span>
                    </a>
                </div>
            </div>
        </div>
    </div>



</div>
