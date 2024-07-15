---
title: Capítulo 3 - Criação dos fragmentos de conteúdo do evento - Serviços de conteúdo
description: O capítulo 3 do tutorial AEM headless abrange a criação e a criação de fragmentos de conteúdo de evento do modelo de fragmento de conteúdo criado no capítulo 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
duration: 167
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---

# Capítulo 3 - Criação de fragmentos de conteúdo de evento

O Capítulo 3 do tutorial AEM Headless aborda a criação e a criação de Fragmentos de conteúdo de eventos a partir do modelo de fragmento de conteúdo criado no [Capítulo 2](./chapter-2.md).

## Criação de um fragmento de conteúdo de evento

Com um Modelo de fragmento de conteúdo [!DNL Event] criado e a Configuração do AEM para WKND aplicada à pasta de ativos `/content/dam/wknd-mobile` (por meio da propriedade `cq:conf`), um Fragmento de conteúdo [!DNL Event] pode ser criado.

Os fragmentos de conteúdo, que são um tipo de ativo, devem ser organizados e gerenciados no AEM Assets da mesma forma que outros ativos.

* Usar pastas de local na estrutura de pastas do Assets se a tradução for (ou puder ser) necessária
* Organize logicamente os fragmentos de conteúdo para facilitar a localização e o gerenciamento

Nesta etapa, crie um novo [!DNL Event] para `Punkrock Fest` na pasta de ativos `/content/dam/wknd-mobile/en/events`.

1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Arquivos] > [!DNL WKND Mobile] >[!DNL English]** e crie pastas de Ativos **[!DNL Events]**.
1. Em **[!UICONTROL Assets] > [!UICONTROL Arquivos] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**, crie um novo Fragmento de Conteúdo do tipo **[!DNL Event]** com um título de **[!DNL Punkrock Fest]**.
1. Crie o fragmento de conteúdo [!DNL Event] recém-criado.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;Insira algumas linhas de descrição...>**
   * [!DNL Event Date] : **&lt;Selecione uma data no futuro>**
   * [!DNL Event Type] : **Música**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **A Casa de Répteis**
   * [!DNL Venue City] : **Nova York**

   Toque em **[!UICONTROL Salvar]** na barra de ações superior para salvar as alterações.

1. Usando o [Gerenciador de Pacotes do AEM](http://localhost:4502/crx/packmgr/index.jsp), instale o pacote abaixo no AEM Author. Esse pacote contém vários Fragmentos de conteúdo de evento.

   [Obter Arquivo: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## Revisão da estrutura JCR do fragmento de conteúdo

*Esta seção é apenas para fins informativos e destina-se a socializar a estrutura JCR subjacente de Fragmentos de Conteúdo produzidos a partir de Modelos de Fragmento de Conteúdo.*

1. Abra **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** no AEM Author.
1. No CRXDE Lite, no menu de hierarquia à esquerda, navegue até [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content), que é o nó que representa o Fragmento de conteúdo [!DNL Punkrock Fest] [!DNL Event] no JCR.
1. Expanda o nó [dados](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master).
No painel **Propriedades**, verifique se ele tem uma propriedade `cq:model` que aponta para a definição do Modelo de fragmento de conteúdo [!DNL Event].
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Abaixo do nó `data`, selecione o nó [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) e revise as propriedades. Este nó contém o conteúdo coletado durante a criação de um Modelo de fragmento de conteúdo [!DNL Event]. Os nomes de propriedades do JCR correspondem aos nomes de propriedades do modelo de fragmento de conteúdo, e os valores correspondem aos valores criados do fragmento de conteúdo &quot;[!DNL Punkrock Fest]&quot; [!DNL Event].

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## Próxima etapa

É recomendável instalar o pacote de conteúdo do [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) no AEM Author por meio do [Gerenciador de Pacotes ](http://localhost:4502/crx/packmgr/index.jsp) do AEM. Este pacote contém as configurações e o conteúdo descritos neste e nos capítulos anteriores do tutorial.

* [Capítulo 4 - Definição de modelos de serviços de conteúdo do AEM](./chapter-4.md)
