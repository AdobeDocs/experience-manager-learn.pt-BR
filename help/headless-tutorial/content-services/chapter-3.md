---
title: Capítulo 3 - Criação de fragmentos de conteúdo do evento - Serviços de conteúdo
seo-title: Getting Started with AEM Content Services - Chapter 3 - Authoring Event Content Fragments
description: O Capítulo 3 do tutorial AEM Headless cobre a criação e criação de Fragmentos de conteúdo de evento do Modelo de fragmento de conteúdo criado no Capítulo 2.
seo-description: Chapter 3 of the AEM Headless tutorial covers creating and authoring Event Content Fragments from the Content Fragment Model created in Chapter 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 3%

---

# Capítulo 3 - Criação de fragmentos de conteúdo do evento

O Capítulo 3 do tutorial AEM Headless cobre a criação e criação de Fragmentos de conteúdo de eventos a partir do Modelo de fragmento de conteúdo criado em [Capítulo 2](./chapter-2.md).

## Criação de um fragmento de conteúdo de evento

Com um [!DNL Event] Modelo de fragmento de conteúdo criado e a Configuração de AEM para WKND aplicada ao `/content/dam/wknd-mobile` Pasta Ativo (por meio da `cq:conf` propriedade), a [!DNL Event] Fragmento de conteúdo pode ser criado.

Os Fragmentos de conteúdo, que são um tipo de Ativo, devem ser organizados e gerenciados no AEM Assets da mesma forma que os outros ativos.

* Usar pastas de localidade na estrutura da pasta Ativos se a tradução for (ou puder ser) necessária
* Organize logicamente os Fragmentos de conteúdo para que eles sejam fáceis de localizar e gerenciar

Nesta etapa, crie um novo [!DNL Event] para `Punkrock Fest` no `/content/dam/wknd-mobile/en/events` pasta de ativos.

1. Navegar para **[!UICONTROL AEM] > [!UICONTROL Ativos] > [!UICONTROL Arquivos] > [!DNL WKND Mobile] >[!DNL English]** e criar pastas de ativos **[!DNL Events]**.
1. Within **[!UICONTROL Ativos] > [!UICONTROL Arquivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** criar um novo Fragmento de conteúdo do tipo **[!DNL Event]** com um título de **[!DNL Punkrock Fest]**.
1. Crie o recém-criado [!DNL Event] Fragmento do conteúdo.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description...=&quot;&quot;>**
   * [!DNL Event Date] : **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **Música**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **A Câmara dos Deputados**
   * [!DNL Venue City] : **Nova York**

   Toque **[!UICONTROL Salvar]** na barra de ação superior para salvar as alterações.

1. Usando [Gerenciador de pacotes de AEM](http://localhost:4502/crx/packmgr/index.jsp), instale o pacote abaixo no AEM Author. Este pacote contém vários Fragmentos do conteúdo do evento.

   [Obter arquivo: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## Revisão da estrutura JCR do Fragmento de conteúdo

*Esta seção é apenas informativa e destina-se a socializar a estrutura JCR subjacente dos Fragmentos de conteúdo criados nos Modelos de fragmento de conteúdo.*

1. Abrir **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** no AEM Author.
1. No CRXDE Lite, no menu de hierarquia à esquerda, navegue até [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) que é o nó que representa a variável [!DNL Punkrock Fest] [!DNL Event] Fragmento de conteúdo no JCR.
1. Expanda o [dados](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) nó .
Revisão no **Painel Propriedades** que tem uma propriedade `cq:model` que aponta para a [!DNL Event] Definição do modelo de fragmento de conteúdo.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Abaixo da `data` nó selecione o [principal](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) e revise as propriedades. Este nó contém o conteúdo coletado durante a criação de um [!DNL Event] Modelo de fragmento de conteúdo. Os nomes de propriedades do JCR correspondem aos nomes de propriedades do Modelo do fragmento de conteúdo e os valores correspondem aos valores criados do &quot;[!DNL Punkrock Fest]&quot; [!DNL Event] Fragmento do conteúdo.

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## Próxima etapa

É recomendável instalar o [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) pacote de conteúdo no AEM Author via [AEM [!UICONTROL Gerenciador de pacotes]](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descrito neste e nos capítulos anteriores do tutorial.

* [Capítulo 4 - Definição de modelos de serviços de conteúdo AEM](./chapter-4.md)
