---
title: Introdução à criação e publicação | Criação rápida de sites no AEM
description: Use o Editor de páginas no Adobe Experience Manager, AEM, para atualizar o conteúdo do site. Saiba como os Componentes são usados para facilitar a criação. Entenda a diferença entre um autor de AEM e ambientes de publicação e saiba como publicar alterações no site em tempo real.
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
duration: 355
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1285'
ht-degree: 1%

---

# Introdução à criação e publicação {#author-content-publish}

É importante entender como um usuário atualizará o conteúdo do site. Neste capítulo, adotaremos a persona de um **Autor de conteúdo** e faça algumas atualizações editoriais no site gerado no capítulo anterior. No final do capítulo, publicaremos as alterações para entender como o site ativo é atualizado.

## Pré-requisitos {#prerequisites}

Este é um tutorial em várias partes e presume-se que as etapas descritas no [Criar um site](./create-site.md) capítulo foram concluídas.

## Objetivo {#objective}

1. Entenda os conceitos do **Páginas** e **Componentes** no AEM Sites.
1. Saiba como atualizar o conteúdo do site.
1. Saiba como publicar alterações no site em tempo real.

## Criar uma nova página {#create-page}

Um site normalmente é dividido em páginas para formar uma experiência de várias páginas. O AEM estrutura o conteúdo da mesma maneira. Em seguida, crie uma nova página para o site.

1. Faça logon no AEM **Autor** Serviço usado no capítulo anterior.
1. Na tela inicial do AEM, clique em **Sites** > **Site da WKND** > **Inglês** > **Artigo**
1. No canto superior direito, clique em **Criar** > **Página**.

   ![Criar página](assets/author-content-publish/create-page-button.png)

   Isso trará à tona o **Criar página** assistente.

1. Escolha o **Página do artigo** e clique em **Próxima**.

   As páginas no AEM são criadas com base em um Modelo de página. Os Modelos de página são explorados detalhadamente na seção [Modelos de página](page-templates.md) capítulo.

1. Em **Propriedades** insira um **Título** de &quot;Hello World&quot;.
1. Defina o **Nome** para ser `hello-world` e clique em **Criar**.

   ![Propriedades da página inicial](assets/author-content-publish/initial-page-properties.png)

1. Na caixa de diálogo pop-up, clique em **Abertura** para abrir a página recém-criada.

## Criar um componente {#author-component}

Componentes AEM podem ser considerados pequenos blocos modulares de uma página da Web. Ao dividir a interface em partes lógicas ou Componentes, fica muito mais fácil gerenciar. Para reutilizar componentes, eles devem ser configuráveis. Isso é realizado por meio da caixa de diálogo do autor.

O AEM fornece um conjunto de [Componentes principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR) que estão prontos para uso na produção. A variável **Componentes principais** variam de elementos básicos como [Texto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) e [Imagem](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=pt-BR) para elementos de interface do usuário mais complexos, como um [Carrossel](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html?lang=pt-BR).

Em seguida, crie alguns componentes usando o Editor de páginas AEM.

1. Navegue até a **Olá, mundo** página criada no exercício anterior.
1. Verifique se você está em **Editar** e, no painel lateral esquerdo, clique no link **Componentes** ícone.

   ![Painel lateral do editor de páginas](assets/author-content-publish/page-editor-siderail.png)

   Isso abrirá a biblioteca de Componentes e listará os Componentes disponíveis que podem ser usados na página.

1. Role para baixo e **Arrastar e soltar** a **Texto (v2)** componente na região editável principal da página.

   ![Arrastar e soltar componente de texto](assets/author-content-publish/drag-drop-text-cmp.png)

1. Clique em **Texto** para realçar e, em seguida, clique no botão **chave inglesa** ícone ![Ícone de chave inglesa](assets/author-content-publish/wrench-icon.png) para abrir a caixa de diálogo do componente. Insira algum texto e salve as alterações na caixa de diálogo.

   ![Componente de Rich Text](assets/author-content-publish/rich-text-populated-component.png)

   A variável **Texto** O componente agora deve exibir o rich text na página.

1. Repita as etapas acima, exceto arraste uma instância da variável **Imagem(v2)** componente na página. Abra o **Imagem** caixa de diálogo do componente.

1. No painel esquerdo, alterne para a **Localizador de ativos** clicando no link **Assets** ícone ![ícone de ativo](assets/author-content-publish/asset-icon.png).
1. **Arrastar e soltar** uma imagem na caixa de diálogo do Componente e clique em **Concluído** para salvar as alterações.

   ![Adicionar ativo à caixa de diálogo](assets/author-content-publish/add-asset-dialog.png)

1. Observe que há componentes na página, como o **Título**, **Navegação**, **Pesquisar** que são fixos. Essas áreas são configuradas como parte do Modelo de página e não podem ser modificadas em uma página individual. Isso é explorado mais no próximo capítulo.

Experimente alguns dos outros componentes. Documentação sobre cada [O componente principal pode ser encontrado aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR). Uma série detalhada de vídeos sobre [A criação de página pode ser encontrada aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## Publicar atualizações {#publish-updates}

Os ambientes AEM são divididos entre um **Serviço do autor** e uma **Serviço de publicação**. Neste capítulo, fizemos várias modificações no site na **Serviço do autor**. Para que os visitantes do site visualizem as alterações, precisamos publicá-las na **Serviço de publicação**.

![Diagrama de alto nível](assets/author-content-publish/author-publish-high-level-flow.png)

*Fluxo de alto nível de conteúdo do Autor para a Publicação*

**1.** Autores de conteúdo fazem atualizações no conteúdo do site. As atualizações podem ser visualizadas, revisadas e aprovadas para serem enviadas ao vivo.

**2.** O conteúdo é publicado. A publicação pode ser feita sob demanda ou programada para uma data futura.

**3.** Os visitantes do site verão as alterações refletidas no serviço de Publicação.

### Publicar as alterações

Em seguida, vamos publicar as alterações.

1. Na tela inicial do AEM, acesse **Sites** e selecione o **Site da WKND**.
1. Clique em **Gerenciar publicação** na barra de menus.

   ![Gerenciar publicação](assets/author-content-publish/click-manage-publiciation.png)

   Como esse é um site totalmente novo, queremos publicar todas as páginas e podemos usar o assistente Gerenciar publicação para definir exatamente o que precisa ser publicado.

1. Em **Opções** deixe as configurações padrão como **Publish** e agendar para **Agora**. Clique em **Avançar**.
1. Em **Escopo**, selecione o **Site da WKND** e clique em **Incluir configurações secundárias**. Na caixa de diálogo, selecione **Incluir filhos**. Desmarque o restante das caixas para garantir que o site inteiro seja publicado.

   ![Atualizar escopo de publicação](assets/author-content-publish/update-scope-publish.png)

1. Clique em **Referências publicadas** botão. Na caixa de diálogo, verifique se tudo está marcado. Isso incluirá a **Modelo de site padrão** e várias configurações geradas pelo Modelo de site. Clique em **Concluído** para atualizar.

   ![Publicar referências](assets/author-content-publish/publish-references.png)

1. Por fim, marque a caixa ao lado de **Site da WKND** e clique em **Próxima** no canto superior direito.
1. No **Fluxos de trabalho** etapa, insira um **Título do fluxo de trabalho**. Pode ser qualquer texto e pode ser útil como parte de uma trilha de auditoria posteriormente. Insira &quot;Publicação inicial&quot; e clique em **Publish**.

![Publicação inicial da etapa do fluxo de trabalho](assets/author-content-publish/workflow-step-publish.png)

## Exibir conteúdo publicado {#publish}

Em seguida, navegue até o serviço de Publicação para visualizar as alterações.

1. Uma maneira fácil de obter o URL do serviço de publicação é copiar o URL do autor e substituir o `author` palavra com `publish`. Por exemplo:

   * **URL do autor** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **URL de publicação** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. Adicionar `/content/wknd.html` ao URL de publicação para que o URL final seja semelhante a: `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > Alterar `wknd.html` para corresponder ao nome do site, se você forneceu um nome exclusivo durante [criação do site](create-site.md).

1. Ao navegar até o URL de publicação, você deve ver o site, sem nenhuma das funcionalidades de criação do AEM.

   ![Site publicado](assets/author-content-publish/publish-url-update.png)

1. Usar o **Navegação** clique no menu **Artigo** > **Olá, mundo** para navegar até a página Olá, mundo, criada anteriormente.
1. Retorne para a **Serviço de Autor do AEM** e faça algumas alterações de conteúdo adicionais no Editor de páginas.
1. Publique essas alterações diretamente no editor de páginas, clicando no link **Propriedades da página** ícone > **Publicar página**

   ![publicar diretamente](assets/author-content-publish/page-editor-publish.png)

1. Retorne para a **Serviço de publicação do AEM** para visualizar as alterações. Provavelmente você **não** veja imediatamente as atualizações. Isso ocorre porque a variável **Serviço de publicação do AEM** inclui [armazenamento em cache por meio de um servidor Web Apache e CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html). Por padrão, o conteúdo de HTML é armazenado em cache por ~5 minutos.

1. Para ignorar o cache para fins de teste/depuração, basta adicionar um parâmetro de consulta como `?nocache=true`. O URL seria semelhante a `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. Mais detalhes sobre a estratégia e as configurações de armazenamento em cache disponíveis [pode ser encontrado aqui](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. Você também pode encontrar o URL para o serviço de publicação no Cloud Manager. Navegue até a **Programa do Cloud Manager** > **Ambientes** > **Ambiente**.

   ![Exibir serviço de publicação](assets/author-content-publish/view-environment-segments.png)

   Em **Segmentos de ambiente** você pode encontrar links para a **Autor** e **Publish** serviços.

## Parabéns. {#congratulations}

Parabéns, você acabou de criar e publicar alterações no seu site AEM!

### Próximas etapas {#next-steps}

Em uma implementação real, o planejamento de um site com modelos e designs de interface do usuário normalmente precede a criação do Site. Saiba como os Kits de interface do usuário do Adobe XD podem ser usados para projetar e acelerar a implementação do Adobe Experience Manager Sites no [Planejamento da interface do usuário com o Adobe XD](./ui-planning-adobe-xd.md).

Deseja continuar explorando os recursos do AEM Sites? Fique à vontade para pular diretamente para o capítulo em [Modelos de página](./page-templates.md) para entender a relação entre um Modelo de página e uma Página.


