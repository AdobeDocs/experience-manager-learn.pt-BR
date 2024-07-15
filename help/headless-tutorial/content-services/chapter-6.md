---
title: Capítulo 6 - Exposição do conteúdo no AEM Publish como JSON - Content Services
description: O capítulo 6 do tutorial AEM headless abrange garantir que todos os pacotes, a configuração e o conteúdo necessários estejam no AEM Publish para permitir o consumo do aplicativo móvel.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
duration: 196
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# Capítulo 6 - Expor o conteúdo no AEM Publish para entrega

O capítulo 6 do tutorial AEM headless abrange garantir que todos os pacotes, a configuração e o conteúdo necessários estejam no AEM Publish para permitir o consumo pelo aplicativo móvel.

## Publicação de conteúdo para o AEM Content Services

A configuração e o conteúdo criados para orientar os eventos por meio do AEM Content Services devem ser publicados no AEM Publish para que o aplicativo móvel possa acessá-los.

Como os Serviços de conteúdo AEM são criados a partir de Configuração (modelos de fragmento de conteúdo, modelos editáveis), Assets (fragmentos de conteúdo, imagens) e Páginas, todas essas partes desfrutam automaticamente dos recursos de gerenciamento de conteúdo do AEM, incluindo:

* Fluxo de trabalho para revisão e processamento
* e ativação/desativação para enviar e receber conteúdo dos pontos de acesso do AEM Content Services do Publish do AEM

1. Verifique se os **[!DNL WKND Mobile]Pacotes de Aplicativos**, listados no [Capítulo 1](./chapter-1.md#wknd-mobile-application-packages), estão instalados no **AEM Publish** usando o [!UICONTROL Gerenciador de Pacotes].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publish o **[!DNL WKND Mobile Events API]Modelo Editável**
   1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Ferramentas] > [!UICONTROL Geral] > [!UICONTROL Modelos] >[!DNL WKND Mobile]**
   1. Selecione o modelo **[!DNL Event API]**
   1. Toque em **[!UICONTROL Publish]** na barra de ação superior
   1. O **modelo** e **todas as referências** do Publish (políticas de conteúdo, mapeamentos de políticas de conteúdo e modelos)

1. Publish os **[!DNL WKND Mobile Events]fragmentos de conteúdo**.

   Observe que isso é necessário, pois a API de eventos usa o componente Lista de fragmentos de conteúdo, que não faz referência específica aos fragmentos de conteúdo.

   1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Arquivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. Selecionar todos os **[!DNL Event]** fragmentos de conteúdo
   1. Toque em **[!UICONTROL Gerenciar publicação]** na barra de ação superior
   1. Deixando a ação padrão **Publish** como está, toque em **[!UICONTROL Avançar]** na barra de ação superior
   1. Selecionar **todos** fragmentos de conteúdo
   1. Toque em **[!UICONTROL Publish]** na barra de ação superior
      * *O Modelo de Fragmento de Conteúdo do [!DNL Events] e as Imagens de Eventos de referência serão publicados automaticamente junto com os fragmentos de conteúdo.*

1. Publish a **[!DNL Events API]página**.
   1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Selecione a página **[!DNL Events]**
   1. Toque em **[!UICONTROL Gerenciar publicação]** na barra de ação superior
   1. Deixando a ação padrão **Publish** como está, toque em **[!UICONTROL Avançar]** na barra de ação superior
   1. Selecione a página **[!DNL Events]**
   1. Toque em **[!DNL Publish]** na barra de ação superior

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## Verificar o AEM Publish

1. Em um novo navegador da Web, verifique se você está desconectado do AEM Publish e solicite as seguintes URLs (substituindo `http://localhost:4503` por qualquer host:porta em que o AEM Publish esteja em execução).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)

   Essas solicitações devem retornar a mesma resposta JSON de quando os pontos de extremidade do autor do AEM correspondentes foram revisados. Caso contrário, verifique se todas as publicações foram bem-sucedidas (verifique as filas de Replicação), se o pacote [!DNL WKND Mobile] `ui.apps` está instalado no AEM Publish e revise o `error.log` for AEM Publish.

## Próxima etapa

Não há pacotes extras a serem instalados. Certifique-se de que o conteúdo e a configuração descritos nesta seção sejam publicados no AEM Publish; caso contrário, os capítulos subsequentes não funcionarão.

* [Capítulo 7 - Consumir serviços de conteúdo de AEM de um aplicativo móvel](./chapter-7.md)
