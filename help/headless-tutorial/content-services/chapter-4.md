---
title: Capítulo 4 - Definição dos modelos do Content Services - Content Services
description: O Capítulo 4 do tutorial AEM Headless aborda o papel dos modelos editáveis AEM no contexto dos serviços de conteúdo AEM. Os modelos editáveis são usados para definir a estrutura de conteúdo JSON que os Serviços de conteúdo AEM expõem.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 0%

---

# Capítulo 4 - Definição dos modelos do Content Services

O Capítulo 4 do tutorial AEM Headless aborda o papel dos modelos editáveis AEM no contexto dos serviços de conteúdo AEM. Modelos editáveis são usados para definir a estrutura de conteúdo JSON que o AEM Content Services expõe aos clientes por meio da composição de componentes AEM habilitados para o Content Services.

## Compreender a função dos modelos nos serviços de conteúdo AEM

Modelos editáveis AEM são usados para definir os pontos de extremidade HTTP que são acessados para expor o conteúdo do Evento como JSON.

Tradicionalmente, os modelos editáveis com AEM são usados para definir páginas da Web, no entanto, esse uso é simplesmente convencional. Modelos editáveis podem ser usados para compor **qualquer** conjunto de conteúdo; como esse conteúdo é acessado: como um HTML em um navegador, como JSON consumido pelo JavaScript (editor de SPA do AEM) ou um aplicativo móvel é uma função de como essa página é solicitada.

No AEM Content Services, os modelos editáveis são usados para definir como os dados JSON são expostos.

Para o [!DNL WKND Mobile] Criaremos um único Modelo editável usado para direcionar um único endpoint de API. Embora este exemplo seja simples de ilustrar os conceitos de AEM Headless, você pode criar várias páginas (ou pontos de extremidade), cada uma expondo diferentes conjuntos de conteúdo para criar uma API mais complexa e mais bem organizada.

## Noções básicas sobre o endpoint da API

Para entender como compor nosso endpoint de API e entender qual conteúdo deve ser exposto ao nosso [!DNL WKND Mobile] App, vamos rever o design.

![Decomposição de página da API de eventos](./assets/chapter-4/design-to-component-mapping.png)

Como podemos ver, temos três conjuntos lógicos de conteúdo para fornecer ao aplicativo móvel.

1. A variável **Logotipo**
2. A variável **Linha de tag**
3. A lista de **Eventos**

Para fazer isso, podemos mapear esses requisitos para componentes AEM (e, no nosso caso, AEM WCM Componentes principais) para expor o conteúdo necessário como JSON.

1. A variável **Logotipo** é revelado através de um **Componente de imagem**
2. A variável **Linha de tag** é revelado através de um **Componente de texto**
3. A lista de **Eventos** é revelado através de um **Componente da lista de fragmentos do conteúdo** que, por sua vez, faz referência a um conjunto de Fragmentos de conteúdo do evento.

>[!NOTE]
>
>Para suportar a exportação JSON de páginas e componentes do serviço de conteúdo AEM, as páginas e os componentes devem **derivar dos componentes principais do WCM no AEM**.
>
>[Componentes principais do WCM no AEM](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) têm funcionalidade integrada para suportar um esquema JSON normalizado de Páginas e Componentes exportados. Todos os componentes do WKND Mobile usados neste tutorial (Página, Imagem, Texto e Lista de fragmentos de conteúdo) são derivados dos Componentes principais do WCM no AEM.

## Definição do modelo de API de eventos

1. Navegue até **[!UICONTROL Ferramentas] > [!UICONTROL Geral] > [!UICONTROL Modelos] >[!DNL WKND Mobile]**.

1. Crie o **[!DNL Events API]** modelo:

   1. Toque **[!UICONTROL Criar]** na barra de ação superior
   1. Selecione o **[!DNL WKND Mobile - Empty Page]** modelo
   1. Toque **[!UICONTROL Próxima]** na barra de ação superior
   1. Enter **[!DNL Events API]** no [!UICONTROL Título do modelo] campo
   1. Toque **[!UICONTROL Criar]** na barra de ação superior
   1. Toque **[!UICONTROL Abertura]** abrir o novo modelo para edição

1. Primeiro, permitimos os três componentes do AEM identificados de que precisamos para modelar o conteúdo editando o [!UICONTROL Política] da Raiz [!UICONTROL Contêiner de layout]. Assegure a **[!UICONTROL Estrutura]** estiver ativo, selecione a variável **[!DNL Layout Container \[Root\]]** e toque no **[!UICONTROL Política]** botão.
1. Em **[!UICONTROL Propriedades] > [!UICONTROL Componentes permitidos]** pesquisar **[!DNL WKND Mobile]**. Permitir os seguintes componentes do [!DNL WKND Mobile] grupo de componentes para que possam ser usados no [!DNL Events] API.

   * **[!DNL WKND Mobile > Image]**

      * O logotipo do aplicativo
   * **[!DNL WKND Mobile > Text]**

      * O texto de introdução do aplicativo
   * **[!DNL WKND Mobile > Content Fragment List]**

      * A lista de categorias de Evento disponíveis para exibição no aplicativo



1. Toque no **[!UICONTROL Concluído]** marca de seleção no canto superior direito ao concluir.
1. **Atualizar** a janela do navegador para ver as novas [!UICONTROL Componentes permitidos] no painel esquerdo.
1. No localizador Componentes no painel à esquerda, arraste os seguintes Componentes AEM:
   1. **[!DNL Image]** para o logotipo
   2. **[!DNL Text]** para a linha da tag
   3. **[!DNL Content Fragment List]** para os eventos
1. **Para cada um dos componentes acima**, selecione-os e pressione a tecla **desbloquear** botão.
1. No entanto, assegurar a **contêiner de layout** é **bloqueado** para impedir que outros componentes sejam adicionados ou que esses três componentes sejam removidos.
1. Toque **[!UICONTROL Informações da página] > [!UICONTROL Exibir no Administrador]** para retornar ao [!DNL WKND Mobile] listagem de modelos. Selecione o recém-criado **[!DNL Events API]** modelo e toque em **[!UICONTROL Ativar]** na barra de ação superior.

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> Observe que os componentes usados para exibir o conteúdo são adicionados ao próprio modelo e bloqueados. Isso permite que os autores editem os componentes predefinidos, mas não adicionem ou removam componentes arbitrariamente, pois alterar a própria API pode quebrar as suposições em torno da estrutura JSON e quebrar aplicativos de consumo. Todas as APIs precisam ser estáveis.

## Próximas etapas

Como opção, instale o [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) pacote de conteúdo no AEM Author via [Gerenciador de pacotes AEM](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descritos neste e nos capítulos anteriores do tutorial.

* [Capítulo 5 - Criação de páginas dos serviços de conteúdo](./chapter-5.md)
