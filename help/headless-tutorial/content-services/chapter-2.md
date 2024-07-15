---
title: Capítulo 2 - Definição de modelos de fragmento de conteúdo de eventos - Serviços de conteúdo
description: O capítulo 2 do tutorial do AEM headless aborda a ativação e a definição de modelos de fragmento de conteúdo usados para definir uma estrutura de dados normalizada e uma interface de criação para criar eventos.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
duration: 378
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 0%

---

# Capítulo 2 - Utilização de modelos de fragmento de conteúdo

Os modelos de fragmento de conteúdo do AEM definem esquemas de conteúdo que podem ser usados para modelar a criação de conteúdo bruto por autores do AEM. Essa abordagem é semelhante à criação em andaime ou baseada em formulários. O conceito principal com Fragmentos de conteúdo é que o conteúdo criado é independente de apresentação, o que significa que ele é destinado ao uso de vários canais, em que o aplicativo de consumo, seja o AEM, um aplicativo de página única ou um aplicativo móvel, controla como o conteúdo é exibido ao usuário.

A principal preocupação do fragmento de conteúdo é garantir:

1. O conteúdo correto é coletado do autor
2. O conteúdo pode ser exposto em um formato estruturado e bem compreendido a aplicativos de consumo.

Este capítulo aborda a ativação e a definição dos modelos de fragmento de conteúdo usados para definir uma estrutura de dados normalizada e uma interface de criação para modelagem e criação de &quot;Eventos&quot;.

## Ativar modelos de fragmento de conteúdo

Os Modelos de Fragmento de Conteúdo **devem** ser habilitados por meio do **[Navegador de Configuração ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=pt-BR)** do AEM.

Se os Modelos de fragmento de conteúdo estiverem **não** habilitados para uma configuração, o botão **[!UICONTROL Criar] > [!UICONTROL Fragmento de conteúdo]** não aparecerá para a configuração relevante do AEM.

>[!NOTE]
>
>As configurações de AEM representam um conjunto de [configurações de locatário com reconhecimento de contexto](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) armazenadas em `/conf`. Normalmente, as configurações do AEM estão correlacionadas a um site específico gerenciado no AEM Sites ou a uma unidade de negócios responsável por um subconjunto de conteúdo (ativos, páginas etc.) no AEM.
>
>Para que uma configuração afete uma hierarquia de conteúdo, a configuração deve ser referenciada por meio da propriedade `cq:conf` nessa hierarquia de conteúdo. (Isso é obtido para a configuração [!DNL WKND Mobile] na **Etapa 5** abaixo).
>
>Quando a configuração `global` é usada, ela se aplica a todo o conteúdo e `cq:conf` não precisa ser definida.
>
>Consulte a documentação do [[!UICONTROL Navegador de Configuração]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=pt-BR) para obter mais informações.

1. Faça logon no AEM Author como um usuário com as permissões apropriadas para modificar a configuração relevante.
   * Para este tutorial, o usuário **admin** pode ser usado.
1. Navegue até **[!UICONTROL Ferramenta] > [!UICONTROL Geral] > [!UICONTROL Navegador de Configuração]**
1. Toque no **ícone de pasta** ao lado de **[!DNL WKND Mobile]** para selecionar e toque no botão **[!UICONTROL Editar]** no canto superior esquerdo.
1. Selecione **[!UICONTROL Modelos de fragmentos do conteúdo]** e toque em **[!UICONTROL Salvar e fechar]** no canto superior direito.

   Isso habilita a criação de modelos de fragmento de conteúdo em árvores de conteúdo da pasta de ativos que têm a configuração [!DNL WKND Mobile] aplicada.

   >[!NOTE]
   >
   >Esta alteração de configuração não é reversível da interface da Web da [!UICONTROL Configuração de AEM]. Para desfazer essa configuração:
   >    
   >    1. Abrir [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Navegue até `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Excluir o nó `models`
   >    
   >Qualquer modelo de fragmento de conteúdo existente criado nessa configuração é excluído, bem como suas definições são armazenadas em `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Aplique a configuração **[!DNL WKND Mobile]** à **[!DNL WKND Mobile]Pasta do Assets** para permitir que os Fragmentos de conteúdo dos Modelos de fragmento de conteúdo sejam criados nessa hierarquia de pastas do Assets:

   1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Arquivos]**
   1. Selecione a **[!UICONTROL pasta WKND Mobile]**
   1. Toque no botão **[!UICONTROL Propriedades]** na barra de ações superior para abrir [!UICONTROL Propriedades da Pasta]
   1. Em [!UICONTROL Propriedades da Pasta], toque na guia **[!UICONTROL Cloud Service]**
   1. Verifique se o campo **[!UICONTROL Configuração da nuvem]** está definido como **/conf/wknd-mobile**
   1. Toque em **[!UICONTROL Salvar e fechar]** no canto superior direito para manter as alterações

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __Os modelos de fragmento de conteúdo__ foram movidos de __Ferramentas > Assets__ para __Ferramentas > Geral__.

## Noções básicas sobre o modelo de fragmento de conteúdo para criação

Antes de definir nosso modelo de Fragmento de conteúdo, vamos analisar a experiência que conduziremos para garantir que capturemos todos os pontos de dados necessários. Para isso, analisaremos o design de aplicativos móveis e mapearemos os elementos de design para o conteúdo a ser coletado.

Podemos dividir os pontos de dados que definem um Evento da seguinte maneira:

![Criando o modelo de fragmento de conteúdo](assets/chapter-2/design-to-model-mapping.png)

De posse do mapeamento, podemos definir os Fragmentos de conteúdo usados para coletar e, por fim, expor os dados do evento.

## Criação do modelo de fragmento de conteúdo

1. Navegue até **[!UICONTROL Ferramentas] > [!UICONTROL Geral] > [!UICONTROL Modelos de fragmentos de conteúdo]**.
1. Toque na pasta **[!DNL WKND Mobile]** para abrir.
1. Toque em **[!UICONTROL Criar]** para abrir o assistente de criação do modelo de fragmento de conteúdo.
1. Insira **[!DNL Event]** como o **[!UICONTROL Título do modelo]** *(a descrição é opcional)* e toque em **[!UICONTROL Criar]** para salvar.

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## Definição da estrutura do modelo de fragmento de conteúdo

1. Navegue até **[!UICONTROL Ferramentas] > [!UICONTROL Geral] > [!UICONTROL Modelos de fragmentos de conteúdo] >[!DNL WKND]**.
1. Selecione o Modelo de fragmento de conteúdo **[!DNL Event]** e toque em **[!UICONTROL Editar]** na barra de ação superior.
1. Na guia **[!UICONTROL Tipos de Dados]** à direita, arraste a **[!UICONTROL Entrada de texto de linha única]** para a área à esquerda para definir o campo **[!DNL Question]**.
1. Verifique se a nova **[!UICONTROL Entrada de texto de linha única]** está selecionada à esquerda e se a guia **[!UICONTROL Propriedades]** está selecionada à direita. Preencha os campos Propriedades da seguinte maneira:

   * [!UICONTROL Renderizar como] : `textfield`
   * [!UICONTROL Rótulo do Campo] : `Event Title`
   * [!UICONTROL Nome da Propriedade] : `eventTitle`
   * [!UICONTROL Comprimento Máximo] : 25
   * [!UICONTROL Obrigatório] : `Yes`

Repita essas etapas usando as definições de entrada definidas abaixo para criar o restante do modelo de fragmento de conteúdo do evento.

>[!NOTE]
>
> Os campos **Nome da Propriedade** DEVEM coincidir exatamente, pois o aplicativo Android está programado para apagar esses nomes.

### Descrição de evento

* [!UICONTROL Tipo de Dados] : `Multi-line text`
* [!UICONTROL Rótulo do Campo] : `Event Description`
* [!UICONTROL Nome da Propriedade] : `eventDescription`
* [!UICONTROL Tipo Padrão] : `Rich text`

### Data e hora do evento

* [!UICONTROL Tipo de Dados] : `Date and time`
* [!UICONTROL Rótulo do Campo] : `Event Date and Time`
* [!UICONTROL Nome da Propriedade] : `eventDateAndTime`
* [!UICONTROL Obrigatório] : `Yes`

### Tipo de evento

* [!UICONTROL Tipo de Dados] : `Enumeration`
* [!UICONTROL Rótulo do Campo] : `Event Type`
* [!UICONTROL Nome da Propriedade] : `eventType`
* [!UICONTROL Opções] : `Art,Music,Performance,Photography`

### Preço do tíquete

* [!UICONTROL Tipo de Dados] : `Number`
* [!UICONTROL Renderizar como] : `numberfield`
* [!UICONTROL Rótulo do Campo] : `Ticket Price`
* [!UICONTROL Nome da Propriedade] : `eventPrice`
* [!UICONTROL Tipo] : `Integer`
* [!UICONTROL Obrigatório] : `Yes`

### Imagem do evento

* [!UICONTROL Tipo de Dados] : `Content Reference`
* [!UICONTROL Renderizar como] : `contentreference`
* [!UICONTROL Rótulo do Campo] : `Event Image`
* [!UICONTROL Nome da Propriedade] : `eventImage`
* [!UICONTROL Caminho raiz] : `/content/dam/wknd-mobile/images`
* [!UICONTROL Obrigatório] : `Yes`

### Nome do local

* [!UICONTROL Tipo de Dados] : `Single-line text`
* [!UICONTROL Renderizar como] : `textfield`
* [!UICONTROL Rótulo do Campo] : `Venue Name`
* [!UICONTROL Nome da Propriedade] : `venueName`
* [!UICONTROL Comprimento Máximo] : 20
* [!UICONTROL Obrigatório] : `Yes`

### Cidade do local

* [!UICONTROL Tipo de Dados] : `Enumeration`
* [!UICONTROL Rótulo do Campo] : `Venue City`
* [!UICONTROL Nome da Propriedade] : `venueCity`
* [!UICONTROL Opções] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>O **[!UICONTROL Nome da Propriedade]** denota **ambos** o nome da propriedade JCR onde esse valor está armazenado, bem como a chave no arquivo JSON. Esse deve ser um nome semântico que não será alterado durante a vida útil do modelo de fragmento de conteúdo.

Depois de concluir a criação do modelo de fragmento de conteúdo, você deve obter uma definição que se parece com:


![Modelo de fragmento de conteúdo do evento](assets/chapter-2/event-content-fragment-model.png)

## Próxima etapa

Opcionalmente, instale o pacote de conteúdo do [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) no AEM Author via [Gerenciador de Pacotes do AEM](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descritos nesta parte do tutorial.

* [Capítulo 3 - Criação de fragmentos de conteúdo de evento](./chapter-3.md)
