---
title: Capítulo 4 - Definição de modelos de serviços de conteúdo - Serviços de conteúdo
description: O Capítulo 4 do tutorial AEM Headless cobre a função de Modelos AEM editáveis no contexto de AEM Content Services. Modelos editáveis são usados para definir a estrutura de conteúdo JSON AEM os Serviços de conteúdo são expostos.
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

# Capítulo 4 - Definição de modelos de serviços de conteúdo

O Capítulo 4 do tutorial AEM Headless cobre a função de Modelos AEM editáveis no contexto de AEM Content Services. Modelos editáveis são usados para definir a estrutura de conteúdo JSON AEM os Serviços de conteúdo expõem aos clientes por meio da composição dos Serviços de conteúdo ativados AEM Componentes.

## Noções básicas sobre a função dos Modelos nos AEM Content Services

AEM Modelos editáveis são usados para definir os pontos finais HTTP que são acessados para expor o conteúdo do Evento como JSON.

Tradicionalmente AEM Modelos editáveis são usados para definir páginas da Web, no entanto, esse uso é simplesmente uma convenção. Modelos editáveis podem ser usados para compor **any** conjunto de conteúdo; como esse conteúdo é acessado: como HTML em um navegador, como JSON consumido pelo JavaScript (AEM Editor de SPA) ou um Aplicativo móvel é uma função de como essa página é solicitada.

Nos AEM Content Services, os modelos editáveis são usados para definir como os dados JSON são expostos.

Para o [!DNL WKND Mobile] Aplicativo, criaremos um único Modelo editável usado para direcionar um único endpoint da API. Embora este exemplo seja simples de ilustrar os conceitos do AEM Headless, você pode criar várias páginas (ou endpoints) cada uma expondo diferentes conjuntos de conteúdo para criar uma API mais complexa e mais organizada.

## Noções básicas do ponto final da API

Para entender como compor nosso ponto de extremidade de API e entender qual conteúdo deve ser exposto a nosso [!DNL WKND Mobile] Aplicativo, vamos revisitar o design.

![Decomposição da página da API Eventos](./assets/chapter-4/design-to-component-mapping.png)

Como podemos ver, temos três conjuntos lógicos de conteúdo para fornecer ao aplicativo móvel.

1. O **Logotipo**
2. O **Linha de tag**
3. A lista de **Eventos**

Para fazer isso, podemos mapear esses requisitos para AEM Componentes (e, no nosso caso, AEM Componentes principais do WCM) para expor o conteúdo necessário como JSON.

1. O **Logotipo** é revelado por um **Componente de imagem**
2. O **Linha de tag** é revelado via **Componente de texto**
3. A lista de **Eventos** é revelado via **Componente da lista de fragmentos do conteúdo** que, por sua vez, faz referência a um conjunto de Fragmentos do conteúdo do evento.

>[!NOTE]
>
>Para suportar AEM exportação JSON de páginas e componentes do serviço de conteúdo, as páginas e os componentes devem **derivar dos Componentes principais AEM WCM**.
>
>[Componentes principais do WCM AEM](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) têm funcionalidade integrada para suportar um esquema JSON normalizado de Páginas e Componentes exportados. Todos os componentes WKND Mobile usados neste tutorial (Página, Imagem, Texto e Lista de fragmentos de conteúdo) são derivados AEM Componentes WCM principais.

## Como definir o modelo da API de eventos

1. Navegar para **[!UICONTROL Ferramentas] > [!UICONTROL Geral] > [!UICONTROL Modelos] >[!DNL WKND Mobile]**.

1. Crie o **[!DNL Events API]** modelo:

   1. Toque **[!UICONTROL Criar]** na barra de ação superior
   1. Selecione o **[!DNL WKND Mobile - Empty Page]** modelo
   1. Toque **[!UICONTROL Próximo]** na barra de ação superior
   1. Enter **[!DNL Events API]** no [!UICONTROL Título do modelo] campo
   1. Toque **[!UICONTROL Criar]** na barra de ação superior
   1. Toque **[!UICONTROL Abrir]** abrir o novo modelo para edição

1. Primeiro, permitimos os três componentes AEM identificados que precisamos modelar o conteúdo editando o [!UICONTROL Política] da Raiz [!UICONTROL Contêiner de layout]. Verifique se a **[!UICONTROL Estrutura]** estiver ativo, selecione o **[!DNL Layout Container \[Root\]]** e toque no **[!UICONTROL Política]** botão.
1. Em **[!UICONTROL Propriedades] > [!UICONTROL Componentes permitidos]** pesquisar por **[!DNL WKND Mobile]**. Permitir os seguintes componentes do [!DNL WKND Mobile] grupo de componentes para que possam ser usados no [!DNL Events] página da API.

   * **[!DNL WKND Mobile > Image]**

      * O logotipo do aplicativo
   * **[!DNL WKND Mobile > Text]**

      * O texto introdutório do aplicativo
   * **[!DNL WKND Mobile > Content Fragment List]**

      * A lista de categorias de Evento disponíveis para exibição no aplicativo



1. Toque no **[!UICONTROL Concluído]** marca de seleção no canto superior direito ao concluir.
1. **Atualizar** a janela do navegador para ver [!UICONTROL Componentes permitidos] no painel esquerdo.
1. No localizador Componentes, no painel esquerdo, arraste os seguintes Componentes AEM:
   1. **[!DNL Image]** para o logotipo
   2. **[!DNL Text]** para a linha de tag
   3. **[!DNL Content Fragment List]** para os eventos
1. **Para cada um dos componentes acima**, selecione-os e pressione a tecla **desbloquear** botão.
1. No entanto, assegure que **contêiner de layout** é **bloqueado** para impedir a adição de outros componentes ou a remoção desses três componentes.
1. Toque **[!UICONTROL Informações da página] > [!UICONTROL Exibir no Admin]** para retornar ao [!DNL WKND Mobile] listagem de modelos. Selecione o **[!DNL Events API]** modelo e toque **[!UICONTROL Habilitar]** na barra de ação superior.

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> Observe que os componentes usados para exibir o conteúdo são adicionados ao próprio Modelo e bloqueados. Isso permite que os autores editem os componentes predefinidos, mas não adicione ou remova arbitrariamente os componentes, pois alterar a própria API pode quebrar as suposições em torno da estrutura JSON e interromper os aplicativos de consumo. Todas as APIs precisam ser estáveis.

## Próximas etapas

Como opção, instale a variável [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) pacote de conteúdo no AEM Author via [Gerenciador de pacotes de AEM](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descrito neste e nos capítulos anteriores do tutorial.

* [Capítulo 5 - Criação das páginas dos serviços de conteúdo](./chapter-5.md)
