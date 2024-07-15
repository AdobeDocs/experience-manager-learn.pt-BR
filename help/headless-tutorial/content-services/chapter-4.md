---
title: Capítulo 4 - Definição dos modelos do Content Services - Content Services
description: O Capítulo 4 do tutorial AEM Headless aborda o papel dos modelos editáveis AEM no contexto dos serviços de conteúdo AEM. Os modelos editáveis são usados para definir a estrutura de conteúdo JSON que os Serviços de conteúdo AEM expõem.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
duration: 245
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 0%

---

# Capítulo 4 - Definição dos modelos do Content Services

O Capítulo 4 do tutorial AEM Headless aborda o papel dos modelos editáveis AEM no contexto dos serviços de conteúdo AEM. Modelos editáveis são usados para definir a estrutura de conteúdo JSON que o AEM Content Services expõe aos clientes por meio da composição de componentes AEM habilitados para o Content Services.

## Compreender a função dos modelos nos serviços de conteúdo AEM

Modelos editáveis AEM são usados para definir os pontos de extremidade HTTP que são acessados para expor o conteúdo do Evento como JSON.

Tradicionalmente, os modelos editáveis do AEM são usados para definir páginas da Web, no entanto, esse uso é simplesmente convencional. Modelos editáveis podem ser usados para compor **qualquer** conjunto de conteúdo; como esse conteúdo é acessado: como um HTML em um navegador, como o JSON consumido pelo JavaScript (editor de AEM SPA) ou por um aplicativo móvel é uma função de como essa página é solicitada.

No AEM Content Services, os modelos editáveis são usados para definir como os dados JSON são expostos.

Para o aplicativo [!DNL WKND Mobile], criaremos um único Modelo Editável que é usado para direcionar um único ponto de extremidade de API. Embora este exemplo seja simples de ilustrar os conceitos de AEM Headless, você pode criar várias páginas (ou pontos de extremidade), cada uma expondo diferentes conjuntos de conteúdo para criar uma API mais complexa e mais bem organizada.

## Noções básicas sobre o endpoint da API

Para entender como compor nosso ponto de extremidade de API e entender qual conteúdo deve ser exposto ao nosso Aplicativo [!DNL WKND Mobile], vamos rever o design.

![Decomposição da página de API de eventos](./assets/chapter-4/design-to-component-mapping.png)

Como podemos ver, temos três conjuntos lógicos de conteúdo para fornecer ao aplicativo móvel.

1. O **Logotipo**
2. A **Linha da Marca**
3. A lista de **Eventos**

Para fazer isso, podemos mapear esses requisitos para componentes AEM (e, no nosso caso, AEM WCM Componentes principais) para expor o conteúdo necessário como JSON.

1. O **Logotipo** é exibido por meio de um **componente de Imagem**
2. A **Linha da Marca** é exibida por meio de um **componente de Texto**
3. A lista de **Eventos** é exibida por meio de um **componente de Lista de Fragmentos de Conteúdo** que, por sua vez, faz referência a um conjunto de Fragmentos de Conteúdo de Evento.

>[!NOTE]
>
>Para oferecer suporte à exportação JSON de Páginas e Componentes do Serviço de Conteúdo do AEM, as Páginas e os Componentes devem **derivar dos Componentes Principais do WCM do AEM**.
>
>[Os Componentes Principais do WCM do AEM](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) têm funcionalidade interna para dar suporte a um esquema JSON normalizado de Páginas e Componentes exportados. Todos os componentes do WKND Mobile usados neste tutorial (Página, Imagem, Texto e Lista de fragmentos de conteúdo) são derivados dos Componentes principais WCM do AEM.

## Definição do modelo de API de eventos

1. Navegue até **[!UICONTROL Ferramentas] > [!UICONTROL Geral] > [!UICONTROL Modelos] >[!DNL WKND Mobile]**.

1. Criar o modelo **[!DNL Events API]**:

   1. Toque em **[!UICONTROL Criar]** na barra de ação superior
   1. Selecione o modelo **[!DNL WKND Mobile - Empty Page]**
   1. Toque em **[!UICONTROL Avançar]** na barra de ação superior
   1. Digite **[!DNL Events API]** no campo [!UICONTROL Título do modelo]
   1. Toque em **[!UICONTROL Criar]** na barra de ação superior
   1. Toque em **[!UICONTROL Abrir]** e abra o novo modelo para edição

1. Primeiro, permitimos os três Componentes AEM identificados de que precisamos para modelar o conteúdo editando a [!UICONTROL Política] do [!UICONTROL Contêiner de Layout] Raiz. Verifique se o modo **[!UICONTROL Estrutura]** está ativo, selecione **[!DNL Layout Container \[Root\]]** e toque no botão **[!UICONTROL Política]**.
1. Em **[!UICONTROL Propriedades] > [!UICONTROL Componentes Permitidos]**, pesquise por **[!DNL WKND Mobile]**. Permitir os seguintes componentes do grupo de componentes [!DNL WKND Mobile] para que eles possam ser usados na página de API [!DNL Events].

   * **[!DNL WKND Mobile > Image]**

      * O logotipo do aplicativo

   * **[!DNL WKND Mobile > Text]**

      * O texto de introdução do aplicativo

   * **[!DNL WKND Mobile > Content Fragment List]**

      * A lista de categorias de Evento disponíveis para exibição no aplicativo

1. Toque na marca de seleção **[!UICONTROL Concluído]** no canto superior direito ao concluir.
1. **Atualize** a janela do navegador para ver a lista de [!UICONTROL Componentes Permitidos] recentemente no painel esquerdo.
1. No localizador Componentes no painel à esquerda, arraste os seguintes Componentes AEM:
   1. **[!DNL Image]** para o Logotipo
   2. **[!DNL Text]** para a Linha de Marca
   3. **[!DNL Content Fragment List]** para os eventos
1. **Para cada um dos componentes** acima, selecione-os e pressione o botão **desbloquear**.
1. No entanto, verifique se o **contêiner de layout** está **bloqueado** para impedir que outros componentes sejam adicionados ou que esses três componentes sejam removidos.
1. Toque em **[!UICONTROL Informações da página] > [!UICONTROL Exibir no Administrador]** para retornar à lista de modelos [!DNL WKND Mobile]. Selecione o modelo **[!DNL Events API]** recém-criado e toque em **[!UICONTROL Habilitar]** na barra de ações superior.

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> Observe que os componentes usados para exibir o conteúdo são adicionados ao próprio modelo e bloqueados. Isso permite que os autores editem os componentes predefinidos, mas não adicionem ou removam componentes arbitrariamente, pois alterar a própria API pode quebrar as suposições em torno da estrutura JSON e quebrar aplicativos de consumo. Todas as APIs precisam ser estáveis.

## Próximas etapas

Opcionalmente, instale o pacote de conteúdo do [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) no AEM Author via [Gerenciador de Pacotes do AEM](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descritos neste e nos capítulos anteriores do tutorial.

* [Capítulo 5 - Criação de páginas dos serviços de conteúdo](./chapter-5.md)
