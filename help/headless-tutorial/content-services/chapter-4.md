---
title: Capítulo 4 - Definição de modelos de serviços de conteúdo - Serviços de conteúdo
seo-title: Introdução ao AEM sem cabeçalho - Capítulo 4 - Definição de modelos de serviços de conteúdo
description: O Capítulo 4 do tutorial AEM sem cabeçalho aborda a função AEM Modelos editáveis no contexto AEM Content Services. Modelos editáveis são usados para definir a estrutura de conteúdo JSON AEM os Serviços de conteúdo serão expostos.
seo-description: O Capítulo 4 do tutorial AEM sem cabeçalho aborda a função AEM Modelos editáveis no contexto AEM Content Services. Modelos editáveis são usados para definir a estrutura de conteúdo JSON AEM os Serviços de conteúdo serão expostos.
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '837'
ht-degree: 0%

---


# Capítulo 4 - Definição de modelos de serviços de conteúdo

O Capítulo 4 do tutorial AEM sem cabeçalho aborda a função AEM Modelos editáveis no contexto AEM Content Services. Modelos editáveis são usados para definir a estrutura de conteúdo JSON AEM os Serviços de conteúdo são expostos aos clientes por meio da composição dos Serviços de conteúdo ativados AEM Componentes.

## Como entender a função de Modelos no AEM Content Services

AEM Modelos editáveis são usados para definir os pontos finais HTTP que serão acessados para expor o conteúdo do Evento como JSON.

Tradicionalmente AEM Modelos editáveis são usados para definir páginas da Web, no entanto, esse uso é uma simples convenção. Modelos editáveis podem ser usados para compor **qualquer** conjunto de conteúdo; como esse conteúdo é acessado: como um HTML em um navegador, como JSON consumido pelo JavaScript (AEM SPA Editor) ou um aplicativo móvel é uma função da forma como essa página é solicitada.

AEM Content Services, modelos editáveis são usados para definir como os dados JSON são expostos.

Para o aplicativo [!DNL WKND Mobile], criaremos um único Modelo editável que será usado para direcionar um único terminal de API. Embora este exemplo seja simples de ilustrar os conceitos de AEM sem cabeçalho, você pode criar várias páginas (ou pontos de extremidade) cada uma expondo diferentes conjuntos de conteúdo para criar uma API mais complexa e mais organizada.

## Como entender o ponto final da API

Para entender como compor nosso terminal de API e entender qual conteúdo deve ser exposto ao [!DNL WKND Mobile] aplicativo, vamos revisitar o design.

![Decomposição da página da API de eventos](./assets/chapter-4/design-to-component-mapping.png)

Como podemos ver, temos três conjuntos lógicos de conteúdo para fornecer ao aplicativo móvel.

1. O **Logo**
2. A **Linha de tag**
3. A lista de **Eventos**

Para fazer isso, podemos mapear esses requisitos para AEM Componentes (e, no nosso caso, AEM Componentes principais do WCM) para expor o conteúdo necessário como JSON.

1. O **logotipo** será exibido por meio de um **componente de imagem**
2. O **Linha de tag** será exibido por meio de um **componente de texto**
3. A lista de **Eventos** será exibida por meio de um **componente de Lista do fragmento de conteúdo** que, por sua vez, faz referência a um conjunto de Fragmentos de conteúdo do Evento.

>[!NOTE]
>
>Para suportar AEM exportação JSON de Páginas e Componentes do Serviço de Conteúdo, as Páginas e os Componentes devem **derivar AEM Componentes Principais WCM**.
>
>[AEM Componentes principais do WCM ](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) têm funcionalidade integrada para suportar um schema JSON normalizado de Páginas e Componentes exportados. Todos os componentes do WKND Mobile usados neste tutorial (Página, Imagem, Texto e Lista de fragmento de conteúdo) são derivados AEM Componentes principais do WCM.

## Definição do modelo de API de Eventos

1. Navegue até **[!UICONTROL Ferramentas] > [!UICONTROL Geral] > [!UICONTROL Modelos] >[!DNL WKND Mobile]**.

1. Crie o modelo **[!DNL Events API]**:

   1. Toque em **[!UICONTROL Criar]** na barra de ação superior
   1. Selecione o modelo **[!DNL WKND Mobile - Empty Page]**
   1. Toque em **[!UICONTROL Próximo]** na barra de ação superior
   1. Digite **[!DNL Events API]** no campo [!UICONTROL Título do modelo]
   1. Toque em **[!UICONTROL Criar]** na barra de ação superior
   1. Toque em **[!UICONTROL Abrir]** para abrir o novo modelo para edição

1. Primeiro, permitimos que os três componentes AEM identificados precisem modelar o conteúdo editando a [!UICONTROL Política] da Raiz [!UICONTROL Container de layout]. Verifique se o modo **[!UICONTROL Estrutura]** está ativo, selecione **[!DNL Layout Container \[Root\]]** e toque no botão **[!UICONTROL Política]**.
1. Em **[!UICONTROL Propriedades] > [!UICONTROL Componentes permitidos]** procure **[!DNL WKND Mobile]**. Permita os seguintes componentes do grupo de componentes [!DNL WKND Mobile] para que possam ser usados na página da API [!DNL Events].

   * **[!DNL WKND Mobile > Image]**

      * O logotipo do aplicativo
   * **[!DNL WKND Mobile > Text]**

      * O texto introdutório do aplicativo
   * **[!DNL WKND Mobile > Content Fragment List]**

      * A lista das categorias do Evento disponível para exibição no aplicativo



1. Pressione a marca de seleção **[!UICONTROL Done]** no canto superior direito quando concluído.
1. **Atualize** a janela do navegador para ver a lista  [!UICONTROL de ] componentes recém-permitidos no painel esquerdo.
1. No Localizador de componentes no painel esquerdo, arraste os seguintes componentes AEM:
   1. **[!DNL Image]** para o logotipo
   2. **[!DNL Text]** para a linha de tag
   3. **[!DNL Content Fragment List]** para os eventos
1. **Para cada um dos componentes** acima, selecione-os e pressione o botão  **** de desbloqueio.
1. No entanto, certifique-se de que **container de layout** esteja **bloqueado** para evitar que outros componentes sejam adicionados ou que esses três componentes sejam removidos.
1. Toque em **[!UICONTROL Informações da página] > [!UICONTROL Visualização em Admin]** para voltar à lista de [!DNL WKND Mobile] modelos. Selecione o modelo **[!DNL Events API]** recém-criado e toque em **[!UICONTROL Ativar]** na barra de ação superior.

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> Observe que os componentes usados para exibir o conteúdo são adicionados ao próprio Modelo e bloqueados. Isso permite que os autores editem os componentes predefinidos, mas não adicionem ou removam arbitrariamente os componentes, já que a alteração da própria API pode quebrar as premissas em torno da estrutura JSON e interromper os aplicativos de consumo. Todas as APIs precisam estar estáveis.

## Próximos passos

Como opção, instale o pacote de conteúdo [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) no AEM Author por [AEM Gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descrito neste e nos capítulos anteriores do tutorial.

* [Capítulo 5 - Criação de páginas de serviços de conteúdo](./chapter-5.md)
