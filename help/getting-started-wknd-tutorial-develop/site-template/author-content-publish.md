---
title: Introdução à criação e publicação | Criação rápida de sites no AEM
description: Use o Editor de páginas no Adobe Experience Manager, AEM, para atualizar o conteúdo do site. Saiba como os Componentes são usados para facilitar a criação. Entenda a diferença entre um autor de AEM e ambientes do Publish e saiba como publicar alterações no site em tempo real.
version: Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7497
thumbnail: KT-7497.jpg
doc-type: Tutorial
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
recommendations: noDisplay, noCatalog
duration: 263
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1285'
ht-degree: 1%

---

# Introdução à criação e publicação {#author-content-publish}

É importante entender como um usuário atualizará o conteúdo do site. Neste capítulo, adotaremos a persona de um **Autor de conteúdo** e faremos algumas atualizações editoriais no site gerado no capítulo anterior. No final do capítulo, publicaremos as alterações para entender como o site ativo é atualizado.

## Pré-requisitos {#prerequisites}

Este é um tutorial de várias partes e presume-se que as etapas descritas no capítulo [Criar um site](./create-site.md) foram concluídas.

## Objetivo {#objective}

1. Entenda os conceitos de **Páginas** e **Componentes** no AEM Sites.
1. Saiba como atualizar o conteúdo do site.
1. Saiba como publicar alterações no site em tempo real.

## Criar uma nova página {#create-page}

Um site normalmente é dividido em páginas para formar uma experiência de várias páginas. O AEM estrutura o conteúdo da mesma maneira. Em seguida, crie uma nova página para o site.

1. Faça logon no Serviço AEM **Author** usado no capítulo anterior.
1. Na tela inicial do AEM, clique em **Sites** > **Site da WKND** > **Inglês** > **Artigo**
1. No canto superior direito, clique em **Criar** > **Página**.

   ![Criar página](assets/author-content-publish/create-page-button.png)

   Isso exibirá o assistente **Criar Página**.

1. Escolha o modelo da **Página de Artigo** e clique em **Avançar**.

   As páginas no AEM são criadas com base em um Modelo de página. Os Modelos de página são explorados detalhadamente no capítulo [Modelos de página](page-templates.md).

1. Em **Propriedades**, digite um **Título** de &quot;Olá, mundo&quot;.
1. Defina o **Nome** como `hello-world` e clique em **Criar**.

   ![Propriedades da página inicial](assets/author-content-publish/initial-page-properties.png)

1. Na caixa de diálogo pop-up, clique em **Abrir** para abrir a página recém-criada.

## Criar um componente {#author-component}

Componentes AEM podem ser considerados pequenos blocos modulares de uma página da Web. Ao dividir a interface em partes lógicas ou Componentes, fica muito mais fácil gerenciar. Para reutilizar componentes, eles devem ser configuráveis. Isso é realizado por meio da caixa de diálogo do autor.

O AEM fornece um conjunto de [Componentes principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR) que estão prontos para uso na produção. Os **Componentes principais** variam de elementos básicos como [Texto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) e [Imagem](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=pt-BR) a elementos de interface do usuário mais complexos, como um [Carrossel](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html?lang=pt-BR).

Em seguida, crie alguns componentes usando o Editor de páginas AEM.

1. Navegue até a página **Olá, mundo**, criada no exercício anterior.
1. Verifique se você está no modo **Editar** e, no painel lateral esquerdo, clique no ícone **Componentes**.

   ![Painel lateral do editor de páginas](assets/author-content-publish/page-editor-siderail.png)

   Isso abrirá a biblioteca de Componentes e listará os Componentes disponíveis que podem ser usados na página.

1. Role para baixo e **Arraste e solte** um componente **Texto (v2)** na região editável principal da página.

   ![Arrastar + Soltar componente de texto](assets/author-content-publish/drag-drop-text-cmp.png)

1. Clique no componente **Texto** para realçar e clique na **chave inglesa** ícone ![ícone de chave inglesa](assets/author-content-publish/wrench-icon.png) para abrir a caixa de diálogo do componente. Insira algum texto e salve as alterações na caixa de diálogo.

   ![Componente de Rich Text](assets/author-content-publish/rich-text-populated-component.png)

   O componente **Texto** agora deve exibir o rich text na página.

1. Repita as etapas acima, exceto para arrastar uma instância do componente **Imagem(v2)** para a página. Abra a caixa de diálogo do componente **Imagem**.

1. No painel à esquerda, alterne para o **Localizador de ativos** clicando no **ícone do Assets** ![ícone do ativo](assets/author-content-publish/asset-icon.png).
1. **Arraste e solte** uma imagem na caixa de diálogo do Componente e clique em **Concluído** para salvar as alterações.

   ![Adicionar ativo à caixa de diálogo](assets/author-content-publish/add-asset-dialog.png)

1. Observe que há componentes na página, como o **Título**, **Navegação**, **Pesquisa**, que são corrigidos. Essas áreas são configuradas como parte do Modelo de página e não podem ser modificadas em uma página individual. Isso é explorado mais no próximo capítulo.

Experimente alguns dos outros componentes. A documentação sobre cada [Componente principal pode ser encontrada aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR). Uma série de vídeos detalhada sobre a [criação de página pode ser encontrada aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## Atualizações do Publish {#publish-updates}

Os ambientes AEM são divididos entre um **Serviço do autor** e um **Serviço do Publish**. Neste capítulo, fizemos várias modificações no site no **Serviço de Autor**. Para que os visitantes do site vejam as alterações, precisamos publicá-las no **Publish Service**.

![Diagrama de alto nível](assets/author-content-publish/author-publish-high-level-flow.png)

*Fluxo de alto nível de conteúdo do Autor para o Publish*

**1.** autores de conteúdo fazem atualizações no conteúdo do site. As atualizações podem ser visualizadas, revisadas e aprovadas para serem enviadas ao vivo.

**2.** Conteúdo publicado. A publicação pode ser feita sob demanda ou programada para uma data futura.

**3.** Os visitantes do site verão as alterações refletidas no serviço do Publish.

### Publish as alterações

Em seguida, vamos publicar as alterações.

1. Na tela inicial do AEM, navegue até **Sites** e selecione o **Site WKND**.
1. Clique em **Gerenciar publicação** na barra de menus.

   ![Gerenciar publicação](assets/author-content-publish/click-manage-publiciation.png)

   Como esse é um site totalmente novo, queremos publicar todas as páginas e podemos usar o assistente Gerenciar publicação para definir exatamente o que precisa ser publicado.

1. Em **Opções**, deixe as configurações padrão para **Publish** e agende-as para **Agora**. Clique em **Avançar**.
1. Em **Escopo**, selecione o **Site WKND** e clique em **Incluir Configurações Filho**. Na caixa de diálogo, marque **Incluir filhos**. Desmarque o restante das caixas para garantir que o site inteiro seja publicado.

   ![Atualizar escopo de publicação](assets/author-content-publish/update-scope-publish.png)

1. Clique no botão **Referências publicadas**. Na caixa de diálogo, verifique se tudo está marcado. Isso incluirá o **Modelo de Site Padrão** e várias configurações geradas pelo Modelo de Site. Clique em **Concluído** para atualizar.

   ![Referências do Publish](assets/author-content-publish/publish-references.png)

1. Por fim, marque a caixa ao lado de **Site WKND** e clique em **Avançar** no canto superior direito.
1. Na etapa **Fluxos de Trabalho**, insira um **título de Fluxo de Trabalho**. Pode ser qualquer texto e pode ser útil como parte de uma trilha de auditoria posteriormente. Insira &quot;Publicação inicial&quot; e clique em **Publish**.

![Publicação inicial da etapa do fluxo de trabalho](assets/author-content-publish/workflow-step-publish.png)

## Exibir conteúdo publicado {#publish}

Em seguida, navegue até o serviço Publish para visualizar as alterações.

1. Uma maneira fácil de obter a URL do Serviço Publish é copiar a URL do Autor e substituir a palavra `author` por `publish`. Por exemplo:

   * **URL do Autor** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **URL DO Publish** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. Adicione `/content/wknd.html` à URL do Publish para que a URL final seja semelhante a: `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > Altere `wknd.html` para corresponder ao nome do seu site, se você forneceu um nome exclusivo durante a [criação do site](create-site.md).

1. Ao navegar para o URL do Publish, você deve ver o site, sem nenhuma funcionalidade de criação do AEM.

   ![Site publicado](assets/author-content-publish/publish-url-update.png)

1. Usando o menu **Navegação**, clique em **Artigo** > **Olá, mundo** para navegar até a página Olá, mundo criada anteriormente.
1. Retorne ao **Serviço de Autor do AEM** e faça algumas alterações de conteúdo adicionais no Editor de Páginas.
1. Publish Faça essas alterações diretamente no editor de páginas clicando no ícone **Propriedades da página** > **Página do Publish**

   ![publicar direto](assets/author-content-publish/page-editor-publish.png)

1. Retorne ao **Serviço Publish do AEM** para exibir as alterações. Muito provavelmente você **não** verá as atualizações imediatamente. Isso ocorre porque o **Serviço Publish do AEM** inclui o [armazenamento em cache por meio de um servidor Web Apache e CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html). Por padrão, o conteúdo de HTML é armazenado em cache por ~5 minutos.

1. Para ignorar o cache para fins de teste/depuração, basta adicionar um parâmetro de consulta como `?nocache=true`. A URL seria semelhante a `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. Mais detalhes sobre a estratégia e as configurações de armazenamento em cache disponíveis [podem ser encontrados aqui](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. Você também pode encontrar o URL para o serviço Publish no Cloud Manager. Navegue até o **Programa Cloud Manager** > **Ambientes** > **Ambiente**.

   ![Exibir Serviço Publish](assets/author-content-publish/view-environment-segments.png)

   Em **Segmentos de ambiente**, você pode encontrar links para os serviços **Author** e **Publish**.

## Parabéns. {#congratulations}

Parabéns, você acabou de criar e publicar alterações no seu site AEM!

### Próximas etapas {#next-steps}

Em uma implementação real, o planejamento de um site com modelos e designs de interface do usuário normalmente precede a criação do Site. Saiba como os Kits de Interface do Usuário do Adobe XD podem ser usados para projetar e acelerar sua implementação do Adobe Experience Manager Sites no [planejamento de interface do usuário com o Adobe XD](./ui-planning-adobe-xd.md).

Deseja continuar explorando os recursos do AEM Sites? Fique à vontade para pular diretamente para o capítulo em [Modelos de página](./page-templates.md) para entender a relação entre um Modelo de página e uma Página.


