---
title: Capítulo 3 - Criação dos fragmentos de conteúdo do evento - Serviços de conteúdo
description: O capítulo 3 do tutorial AEM headless abrange a criação e a criação de fragmentos de conteúdo de evento do modelo de fragmento de conteúdo criado no capítulo 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
duration: 196
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---

# Capítulo 3 - Criação de fragmentos de conteúdo de evento

O capítulo 3 do tutorial do AEM headless abrange a criação e a criação de eventos e fragmentos de conteúdo do modelo de fragmento de conteúdo criado no [Capítulo 2](./chapter-2.md).

## Criação de um fragmento de conteúdo de evento

Com um [!DNL Event] Modelo de fragmento de conteúdo criado e configuração do AEM para WKND aplicada ao `/content/dam/wknd-mobile` Pasta de ativos (por meio da `cq:conf` propriedade), uma [!DNL Event] O fragmento de conteúdo pode ser criado.

Os fragmentos de conteúdo, que são um tipo de ativo, devem ser organizados e gerenciados no AEM Assets da mesma forma que outros ativos.

* Usar pastas de localidade na estrutura de pastas do Assets se a tradução for (ou puder ser) necessária
* Organize logicamente os fragmentos de conteúdo para facilitar a localização e o gerenciamento

Nesta etapa, crie um novo [!DNL Event] para `Punkrock Fest` no `/content/dam/wknd-mobile/en/events` pasta de ativos.

1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Arquivos] > [!DNL WKND Mobile] >[!DNL English]** e criar pastas de ativos **[!DNL Events]**.
1. Dentro de **[!UICONTROL Assets] > [!UICONTROL Arquivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** criar um novo Fragmento de conteúdo do tipo **[!DNL Event]** com um título de **[!DNL Punkrock Fest]**.
1. Criar o recém-criado [!DNL Event] Fragmento do conteúdo.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description...=&quot;&quot;>**
   * [!DNL Event Date] : **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **Música**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **A casa réptil**
   * [!DNL Venue City] : **Nova York**

   Toque **[!UICONTROL Salvar]** na barra de ações superior para salvar as alterações.

1. Usar [Gerenciador de pacotes AEM](http://localhost:4502/crx/packmgr/index.jsp), instale o pacote abaixo no AEM Author. Esse pacote contém vários Fragmentos de conteúdo de evento.

   [Obter arquivo: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## Revisão da estrutura JCR do fragmento de conteúdo

*Esta seção é apenas informativa e destina-se a socializar a estrutura JCR subjacente de fragmentos de conteúdo feitos de modelos de fragmento de conteúdo.*

1. Abertura **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** sobre o AEM Author.
1. No CRXDE Lite, no menu de hierarquia à esquerda, navegue até [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) que é o nó que representa o [!DNL Punkrock Fest] [!DNL Event] Fragmento de conteúdo no JCR.
1. Expanda a [dados](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) nó.
Revisão no **Painel Propriedades** que possui uma propriedade `cq:model` que aponta para o [!DNL Event] Definição do modelo de fragmento de conteúdo.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Abaixo de `data` nó selecione o [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) e revise as propriedades. Este nó contém o conteúdo coletado durante a criação de um [!DNL Event] Modelo de fragmento de conteúdo. Os nomes de propriedade do JCR correspondem aos nomes de propriedade do modelo de fragmento de conteúdo, e os valores correspondem aos valores criados do &quot;[!DNL Punkrock Fest]&quot; [!DNL Event] Fragmento do conteúdo.

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## Próxima etapa

É recomendável instalar o [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) pacote de conteúdo no AEM Author via [AEM [!UICONTROL Gerenciador de pacotes]](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descritos neste e nos capítulos anteriores do tutorial.

* [Capítulo 4 - Definição de modelos de serviços de conteúdo do AEM](./chapter-4.md)
