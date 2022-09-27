---
title: Capítulo 2 - Definição dos modelos de fragmento do conteúdo do evento - Serviços de conteúdo
seo-title: Getting Started with AEM Content Services - Chapter 2 - Defining Event Content Fragment Models
description: O Capítulo 2 do tutorial AEM Headless abrange a ativação e definição dos Modelos de fragmento de conteúdo usados para definir uma estrutura de dados normalizada e uma interface de criação para criar Eventos.
seo-description: Chapter 2 of the AEM Headless tutorial covers enabling and defining Content Fragment Models used to define a normalized data structure and authoring interface for creating Events.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
source-git-commit: cfb7ed39ecb85998192ba854b34161f7e1dba19a
workflow-type: tm+mt
source-wordcount: '965'
ht-degree: 10%

---

# Capítulo 2 - Uso de modelos de fragmento de conteúdo

AEM Modelos de fragmento de conteúdo define esquemas de conteúdo que podem ser usados para modelar a criação de conteúdo bruto por autores AEM. Essa abordagem é semelhante ao scaffolding ou à criação baseada em formulários. O conceito principal com Fragmentos de conteúdo é que o conteúdo criado é independente de apresentação, o que significa que ele se destina ao uso de vários canais, onde o aplicativo de consumo, seja AEM, um aplicativo de página única ou um aplicativo móvel, controla como o conteúdo é exibido ao usuário.

A principal preocupação do Fragmento de conteúdo é garantir:

1. O conteúdo correto é coletado do autor
2. O conteúdo pode ser exposto em um formato estruturado e bem compreendido ao consumo de aplicativos.

Este capítulo aborda a ativação e definição dos Modelos de fragmento de conteúdo usados para definir uma estrutura de dados normalizada e uma interface de criação para modelagem e criação de &quot;Eventos&quot;.

## Ativar modelos de fragmento de conteúdo

Modelos de fragmentos do conteúdo **must** ser ativado via **[AEM [!UICONTROL Navegador de configuração]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=pt-BR)**.

Se os modelos de fragmento de conteúdo forem **not** habilitado para uma configuração, a variável **[!UICONTROL Criar] > [!UICONTROL Fragmento de conteúdo]** não aparecerá para a configuração de AEM relevante.

>[!NOTE]
>
>AEM configurações representam um conjunto de [configurações de locatários com reconhecimento de contexto](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) armazenado em `/conf`. Normalmente, AEM configurações se correlacionam a um site específico gerenciado no AEM Sites ou a uma unidade comercial responsável por um subconjunto de conteúdo (ativos, páginas etc.) em AEM.
>
>Para que uma configuração afete uma hierarquia de conteúdo, a configuração deve ser referenciada por meio da variável `cq:conf` nessa hierarquia de conteúdo. (Isso é feito para o [!DNL WKND Mobile] configuração em **Etapa 5** abaixo).
>
>Quando a variável `global` for usada, a configuração se aplica a todo o conteúdo e `cq:conf` não precisa ser definida.
>
>Consulte a [[!UICONTROL Navegador de configuração] documentação](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html) para obter mais informações.

1. Faça logon no AEM Author como um usuário com as permissões apropriadas para modificar a configuração relevante.
   * Para este tutorial, a variável **administrador** usuário pode ser usado.
1. Navegar para **[!UICONTROL Ferramenta] > [!UICONTROL Geral] > [!UICONTROL Navegador de configuração]**
1. Toque no **ícone de pasta** ao lado de **[!DNL WKND Mobile]** para selecionar e tocar em **[!UICONTROL Editar] botão** no canto superior esquerdo.
1. Selecionar **[!UICONTROL Modelos de fragmentos do conteúdo]** e toque em **[!UICONTROL Salvar e fechar]** no canto superior direito.

   Isso permite a utilização de Modelos de fragmento de conteúdo em árvores de conteúdo da pasta de ativos que têm a variável [!DNL WKND Mobile] configuração aplicada.

   >[!NOTE]
   >
   >Essa alteração de configuração não é reversível no [!UICONTROL Configuração de AEM] Interface do usuário da Web. Para desfazer essa configuração:
   >    
   >    1. Abrir [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Vá até `/conf/wknd-mobile/settings/dam/cfm`
   >    1. Exclua o `models` nó

   >    
   >Todos os Modelos de Fragmento de conteúdo existentes criados nessa configuração serão excluídos, bem como suas definições são armazenadas em `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Aplique o **[!DNL WKND Mobile]** para a **[!DNL WKND Mobile]Pasta de ativos** para permitir que os Fragmentos de conteúdo dos Modelos de fragmento de conteúdo sejam criados na hierarquia da pasta Ativos:

   1. Navegar para **[!UICONTROL AEM] > [!UICONTROL Ativos] > [!UICONTROL Arquivos]**
   1. Selecione o **[!UICONTROL WKND Mobile] pasta**
   1. Toque no **[!UICONTROL Propriedades]** na barra de ações superior para abrir [!UICONTROL Propriedades da pasta]
   1. Em [!UICONTROL Propriedades da pasta]toque no **[!UICONTROL Cloud Services]** guia
   1. Verifique o **[!UICONTROL Configuração na nuvem]** estiver definido como **/conf/wknd-mobile**
   1. Toque **[!UICONTROL Salvar e fechar]** no canto superior direito para persistir as alterações

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

>[!WARNING]
>
> __Modelos de fragmentos do conteúdo__ foram transferidos de __Ferramentas > Ativos__ para __Ferramentas > Geral__.

## Noções básicas do modelo de fragmento de conteúdo para criar

Antes de definir o modelo de Fragmento de conteúdo, vamos rever a experiência que estaremos conduzindo para garantir que estamos capturando todos os pontos de dados necessários. Para isso, analisaremos o design dos aplicativos móveis e mapearemos os elementos do design para o conteúdo coletado.

Podemos dividir os pontos de dados que definem um Evento da seguinte maneira:

![Criação do modelo de fragmento de conteúdo](assets/chapter-2/design-to-model-mapping.png)

Munido do mapeamento, podemos definir o Fragmento do conteúdo que será usado para coletar e, em última análise, expor os dados do Evento.

## Criação do modelo de fragmento de conteúdo

1. Navegar para **[!UICONTROL Ferramentas] > [!UICONTROL Geral] > [!UICONTROL Modelos de fragmentos do conteúdo]**.
1. Toque no **[!DNL WKND Mobile]** pasta a ser aberta.
1. Toque **[!UICONTROL Criar]** para abrir o assistente de criação do Modelo de fragmento de conteúdo.
1. Enter **[!DNL Event]** como **[!UICONTROL Título do modelo]** *(a descrição é opcional)* e tocar **[!UICONTROL Criar]** para salvar.

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## Definição da estrutura do modelo de fragmento de conteúdo

1. Navegar para **[!UICONTROL Ferramentas] > [!UICONTROL Geral] > [!UICONTROL Modelos de fragmentos do conteúdo] >[!DNL WKND]**.
1. Selecione o **[!DNL Event]** Modelo do fragmento de conteúdo e toque em **[!UICONTROL Editar]** na barra de ação superior.
1. No **[!UICONTROL Tipos de dados] guia** à direita, arraste a **[!UICONTROL Entrada de texto de linha única]** na zona suspensa esquerda para definir a variável **[!DNL Question]** campo.
1. Certifique-se de que o novo **[!UICONTROL Entrada de texto de linha única]** é selecionada à esquerda e a variável **[!UICONTROL Propriedades] guia** for selecionada à direita. Preencha os campos Propriedades da seguinte maneira:

   * [!UICONTROL Renderizar como] : `textfield`
   * [!UICONTROL Rótulo do campo] : `Event Title`
   * [!UICONTROL Nome da Propriedade] : `eventTitle`
   * [!UICONTROL Extensão Máx.] : 25.
   * [!UICONTROL Obrigatório] : `Yes`

Repita essas etapas usando as definições de entrada definidas abaixo para criar o restante do Modelo de fragmento de conteúdo do evento.

>[!NOTE]
>
> O **Nome da propriedade** Os campos DEVEM corresponder exatamente, pois o aplicativo Android está programado para destacar esses nomes.

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
* [!UICONTROL Extensão Máx.] : 20º
* [!UICONTROL Obrigatório] : `Yes`

### Cidade do local

* [!UICONTROL Tipo de dados] : `Enumeration`
* [!UICONTROL Rótulo do campo] : `Venue City`
* [!UICONTROL Nome da Propriedade] : `venueCity`
* [!UICONTROL Opções] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>O **[!UICONTROL Nome da propriedade]** indica que **both** o nome da propriedade JCR onde esse valor será armazenado, bem como a chave no arquivo JSON . Esse deve ser um nome semântico que não será alterado durante a vida útil do Modelo de fragmento de conteúdo.

Após concluir a criação do Modelo do fragmento de conteúdo, você deve acabar com uma definição que se parece com:


![Modelo de fragmento do conteúdo do evento](assets/chapter-2/event-content-fragment-model.png)

## Próxima etapa

Como opção, instale a variável [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) pacote de conteúdo no AEM Author via [Gerenciador de pacotes de AEM](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descritos nesta parte do tutorial.

* [Capítulo 3 - Criação de fragmentos de conteúdo do evento](./chapter-3.md)
