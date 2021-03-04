---
title: Capítulo 3 - Criação de fragmentos de conteúdo do evento - Serviços de conteúdo
seo-title: Introdução aos serviços de conteúdo do AEM - Capítulo 3 - Criação dos fragmentos de conteúdo do evento
description: O Capítulo 3 do tutorial AEM Headless aborda a criação e criação de Fragmentos de conteúdo de evento do Modelo de fragmento de conteúdo criado no Capítulo 2.
seo-description: O Capítulo 3 do tutorial AEM Headless aborda a criação e criação de Fragmentos de conteúdo de evento do Modelo de fragmento de conteúdo criado no Capítulo 2.
feature: Fragmentos de conteúdo, APIs
topic: Sem periféricos, gerenciamento de conteúdo
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 3%

---


# Capítulo 3 - Criação de fragmentos de conteúdo do evento

O Capítulo 3 do tutorial AEM Headless cobre a criação e criação de Fragmentos de conteúdo de eventos a partir do Modelo de fragmento de conteúdo criado em [Capítulo 2](./chapter-2.md).

## Criação de um fragmento de conteúdo de evento

Com um [!DNL Event] Modelo de fragmento de conteúdo criado e a Configuração do AEM para WKND aplicada à pasta `/content/dam/wknd-mobile` Ativo (por meio da propriedade `cq:conf`), um [!DNL Event] Fragmento de conteúdo pode ser criado.

Os Fragmentos de conteúdo, que são um tipo de Ativo, devem ser organizados e gerenciados no AEM Assets da mesma forma que outros ativos.

* Usar pastas de localidade na estrutura da pasta Ativos se a tradução for (ou puder ser) necessária
* Organize logicamente os Fragmentos de conteúdo para que eles sejam fáceis de localizar e gerenciar

Nesta etapa, crie um novo [!DNL Event] para `Punkrock Fest` na pasta `/content/dam/wknd-mobile/en/events` de ativos.

1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Arquivos] > [!DNL WKND Mobile] >[!DNL English]** e crie pastas de Ativos **[!DNL Events]**.
1. Em **[!UICONTROL Assets] > [!UICONTROL Arquivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** crie um novo Fragmento de conteúdo do tipo **[!DNL Event]** com um título **[!DNL Punkrock Fest]**.
1. Crie o [!DNL Event] Fragmento de conteúdo recém-criado.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] :  **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description=&quot;&quot;>**
   * [!DNL Event Date] :  **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] :  **Música**
   * [!DNL Ticket Price] :  **10º**
   * [!DNL Event Image] :  **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] :  **A Câmara dos Deputados**
   * [!DNL Venue City] : **Nova York**

   Toque em **[!UICONTROL Salvar]** na barra de ações superior para salvar as alterações.

1. Usando o [Gerenciador de pacotes do AEM](http://localhost:4502/crx/packmgr/index.jsp), instale o pacote abaixo no AEM Author. Este pacote contém vários Fragmentos do conteúdo do evento.

   [Obter arquivo: GitHub > Ativos > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## Revisão da estrutura JCR do Fragmento de conteúdo

*Esta seção é apenas informativa e destina-se a socializar a estrutura JCR subjacente dos Fragmentos de conteúdo criados nos Modelos de fragmento de conteúdo.*

1. Abra **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** no Autor do AEM.
1. No CRXDE Lite, no menu de hierarquia à esquerda, navegue até [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) que é o nó que representa o [!DNL Punkrock Fest] [!DNL Event] Fragmento de conteúdo no JCR.
1. Expanda o nó [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master).
Verifique no **painel Propriedades** se ele tem uma propriedade `cq:model` que aponta para a definição [!DNL Event] do Modelo de fragmento de conteúdo.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Abaixo do nó `data`, selecione o nó [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) e revise as propriedades. Este nó contém o conteúdo coletado durante a criação de um [!DNL Event] Modelo de fragmento de conteúdo. Os nomes de propriedades do JCR correspondem aos nomes de propriedade do Modelo do fragmento de conteúdo e os valores correspondem aos valores criados do Fragmento de conteúdo &quot;[!DNL Punkrock Fest]&quot; [!DNL Event].

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## Próxima etapa

É recomendável instalar o pacote de conteúdo [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) no AEM Author via [Gerenciador de pacotes do AEM [!UICONTROL Gerenciador de pacotes]](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descrito neste e nos capítulos anteriores do tutorial.

* [Capítulo 4 - Definição de modelos de serviços de conteúdo do AEM](./chapter-4.md)
