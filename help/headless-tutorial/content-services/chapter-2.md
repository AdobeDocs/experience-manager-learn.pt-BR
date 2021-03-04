---
title: Capítulo 2 - Definição dos modelos de fragmento do conteúdo do evento - Serviços de conteúdo
seo-title: Introdução aos serviços de conteúdo do AEM - Capítulo 2 - Definição dos modelos de fragmento do conteúdo do evento
description: O Capítulo 2 do tutorial AEM Headless abrange a ativação e definição dos Modelos de fragmento de conteúdo usados para definir uma estrutura de dados normalizada e uma interface de criação para criar Eventos.
seo-description: O Capítulo 2 do tutorial AEM Headless abrange a ativação e definição dos Modelos de fragmento de conteúdo usados para definir uma estrutura de dados normalizada e uma interface de criação para criar Eventos.
feature: Fragmentos de conteúdo, APIs
topic: Sem periféricos, gerenciamento de conteúdo
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1004'
ht-degree: 7%

---


# Capítulo 2 - Uso de modelos de fragmento de conteúdo

Os Modelos de fragmento de conteúdo do AEM definem esquemas de conteúdo que podem ser usados para modelar a criação de conteúdo bruto por autores do AEM. Essa abordagem é semelhante ao scaffolding ou à criação baseada em formulários. O conceito principal com Fragmentos de conteúdo é que o conteúdo criado é independente de apresentação, o que significa que ele se destina ao uso de vários canais, onde o aplicativo de consumo, seja o AEM, um aplicativo de página única ou um aplicativo móvel, controla como o conteúdo é exibido ao usuário.

A principal preocupação do Fragmento de conteúdo é garantir:

1. O conteúdo correto é coletado do autor
2. O conteúdo pode ser exposto em um formato estruturado e bem compreendido ao consumo de aplicativos.

Este capítulo aborda a ativação e definição dos Modelos de fragmento de conteúdo usados para definir uma estrutura de dados normalizada e uma interface de criação para modelagem e criação de &quot;Eventos&quot;.

## Ativar modelos de fragmento de conteúdo

Os Modelos de fragmento de conteúdo **devem** ser ativados por meio de **[Navegador de configuração [!UICONTROL do AEM]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html)**.

Se os Modelos de fragmento de conteúdo forem **not** ativados para uma configuração, o botão **[!UICONTROL Criar] > [!UICONTROL Fragmento de conteúdo]** não aparecerá para a configuração relevante do AEM.

>[!NOTE]
>
>As configurações do AEM representam um conjunto de [configurações de locatários com reconhecimento de contexto](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) armazenadas em `/conf`. Normalmente, as configurações do AEM se correlacionam a um site específico gerenciado no AEM Sites ou a uma unidade comercial responsável por um subconjunto de conteúdo (ativos, páginas etc.) no AEM.
>
>Para que uma configuração afete uma hierarquia de conteúdo, a configuração deve ser referenciada por meio da propriedade `cq:conf` nessa hierarquia de conteúdo. (Isso é obtido para a configuração [!DNL WKND Mobile] em **Etapa 5** abaixo).
>
>Quando a configuração `global` é usada, a configuração se aplica a todo o conteúdo e `cq:conf` não precisa ser definida.
>
>Consulte a documentação [[!UICONTROL Navegador de configuração]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html) para obter mais informações.

1. Faça logon no AEM Author como um usuário com as permissões apropriadas para modificar a configuração relevante.
   * Para este tutorial, o usuário **admin** pode ser usado.
1. Navegue até **[!UICONTROL Ferramenta] > [!UICONTROL Geral] > [!UICONTROL Navegador de Configuração]**
1. Toque no **ícone de pasta** ao lado de **[!DNL WKND Mobile]** para selecionar e toque no botão **[!UICONTROL Editar]** no canto superior esquerdo.
1. Selecione **[!UICONTROL Modelos de fragmento de conteúdo]** e toque em **[!UICONTROL Salvar e fechar]** no canto superior direito.

   Isso ativa os Modelos de fragmento de conteúdo nas árvores de conteúdo da pasta de ativos com a configuração [!DNL WKND Mobile] aplicada.

   >[!NOTE]
   >
   >Essa alteração de configuração não é reversível na interface do usuário da Web [!UICONTROL AEM Configuration]. Para desfazer essa configuração:
   >    
   >    1. Abra [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Vá até `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Exclua o nó `models`

   >    
   >Quaisquer Modelos de fragmento de conteúdo existentes criados nessa configuração serão excluídos, bem como suas definições serão armazenadas em `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Aplique a configuração **[!DNL WKND Mobile]** à pasta **[!DNL WKND Mobile]Ativos** para permitir que os Fragmentos de conteúdo dos Modelos de fragmento de conteúdo sejam criados na hierarquia da pasta Ativos:

   1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Arquivos]**
   1. Selecione a pasta **[!UICONTROL WKND Mobile]**
   1. Toque no botão **[!UICONTROL Propriedades]** na barra de ação superior para abrir [!UICONTROL Propriedades da pasta]
   1. Em [!UICONTROL Propriedades da pasta], toque na guia **[!UICONTROL Serviços da nuvem]**
   1. Verifique se o campo **[!UICONTROL Cloud Configuration]** está definido como **/conf/wknd-mobile**
   1. Toque em **[!UICONTROL Salvar e fechar]** no canto superior direito para persistir as alterações

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

## Noções básicas do modelo de fragmento de conteúdo para criar

Antes de definir o modelo de Fragmento de conteúdo, vamos rever a experiência que estaremos conduzindo para garantir que estamos capturando todos os pontos de dados necessários. Para isso, analisaremos o design dos aplicativos móveis e mapearemos os elementos do design para o conteúdo coletado.

Podemos dividir os pontos de dados que definem um Evento da seguinte maneira:

![Criação do modelo de fragmento de conteúdo](assets/chapter-2/design-to-model-mapping.png)

Munido do mapeamento, podemos definir o Fragmento do conteúdo que será usado para coletar e, em última análise, expor os dados do Evento.

## Criação do modelo de fragmento de conteúdo

1. Navegue até **[!UICONTROL Ferramentas] > [!UICONTROL Ativos] > [!UICONTROL Modelos de fragmento de conteúdo]**.
1. Toque na pasta **[!DNL WKND Mobile]** para abrir.
1. Toque em **[!UICONTROL Criar]** para abrir o assistente de criação do Modelo de fragmento de conteúdo.
1. Insira **[!DNL Event]** como o **[!UICONTROL Título do Modelo]** *(a descrição é opcional)* e toque em **[!UICONTROL Criar]** para salvar.

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## Definição da estrutura do modelo de fragmento de conteúdo

1. Navegue até **[!UICONTROL Ferramentas] > [!UICONTROL Ativos] > [!UICONTROL Modelos de fragmento de conteúdo] >[!DNL WKND]**.
1. Selecione o **[!DNL Event]** Modelo do fragmento de conteúdo e toque em **[!UICONTROL Editar]** na barra de ação superior.
1. Na guia **[!UICONTROL Data Types]** à direita, arraste **[!UICONTROL Single line text input]** para a zona suspensa esquerda para definir o campo **[!DNL Question]**.
1. Verifique se a nova **[!UICONTROL Single line text input]** está selecionada à esquerda e se a **[!UICONTROL Properties] tab** está selecionada à direita. Preencha os campos Propriedades da seguinte maneira:

   * [!UICONTROL Renderizar como] : `textfield`
   * [!UICONTROL Rótulo do campo] : `Event Title`
   * [!UICONTROL Nome da Propriedade] : `eventTitle`
   * [!UICONTROL Extensão]  Máx.: 25.
   * [!UICONTROL Obrigatório] : `Yes`

Repita essas etapas usando as definições de entrada definidas abaixo para criar o restante do Modelo de fragmento de conteúdo do evento.

>[!NOTE]
>
> Os campos **Nome da propriedade** DEVEM corresponder exatamente, pois o aplicativo Android é programado para destacar esses nomes.

### Descrição de evento

* [!UICONTROL Tipo de dados] : `Multi-line text`
* [!UICONTROL Rótulo do campo] : `Event Description`
* [!UICONTROL Nome da Propriedade] : `eventDescription`
* [!UICONTROL Tipo padrão] : `Rich text`

### Data e hora do evento

* [!UICONTROL Tipo de dados] : `Date and time`
* [!UICONTROL Rótulo do campo] : `Event Date and Time`
* [!UICONTROL Nome da Propriedade] : `eventDateAndTime`
* [!UICONTROL Obrigatório] : `Yes`

### Tipo de evento

* [!UICONTROL Tipo de dados] : `Enumeration`
* [!UICONTROL Rótulo do campo] : `Event Type`
* [!UICONTROL Nome da Propriedade] : `eventType`
* [!UICONTROL Opções] :  `Art,Music,Performance,Photography`

### Preço do tíquete

* [!UICONTROL Tipo de dados] : `Number`
* [!UICONTROL Renderizar como] : `numberfield`
* [!UICONTROL Rótulo do campo] : `Ticket Price`
* [!UICONTROL Nome da Propriedade] : `eventPrice`
* [!UICONTROL Tipo] : `Integer`
* [!UICONTROL Obrigatório] : `Yes`

### Imagem do evento

* [!UICONTROL Tipo de dados] : `Content Reference`
* [!UICONTROL Renderizar como] : `contentreference`
* [!UICONTROL Rótulo do campo] : `Event Image`
* [!UICONTROL Nome da Propriedade] : `eventImage`
* [!UICONTROL Caminho raiz] : `/content/dam/wknd-mobile/images`
* [!UICONTROL Obrigatório] : `Yes`

### Nome do local

* [!UICONTROL Tipo de dados] : `Single-line text`
* [!UICONTROL Renderizar como] : `textfield`
* [!UICONTROL Rótulo do campo] : `Venue Name`
* [!UICONTROL Nome da Propriedade] : `venueName`
* [!UICONTROL Extensão]  Máx.: 20º
* [!UICONTROL Obrigatório] : `Yes`

### Cidade do local

* [!UICONTROL Tipo de dados] : `Enumeration`
* [!UICONTROL Rótulo do campo] : `Venue City`
* [!UICONTROL Nome da Propriedade] : `venueCity`
* [!UICONTROL Opções] :  `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>O **[!UICONTROL Nome da propriedade]** indica o **ambos** nome da propriedade JCR onde esse valor será armazenado, bem como a chave no arquivo JSON . Esse deve ser um nome semântico que não será alterado durante a vida útil do Modelo de fragmento de conteúdo.

Após concluir a criação do Modelo do fragmento de conteúdo, você deve acabar com uma definição que se parece com:


![Modelo de fragmento do conteúdo do evento](assets/chapter-2/event-content-fragment-model.png)

## Próxima etapa

Como opção, instale o pacote de conteúdo [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) no AEM Author por meio do [Gerenciador de pacotes do AEM](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descritos nesta parte do tutorial.

* [Capítulo 3 - Criação de fragmentos de conteúdo do evento](./chapter-3.md)
