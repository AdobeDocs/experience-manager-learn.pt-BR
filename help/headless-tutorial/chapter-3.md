---
title: Capítulo 3 - Criação de fragmentos de conteúdo do Evento
seo-title: Introdução ao AEM Content Services - Capítulo 3 - Criação de fragmentos de conteúdo do Evento
description: O Capítulo 3 do tutorial AEM sem cabeçalho aborda a criação e criação de Fragmentos de conteúdo de Evento a partir do Modelo de fragmento de conteúdo criado no Capítulo 2.
seo-description: O Capítulo 3 do tutorial AEM sem cabeçalho aborda a criação e criação de Fragmentos de conteúdo de Evento a partir do Modelo de fragmento de conteúdo criado no Capítulo 2.
translation-type: tm+mt
source-git-commit: d7258f8acf6df680795ce61cc8383e60b5b7d722
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 1%

---


# Capítulo 3 - Criação de fragmentos de conteúdo do Evento

O Capítulo 3 do tutorial AEM sem cabeçalho aborda a criação e criação de Eventos Fragmentos de conteúdo do Modelo de fragmento de conteúdo criado em [Capítulo 2](./chapter-2.md).

## Criação de um fragmento de conteúdo de Evento

Com um [!DNL Event] Modelo de fragmento de conteúdo criado e a Configuração AEM para WKND aplicada à pasta `/content/dam/wknd-mobile` Asset (por meio da propriedade `cq:conf`), um [!DNL Event] Fragmento de conteúdo pode ser criado.

Fragmentos de conteúdo, que são um tipo de ativo, devem ser organizados e gerenciados no AEM Assets, assim como em outros ativos.

* Usar pastas de localidade na estrutura da pasta Ativos se a tradução for (ou puder ser) necessária
* Organize logicamente os Fragmentos de conteúdo para que eles sejam fáceis de localizar e gerenciar

Nesta etapa, crie um novo [!DNL Event] para `Punkrock Fest` na pasta `/content/dam/wknd-mobile/en/events` assets.

1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Ativos] > [!UICONTROL Arquivos] > [!DNL WKND Mobile] >[!DNL English]** e crie pastas de Ativos **[!DNL Events]**.
1. Em **[!UICONTROL Assets] > [!UICONTROL Arquivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**, crie um novo Fragmento de conteúdo do tipo **[!DNL Event]** com um título de **[!DNL Punkrock Fest]**.
1. Crie o [!DNL Event] Fragmento de conteúdo recém-criado.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] :  **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description=&quot;&quot;>**
   * [!DNL Event Date] :  **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] :  **Música**
   * [!DNL Ticket Price] :  **10**
   * [!DNL Event Image] :  **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] :  **A Casa dos Répteis**
   * [!DNL Venue City] : **Nova York**

   Toque em **[!UICONTROL Salvar]** na barra de ação superior para salvar as alterações.

1. Usando [AEM Gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp), instale o pacote abaixo no Autor de AEM. Este pacote contém vários Fragmentos de conteúdo de Evento.

   [Obter arquivo: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## Revisão da estrutura JCR do fragmento de conteúdo

*Esta seção é apenas informativa e destina-se a socializar a estrutura JCR subjacente de Fragmentos de conteúdo feitos de Modelos de fragmento de conteúdo.*

1. Abra **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** no autor de AEM.
1. No CRXDE Lite, no menu de hierarquia à esquerda, navegue até [/content/dam/wknd-mobile/en/eventos/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content), que é o nó que representa o [!DNL Punkrock Fest] [!DNL Event] Fragmento do conteúdo no JCR.
1. Expanda o nó [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master).
Verifique no **painel Propriedades** se ele tem uma propriedade `cq:model` que aponta para a definição [!DNL Event] do Modelo de fragmento de conteúdo.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Abaixo do nó `data`, selecione o nó [principal](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) e reveja as propriedades. Este nó contém o conteúdo coletado durante a criação de um [!DNL Event] Modelo de fragmento de conteúdo. Os nomes das propriedades do JCR correspondem aos nomes das propriedades do Modelo de fragmento de conteúdo e os valores correspondem aos valores criados do Fragmento de conteúdo &quot;[!DNL Punkrock Fest]&quot; [!DNL Event].

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## Próxima etapa

É recomendável instalar o pacote de conteúdo [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) no AEM Author via [AEM [!UICONTROL Gerenciador de pacotes]](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descrito neste e nos capítulos anteriores do tutorial.

* [Capítulo 4 - Definição de modelos AEM Content Services](./chapter-4.md)
