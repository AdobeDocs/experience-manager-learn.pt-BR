---
title: Capítulo 4 - Definição de modelos de serviços de conteúdo - Serviços de conteúdo
seo-title: Introdução ao AEM Headless - Capítulo 4 - Definição de modelos de serviços de conteúdo
description: O Capítulo 4 do tutorial AEM Headless cobre a função de Modelos editáveis do AEM no contexto dos Serviços de conteúdo do AEM. Modelos editáveis são usados para definir a estrutura de conteúdo JSON que os AEM Content Services irão expor.
seo-description: O Capítulo 4 do tutorial AEM Headless cobre a função de Modelos editáveis do AEM no contexto dos Serviços de conteúdo do AEM. Modelos editáveis são usados para definir a estrutura de conteúdo JSON que os AEM Content Services irão expor.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '845'
ht-degree: 0%

---


# Capítulo 4 - Definição de modelos de serviços de conteúdo

O Capítulo 4 do tutorial AEM Headless cobre a função de Modelos editáveis do AEM no contexto dos Serviços de conteúdo do AEM. Modelos editáveis são usados para definir a estrutura de conteúdo JSON que os AEM Content Services expõe aos clientes por meio da composição de Componentes do AEM habilitados para Content Services.

## Noções básicas sobre a função dos modelos nos serviços de conteúdo do AEM

Modelos editáveis do AEM são usados para definir os pontos finais HTTP que serão acessados para expor o conteúdo do Evento como JSON.

Tradicionalmente, os Modelos editáveis do AEM são usados para definir páginas da Web, no entanto, esse uso é simplesmente uma convenção. Modelos editáveis podem ser usados para compor **qualquer** conjunto de conteúdo; como esse conteúdo é acessado: como um HTML em um navegador, como JSON consumido pelo JavaScript (Editor SPA do AEM) ou um Aplicativo móvel é uma função de como essa página é solicitada.

Nos Serviços de conteúdo do AEM, os modelos editáveis são usados para definir como os dados JSON são expostos.

Para o aplicativo [!DNL WKND Mobile], criaremos um modelo editável único que será usado para direcionar um único endpoint da API. Embora este exemplo seja simples de ilustrar os conceitos do AEM Headless, você pode criar várias páginas (ou endpoints) cada uma expondo diferentes conjuntos de conteúdo para criar uma API mais complexa e mais organizada.

## Noções básicas do ponto final da API

Para entender como compor nosso ponto de extremidade de API e entender qual conteúdo deve ser exposto ao nosso [!DNL WKND Mobile] aplicativo, vamos revisitar o design.

![Decomposição da página da API Eventos](./assets/chapter-4/design-to-component-mapping.png)

Como podemos ver, temos três conjuntos lógicos de conteúdo para fornecer ao aplicativo móvel.

1. O **Logo**
2. A **Linha de Tag**
3. A lista de **Eventos**

Para fazer isso, podemos mapear esses requisitos para os Componentes do AEM (e, no nosso caso, os Componentes principais do WCM no AEM) para expor o conteúdo necessário como JSON.

1. O **Logotipo** será exibido por meio de um **Componente de imagem**
2. O **Linha de Tag** será exibido por meio de um **Componente de texto**
3. A lista de **Eventos** será exibida por meio de um **componente da Lista de fragmentos de conteúdo** que, por sua vez, faz referência a um conjunto de Fragmentos de conteúdo de evento.

>[!NOTE]
>
>Para ser compatível com a exportação JSON de páginas e componentes do AEM Content Service, as páginas e os componentes devem **derivar dos Componentes principais do WCM AEM**.
>
>[Os ](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) Componentes principais do WCM no AEM têm funcionalidade integrada para suportar um esquema JSON normalizado de páginas e componentes exportados. Todos os componentes WKND Mobile usados neste tutorial (Página, Imagem, Texto e Lista de fragmentos de conteúdo) são derivados dos Componentes principais WCM do AEM.

## Como definir o modelo da API de eventos

1. Navegue até **[!UICONTROL Ferramentas] > [!UICONTROL Geral] > [!UICONTROL Modelos] >[!DNL WKND Mobile]**.

1. Crie o modelo **[!DNL Events API]**:

   1. Toque em **[!UICONTROL Criar]** na barra de ações superior
   1. Selecione o modelo **[!DNL WKND Mobile - Empty Page]**
   1. Toque em **[!UICONTROL Próximo]** na barra de ação superior
   1. Insira **[!DNL Events API]** no campo [!UICONTROL Título do modelo]
   1. Toque em **[!UICONTROL Criar]** na barra de ações superior
   1. Toque em **[!UICONTROL Abrir]** para abrir o novo modelo para edição

1. Primeiro, permitimos os três Componentes do AEM identificados, necessários para modelar o conteúdo ao editar a [!UICONTROL Política] da Raiz [!UICONTROL Contêiner de layout]. Certifique-se de que o modo **[!UICONTROL Estrutura]** está ativo, selecione o **[!DNL Layout Container \[Root\]]** e toque no botão **[!UICONTROL Política]**.
1. Em **[!UICONTROL Propriedades] > [!UICONTROL Componentes permitidos]** pesquise **[!DNL WKND Mobile]**. Permita os seguintes componentes do grupo de componentes [!DNL WKND Mobile] para que possam ser usados na página da API [!DNL Events].

   * **[!DNL WKND Mobile > Image]**

      * O logotipo do aplicativo
   * **[!DNL WKND Mobile > Text]**

      * O texto introdutório do aplicativo
   * **[!DNL WKND Mobile > Content Fragment List]**

      * A lista de categorias de Evento disponíveis para exibição no aplicativo



1. Toque na marca de seleção **[!UICONTROL Concluído]** no canto superior direito ao concluir.
1. **** Atualize a janela do navegador para ver a lista de  [!UICONTROL Componentes recém-] permitidos no painel esquerdo.
1. No Localizador de Componentes no painel esquerdo, arraste os seguintes Componentes do AEM:
   1. **[!DNL Image]** para o logotipo
   2. **[!DNL Text]** para a linha de tag
   3. **[!DNL Content Fragment List]** para os eventos
1. **Para cada um dos componentes** acima, selecione-os e pressione o botão  **** de desbloqueio.
1. No entanto, verifique se o **contêiner de layout** está **bloqueado** para impedir que outros componentes sejam adicionados ou que esses três componentes sejam removidos.
1. Toque em **[!UICONTROL Informações da página] > [!UICONTROL Exibir em Admin]** para retornar à lista de modelos [!DNL WKND Mobile]. Selecione o modelo **[!DNL Events API]** recém-criado e toque em **[!UICONTROL Ativar]** na barra de ação superior.

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> Observe que os componentes usados para exibir o conteúdo são adicionados ao próprio Modelo e bloqueados. Isso permite que os autores editem os componentes predefinidos, mas não adicione ou remova arbitrariamente os componentes, pois alterar a própria API pode quebrar as suposições em torno da estrutura JSON e interromper os aplicativos de consumo. Todas as APIs precisam ser estáveis.

## Próximas etapas

Como opção, instale o pacote de conteúdo [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) no AEM Author por meio do [Gerenciador de pacotes do AEM](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descrito neste e nos capítulos anteriores do tutorial.

* [Capítulo 5 - Criação das páginas dos serviços de conteúdo](./chapter-5.md)
