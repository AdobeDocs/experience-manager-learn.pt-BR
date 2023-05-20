---
title: Capítulo 2 - Definição de modelos de fragmento de conteúdo de eventos - Serviços de conteúdo
seo-title: Getting Started with AEM Content Services - Chapter 2 - Defining Event Content Fragment Models
description: O capítulo 2 do tutorial do AEM headless aborda a ativação e a definição de modelos de fragmento de conteúdo usados para definir uma estrutura de dados normalizada e uma interface de criação para criar eventos.
seo-description: Chapter 2 of the AEM Headless tutorial covers enabling and defining Content Fragment Models used to define a normalized data structure and authoring interface for creating Events.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 9%

---

# Capítulo 2 - Utilização de modelos de fragmento de conteúdo

Os modelos de fragmento de conteúdo do AEM definem esquemas de conteúdo que podem ser usados para modelar a criação de conteúdo bruto por autores do AEM. Essa abordagem é semelhante à criação em andaime ou baseada em formulários. O conceito principal com Fragmentos de conteúdo é que o conteúdo criado é independente de apresentação, o que significa que ele é destinado ao uso em vários canais, onde o aplicativo de consumo, seja o AEM, um aplicativo de página única ou um aplicativo móvel, controla como o conteúdo é exibido ao usuário.

A principal preocupação do fragmento de conteúdo é garantir:

1. O conteúdo correto é coletado do autor
2. O conteúdo pode ser exposto em um formato estruturado e bem compreendido a aplicativos de consumo.

Este capítulo aborda a ativação e a definição dos modelos de fragmento de conteúdo usados para definir uma estrutura de dados normalizada e uma interface de criação para modelagem e criação de &quot;Eventos&quot;.

## Ativar modelos de fragmento de conteúdo

Modelos de fragmentos do conteúdo **deve** ser ativado via **[AEM [!UICONTROL Navegador de configuração]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=pt-BR)**.

Se os modelos de fragmento de conteúdo forem **não** ativado para uma configuração, a variável **[!UICONTROL Criar] > [!UICONTROL Fragmento do conteúdo]** O botão não será exibido para a configuração relevante do AEM.

>[!NOTE]
>
>As configurações de AEM representam um conjunto de [configurações de locatários com reconhecimento de contexto](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) armazenado em `/conf`. Normalmente, as configurações do AEM estão correlacionadas a um site específico gerenciado no AEM Sites ou a uma unidade de negócios responsável por um subconjunto de conteúdo (ativos, páginas etc.) no AEM.
>
>Para que uma configuração afete uma hierarquia de conteúdo, a configuração deve ser referenciada por meio do `cq:conf` nessa hierarquia de conteúdo. (Isso é feito para o [!DNL WKND Mobile] configuração no **Etapa 5** abaixo).
>
>Quando a variável `global` for usada, a configuração se aplicará a todo o conteúdo e `cq:conf` não precisa ser definido.
>
>Consulte a [[!UICONTROL Navegador de configuração] documentação](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=pt-BR) para obter mais informações.

1. Faça logon no AEM Author como um usuário com as permissões apropriadas para modificar a configuração relevante.
   * Para este tutorial, a variável **administrador** usuário pode ser usado.
1. Navegue até **[!UICONTROL Ferramenta] > [!UICONTROL Geral] > [!UICONTROL Navegador de configuração]**
1. Toque no **ícone de pasta** ao lado de **[!DNL WKND Mobile]** para selecionar e, em seguida, toque no **[!UICONTROL Editar] botão** no canto superior esquerdo.
1. Selecionar **[!UICONTROL Modelos de fragmentos do conteúdo]** e toque em **[!UICONTROL Salvar e fechar]** no canto superior direito.

   Isso ativa a criação de modelos de fragmento de conteúdo em árvores de conteúdo da pasta de ativos que têm o [!DNL WKND Mobile] configuração aplicada.

   >[!NOTE]
   >
   >Essa alteração de configuração não é reversível do [!UICONTROL Configuração do AEM] Interface da Web. Para desfazer essa configuração:
   >    
   >    1. Abertura [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Vá até `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Exclua o `models` nó

   >    
   >Todos os modelos de fragmento de conteúdo existentes criados nessa configuração são excluídos, bem como suas definições são armazenadas em `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Aplique o **[!DNL WKND Mobile]** configuração para o **[!DNL WKND Mobile]Pasta de ativos** para permitir que fragmentos de conteúdo de modelos de fragmento de conteúdo sejam criados nessa hierarquia de pastas de ativos:

   1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Arquivos]**
   1. Selecione o **[!UICONTROL WKND Mobile] pasta**
   1. Toque no **[!UICONTROL Propriedades]** botão na barra de ação superior para abrir [!UICONTROL Propriedades da pasta]
   1. Entrada [!UICONTROL Propriedades da pasta], toque na guia **[!UICONTROL Cloud Services]** guia
   1. Verifique se **[!UICONTROL Configuração na nuvem]** o campo está definido como **/conf/wknd-mobile**
   1. Toque **[!UICONTROL Salvar e fechar]** no canto superior direito para continuar com as alterações

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __Modelos de fragmentos do conteúdo__ moveram-se de __Ferramentas > Ativos__ para __Ferramentas > Geral__.

## Noções básicas sobre o modelo de fragmento de conteúdo para criação

Antes de definir nosso modelo de Fragmento de conteúdo, vamos analisar a experiência que conduziremos para garantir que capturemos todos os pontos de dados necessários. Para isso, analisaremos o design de aplicativos móveis e mapearemos os elementos de design para o conteúdo a ser coletado.

Podemos dividir os pontos de dados que definem um Evento da seguinte maneira:

![Criação do modelo de fragmento de conteúdo](assets/chapter-2/design-to-model-mapping.png)

De posse do mapeamento, podemos definir os Fragmentos de conteúdo usados para coletar e, por fim, expor os dados do evento.

## Criação do modelo de fragmento de conteúdo

1. Navegue até **[!UICONTROL Ferramentas] > [!UICONTROL Geral] > [!UICONTROL Modelos de fragmentos do conteúdo]**.
1. Toque no **[!DNL WKND Mobile]** pasta a ser aberta.
1. Toque **[!UICONTROL Criar]** para abrir o assistente de criação do modelo de fragmento de conteúdo.
1. Enter **[!DNL Event]** como o **[!UICONTROL Título do modelo]** *(a descrição é opcional)* e toque em **[!UICONTROL Criar]** para salvar.

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## Definição da estrutura do modelo de fragmento de conteúdo

1. Navegue até **[!UICONTROL Ferramentas] > [!UICONTROL Geral] > [!UICONTROL Modelos de fragmentos do conteúdo] >[!DNL WKND]**.
1. Selecione o **[!DNL Event]** Modelo de fragmento de conteúdo e toque em **[!UICONTROL Editar]** na barra de ação superior.
1. No **[!UICONTROL Tipos de dados] guia** à direita, arraste o **[!UICONTROL Entrada de texto em linha única]** na área à esquerda para definir o **[!DNL Question]** campo.
1. Garanta a nova **[!UICONTROL Entrada de texto em linha única]** está selecionada à esquerda e a variável **[!UICONTROL Propriedades] guia** está selecionada à direita. Preencha os campos Propriedades da seguinte maneira:

   * [!UICONTROL Renderizar como] : `textfield`
   * [!UICONTROL Rótulo do campo] : `Event Title`
   * [!UICONTROL Nome da Propriedade] : `eventTitle`
   * [!UICONTROL Comprimento máximo] : 25
   * [!UICONTROL Obrigatório] : `Yes`

Repita essas etapas usando as definições de entrada definidas abaixo para criar o restante do modelo de fragmento de conteúdo do evento.

>[!NOTE]
>
> A variável **Nome da propriedade** Os campos DEVEM corresponder exatamente, pois o aplicativo Android está programado para apagar esses nomes.

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
* [!UICONTROL Opções] : `Art,Music,Performance,Photography`

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
* [!UICONTROL Comprimento máximo] : 20
* [!UICONTROL Obrigatório] : `Yes`

### Cidade do local

* [!UICONTROL Tipo de dados] : `Enumeration`
* [!UICONTROL Rótulo do campo] : `Venue City`
* [!UICONTROL Nome da Propriedade] : `venueCity`
* [!UICONTROL Opções] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>A variável **[!UICONTROL Nome da propriedade]** denota o **ambos** o nome da propriedade JCR em que esse valor é armazenado, bem como a chave no arquivo JSON. Esse deve ser um nome semântico que não será alterado durante a vida útil do modelo de fragmento de conteúdo.

Depois de concluir a criação do modelo de fragmento de conteúdo, você deve obter uma definição que se parece com:


![Modelo de fragmento do conteúdo do evento](assets/chapter-2/event-content-fragment-model.png)

## Próxima etapa

Como opção, instale o [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) pacote de conteúdo no AEM Author via [Gerenciador de pacotes AEM](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descritos nesta parte do tutorial.

* [Capítulo 3 - Criação de fragmentos de conteúdo de evento](./chapter-3.md)
