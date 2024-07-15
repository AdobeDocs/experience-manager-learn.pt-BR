---
title: Capítulo 5 - Criação de páginas dos serviços de conteúdo - Serviços de conteúdo
description: O capítulo 5 do tutorial do AEM headless aborda a criação de páginas a partir dos modelos definidos no Capítulo 4. Essas páginas atuarão como pontos de extremidade HTTP JSON.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
duration: 189
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '570'
ht-degree: 0%

---

# Capítulo 5 - Criação de páginas dos serviços de conteúdo

O capítulo 5 do tutorial sem periféricos do AEM aborda a criação da página a partir dos modelos definidos no Capítulo 4. A página criada neste capítulo atuará como o ponto de extremidade HTTP JSON para o aplicativo móvel.

>[!NOTE]
>
> A arquitetura de conteúdo da página de `/content/wknd-mobile/en/api` foi pré-criada. As páginas base de `en` e `api` atendem a uma finalidade arquitetônica e organizacional, mas não são estritamente necessárias. Se o conteúdo da API puder ser localizado, é prática recomendada seguir as práticas recomendadas habituais da organização de Cópia de idioma e Página de vários sites, já que a página da API pode ser localizada como qualquer uma das páginas do AEM Sites.

## Criando a página API de evento

1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Toque no rótulo da página API** e toque no botão **Criar** na barra de ações superior e crie uma nova página API de Eventos na página API.
   1. Toque em **Criar** na barra de ação superior
   1. Selecionar modelo de **API de Eventos**
   1. No campo **Nome**, digite **eventos**
   1. No campo **Título**, digite **API de Eventos**
   1. Toque em **Criar** na barra de ações superior para criar a página
   1. Toque em **Concluído** para retornar ao administrador do AEM Sites

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## Criação da página da API de eventos

>[!NOTE]
>
> O projeto fornece CSS para fornecer alguns estilos básicos à experiência do autor.

1. Edite a página **API de Eventos** navegando até **AEM > Sites > WKND Mobile > Inglês > API**, selecionando a página **API de Eventos** e tocando em **Editar** na barra de ação superior.
1. Adicione uma **imagem de logotipo** para exibir no aplicativo, arrastando-a e soltando-a do Localizador de ativos no espaço reservado do componente Imagem.
   * Use o logotipo fornecido em `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Adicione a **linha da marca** para exibição acima dos eventos.
   1. Editar o componente **Texto**
   1. Insira o texto:
      * `The WKND is here.`

1. Selecione os **eventos** a serem exibidos:
   1. Defina a seguinte configuração na guia **Propriedades**:
      * Modelo: **Evento**
      * Caminho pai: **/content/dam/wknd-mobile/en/events**
      * Marcas: **&lt;Deixe em branco>**
   1. Defina a seguinte configuração na guia **Elementos**:
      * Remova todos os elementos listados, para garantir que TODOS os elementos dos Fragmentos de conteúdo do evento sejam expostos.

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## Revise a saída JSON da página da API

A saída JSON e seu formato podem ser revisados ao solicitar a Página com o seletor `.model.json`.

Essa estrutura JSON (ou esquema) deve ser bem compreendida pelos consumidores dessa API. É essencial que os consumidores de API entendam quais aspectos da estrutura são fixos (ou seja, o Logotipo da API de evento (Imagem) e a Tag ativa (Texto), que são fluidos (ou seja, os eventos listados em Componente de lista do fragmento de conteúdo).

Romper este contrato em uma API publicada pode resultar em comportamento incorreto no consumo de aplicativos.

1. Em novas guias do navegador, solicite as páginas da API de Eventos usando o seletor `.model.json`, que chama o Exportador JSON do AEM Content Services, e serializa a Página e os Componentes em uma estrutura JSON normalizada e bem definida.

   A estrutura JSON produzida por essas páginas é a estrutura que os aplicativos que consomem devem se alinhar.

1. Solicite a página **API de Eventos** como **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   O resultado deve ser semelhante a:

![Saída JSON do AEM Content Services](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Este JSON pode ser emitido em um modo **organizado** (formatado) para legibilidade humana usando o seletor `.tidy`:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

## Próxima etapa

Opcionalmente, instale o pacote de conteúdo do [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) no AEM Author via [Gerenciador de Pacotes do AEM](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descritos neste e nos capítulos anteriores do tutorial.

* [Capítulo 6 - Exposição do conteúdo no AEM Publish como JSON](./chapter-6.md)
