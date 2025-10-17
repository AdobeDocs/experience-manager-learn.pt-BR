---
title: Introdução à criação e publicação | Criação rápida de sites do AEM
description: Use o editor de páginas do Adobe Experience Manager (AEM) para atualizar o conteúdo do site. Saiba como os componentes são usados para facilitar a criação. Entenda a diferença entre os ambientes de criação e publicação do AEM, e saiba como publicar alterações no site ativo.
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1285'
ht-degree: 100%

---

# Introdução à criação e publicação {#author-content-publish}

É importante entender como um usuário atualizará o conteúdo do site. Neste capítulo, adotaremos a persona de um **autor de conteúdo** e faremos algumas atualizações editoriais no site gerado no capítulo anterior. No fim do capítulo, publicaremos as alterações para entender como o site ativo é atualizado.

## Pré-requisitos {#prerequisites}

Este é um tutorial em várias partes, e presume-se que as etapas descritas no capítulo [Criar um site](./create-site.md) tenham sido concluídas.

## Objetivo {#objective}

1. Entender os conceitos de **Páginas** e **Componentes** no AEM Sites.
1. Aprender a atualizar o conteúdo do site.
1. Aprender a publicar alterações no site ativo.

## Criar uma nova página {#create-page}

Um site normalmente é dividido em páginas para formar uma experiência de várias páginas. O AEM estrutura o conteúdo da mesma maneira. Em seguida, crie uma nova página para o site.

1. Faça logon no serviço de **Criação** do AEM usado no capítulo anterior.
1. Na tela inicial do AEM, clique em **Sites** > **Site da WKND** > **Português** > **Artigo**
1. No canto superior direito, clique em **Criar** > **Página**.

   ![Criar página](assets/author-content-publish/create-page-button.png)

   Isso exibirá o assistente **Criar página**.

1. Escolha o modelo da **Página de artigo** e clique em **Próximo**.

   As páginas no AEM são criadas com base em um modelo de página. Os modelos de página são detalhados no capítulo [Modelos de página](page-templates.md).

1. Em **Propriedades**, digite o **Título** “Olá, mundo”.
1. Defina o **Nome** como `hello-world` e clique em **Criar**.

   ![Propriedades da página inicial](assets/author-content-publish/initial-page-properties.png)

1. Na caixa de diálogo pop-up, clique em **Abrir** para abrir a página recém-criada.

## Criar um componente {#author-component}

Os componentes do AEM podem ser considerados pequenos blocos de construção modulares de uma página da web. Ao dividir a interface em partes lógicas ou componentes, fica muito mais fácil gerenciá-la. Para reutilizar componentes, eles precisam ser configuráveis. Isso é feito por meio da caixa de diálogo de criação.

O AEM fornece um conjunto de [Componentes principais](https://experienceleague.adobe.com/pt-br/docs/experience-manager-core-components/using/introduction) que estão prontos para uso na produção. Os **Componentes principais** variam de elementos básicos, como [Texto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) e [Imagem](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=pt-BR), a elementos da IU mais complexos, como um [Carrossel](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html?lang=pt-BR).

Em seguida, crie alguns componentes com o editor de páginas do AEM.

1. Navegue até a página **Olá, mundo**, criada no exercício anterior.
1. Certifique-se de estar no modo **Editar** e, no painel lateral esquerdo, clique no ícone **Componentes**.

   ![Painel lateral do editor de páginas](assets/author-content-publish/page-editor-siderail.png)

   Isso abrirá a biblioteca de componentes e listará os componentes disponíveis que podem ser usados na página.

1. Role para baixo e **Arraste e solte** um componente de **Texto (v2)** na região editável principal da página.

   ![Arrastar + soltar componente de texto](assets/author-content-publish/drag-drop-text-cmp.png)

1. Selecione o componente de **Texto** para realçá-lo e clique no ícone de **chave inglesa** ![Ícone de chave inglesa](assets/author-content-publish/wrench-icon.png) para abrir a caixa de diálogo do componente. Insira um texto e salve as alterações na caixa de diálogo.

   ![Componente de rich text](assets/author-content-publish/rich-text-populated-component.png)

   O componente de **Texto** agora deve exibir o rich text na página.

1. Repita as etapas acima, mas arrastando uma instância do componente de **Imagem (v2)** para a página. Abra a caixa de diálogo do componente de **Imagem**.

1. No painel à esquerda, alterne para o **Localizador de ativos**, clicando no **ícone de ativos** ![Ícone de ativos](assets/author-content-publish/asset-icon.png).
1. **Arraste e solte** uma imagem na caixa de diálogo do componente e clique em **Concluído** para salvar as alterações.

   ![Adicionar ativo à caixa de diálogo](assets/author-content-publish/add-asset-dialog.png)

1. Observe que há componentes na página, como **Título**, **Navegação** e **Pesquisa**, que são fixos. Essas áreas são configuradas como parte do modelo de página e não podem ser modificadas em uma página individual. Isso será abordado mais a fundo no próximo capítulo.

Experimente alguns dos outros componentes. A documentação sobre cada [componente principal pode ser encontrada aqui](https://experienceleague.adobe.com/pt-br/docs/experience-manager-core-components/using/introduction). Uma série de vídeos detalhados sobre a [criação de páginas pode ser encontrada aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## Publicar atualizações {#publish-updates}

Os ambientes do AEM são divididos entre um **Serviço do criação** e um **Serviço de publicação**. Neste capítulo, fizemos várias modificações no site no **Serviço de criação**. Para que os visitantes do site vejam as alterações, precisamos publicá-las no **Serviço de publicação**.

![Diagrama de alto nível](assets/author-content-publish/author-publish-high-level-flow.png)

*Fluxo de conteúdo de alto nível da criação para a publicação*

**1.** Os criadores de conteúdo fazem atualizações no conteúdo do site. As atualizações podem ser visualizadas, revisadas e aprovadas para entrar no ar.

**2.** O conteúdo é publicado. A publicação pode ser feita sob demanda ou programada para uma data futura.

**3.** Os visitantes do site verão as alterações refletidas no serviço de publicação.

### Publicar as alterações

Em seguida, vamos publicar as alterações.

1. Na tela inicial do AEM, navegue até **Sites** e selecione o **Site da WKND**.
1. Clique em **Gerenciar publicação** na barra do menu.

   ![Gerenciar publicação](assets/author-content-publish/click-manage-publiciation.png)

   Como se trata de um site totalmente novo, temos que publicar todas as páginas e podemos usar o assistente “Gerenciar publicação” para definir exatamente o que precisa ser publicado.

1. Em **Opções**, deixe as configurações padrão como **Publicar** e programe-as para **Agora**. Clique em **Avançar**.
1. Em **Escopo**, selecione o **Site da WKND** e clique em **Incluir configurações secundárias**. Na caixa de diálogo, marque **Incluir secundárias**. Desmarque o restante das caixas para garantir que o site inteiro seja publicado.

   ![Atualizar escopo de publicação](assets/author-content-publish/update-scope-publish.png)

1. Clique no botão **Referências publicadas**. Na caixa de diálogo, verifique se tudo está marcado. Isso inclui o **Modelo de site padrão** e várias configurações geradas pelo modelo de site. Clique em **Concluído** para atualizar.

   ![Publicar referências](assets/author-content-publish/publish-references.png)

1. Por fim, marque a caixa ao lado de **Site da WKND** e clique em **Próximo**, no canto superior direito.
1. Na etapa **Fluxos de trabalho**, insira o **Título do fluxo de trabalho**. Pode ser qualquer texto e pode ser útil como parte de uma trilha de auditoria posteriormente. Digite “Publicação inicial” e clique em **Publicar**.

![Etapa do fluxo de trabalho de publicação inicial](assets/author-content-publish/workflow-step-publish.png)

## Visualizar o conteúdo publicado {#publish}

Em seguida, navegue até o serviço de publicação para visualizar as alterações.

1. Uma maneira fácil de obter o URL do serviço de publicação é copiar o URL da criação e substituir a palavra `author` por `publish`. Por exemplo:

   * **URL da criação**: `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **URL da publicação**: `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. Adicione `/content/wknd.html` ao URL da publicação, de modo que o URL final fique semelhante a: `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > Altere `wknd.html` para corresponder ao nome do seu site, caso tenha fornecido um nome exclusivo durante a [criação do site](create-site.md).

1. Ao navegar até o URL da publicação, você deve ver o site, sem nenhum dos recursos de criação do AEM.

   ![Site publicado](assets/author-content-publish/publish-url-update.png)

1. Usando o menu **Navegação**, clique em **Artigo** > **Olá, mundo** para navegar até a página “Olá, mundo” criada anteriormente.
1. Retorne ao **Serviço de criação do AEM** e faça algumas alterações adicionais no conteúdo dentro do editor de páginas.
1. Publique essas alterações diretamente no editor de páginas, clicando no ícone **Propriedades da página** > **Publicar página**

   ![publicação direta](assets/author-content-publish/page-editor-publish.png)

1. Retorne ao **Serviço de publicação do AEM** para visualizar as alterações. É muito provável que você **não** veja as atualizações imediatamente. Isso ocorre porque o **Serviço de publicação do AEM** inclui o [armazenamento em cache por meio de um servidor da web Apache e uma CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html). Por padrão, o conteúdo do HTML é armazenado em cache por ~5 minutos.

1. Para ignorar o cache para fins de teste/depuração, basta adicionar um parâmetro de consulta, como `?nocache=true`. O URL ficaria semelhante a `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. Mais detalhes sobre a estratégia de armazenamento em cache e as configurações disponíveis [podem ser encontrados aqui](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. Você também pode encontrar o URL do serviço de publicação no Cloud Manager. Navegue até **Programa do Cloud Manager** > **Ambientes** > **Ambiente**.

   ![Visualizar o serviço de publicação](assets/author-content-publish/view-environment-segments.png)

   Em **Segmentos de ambiente**, você pode encontrar links para os serviços de **Criação** e **Publicação**.

## Parabéns! {#congratulations}

Parabéns, você acabou de criar e publicar as alterações no seu site do AEM.

### Próximas etapas {#next-steps}

Em uma implementação real, o planejamento de um site com simulações e designs da IU normalmente precede a criação do site. Saiba como os kits da IU do Adobe XD podem ser usados para projetar e acelerar a sua implementação do Adobe Experience Manager Sites no [planejamento da IU com o Adobe XD](./ui-planning-adobe-xd.md).

Deseja continuar conhecendo os recursos do AEM Sites? Sinta-se à vontade para pular diretamente para o capítulo de [Modelos de página](./page-templates.md) para entender a relação entre um modelo de página e uma página.


