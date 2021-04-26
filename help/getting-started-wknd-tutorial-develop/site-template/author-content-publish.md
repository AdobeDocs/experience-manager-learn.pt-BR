---
title: Criar conteúdo e publicar alterações
seo-title: Introdução ao AEM Sites - Aumentar o conteúdo e publicar as alterações
description: Use o editor de páginas no Adobe Experience Manager, AEM, para atualizar o conteúdo do site. Saiba como os Componentes são usados para facilitar a criação. Entenda a diferença entre um autor do AEM e ambientes de publicação e saiba como publicar alterações no site ativo.
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Gerenciamento de conteúdo
feature: Componentes principais, Editor de páginas
role: Developer
level: Beginner
kt: 7497
thumbnail: KT-7497.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '1306'
ht-degree: 2%

---


# Aumentar o conteúdo e publicar as alterações {#author-content-publish}

>[!CAUTION]
>
> Os recursos rápidos de criação de sites mostrados aqui serão lançados na segunda metade de 2021. A documentação relacionada está disponível para fins de visualização.

É importante entender como um usuário atualizará o conteúdo do site. Neste capítulo, adotaremos a persona de um **Content Author** e faremos algumas atualizações editoriais no site gerado no capítulo anterior. No final do capítulo, publicaremos as alterações para entender como o site ativo é atualizado.

## Pré-requisitos {#prerequisites}

Este é um tutorial de várias partes e é pressuposto que as etapas descritas no capítulo [Criar um site](./create-site.md) foram concluídas.

## Objetivo {#objective}

1. Entenda os conceitos de **Pages** e **Components** no AEM Sites.
1. Saiba como atualizar o conteúdo do site.
1. Saiba como publicar alterações no site ativo.

## Criar uma nova página {#create-page}

Normalmente, um site é dividido em páginas para formar uma experiência com várias páginas. AEM estrutura o conteúdo da mesma maneira. Em seguida, crie uma nova página para o site.

1. Faça logon no AEM **Author** Service usado no capítulo anterior.
1. Na tela inicial AEM, clique em **Sites** > **Site WKND** > **Inglês** > **Artigo**
1. No canto superior direito, clique em **Create** > **Page**.

   ![Criar página](assets/author-content-publish/create-page-button.png)

   Isso exibirá o assistente **Criar página**.

1. Escolha o modelo **Página do artigo** e clique em **Próximo**.

   As páginas em AEM são criadas com base em um modelo de página. Os Modelos de página serão explorados com mais detalhes no capítulo [Modelos de página](page-templates.md).

1. Em **Properties** digite um **Title** de &quot;Hello World&quot;.
1. Defina o **Nome** para ser `hello-world` e clique em **Criar**.

   ![Propriedades da página inicial](assets/author-content-publish/initial-page-properties.png)

1. No pop-up da caixa de diálogo, clique em **Abrir** para abrir a página recém-criada.

## Criar um componente {#author-component}

AEM Os componentes podem ser considerados como pequenos blocos componentes modulares de uma página da Web. Ao quebrar a interface em partes ou componentes lógicos, é muito mais fácil gerenciar. Para reutilizar componentes, eles devem ser configuráveis. Isso é feito por meio da caixa de diálogo do autor.

AEM fornece um conjunto de [Componentes principais](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR) que estão prontos para uso na produção. Os **Componentes principais** variam de elementos básicos como [Texto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) e [Imagem](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) para elementos de interface mais complexos, como um [Carrossel](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html).

Em seguida, vamos criar alguns componentes usando AEM Editor de páginas.

1. Navegue até a página **Hello World** criada no exercício anterior.
1. Certifique-se de estar no modo **Edit** e, no painel lateral esquerdo, clique no ícone **Components**.

   ![Painel lateral do editor de páginas](assets/author-content-publish/page-editor-siderail.png)

   Isso abrirá a biblioteca de Componentes e listará os Componentes disponíveis que podem ser usados na página.

1. Role para baixo e **Arraste e solte** um componente **Texto (v2)** na região editável principal da página.

   ![Arrastar + Soltar componente de texto](assets/author-content-publish/drag-drop-text-cmp.png)

1. Clique no componente **Texto** para realçar e, em seguida, clique no ícone **chave** ![Chave icon](assets/author-content-publish/wrench-icon.png) para abrir a caixa de diálogo Componente. Insira algum texto e salve as alterações na caixa de diálogo.

   ![Componente de Rich Text](assets/author-content-publish/rich-text-populated-component.png)

   O componente **Text** agora deve exibir o rich text na página.

1. Repita as etapas acima, exceto para arrastar uma instância do componente **Image(v2)** para a página. Abra a caixa de diálogo do componente **Image**.

1. No painel à esquerda, alterne para o **Localizador de ativos** clicando no ícone **Ativos** ![ícone de ativo](assets/author-content-publish/asset-icon.png).
1. **Arraste e** solte a imagem na caixa de diálogo do Componente e clique em  **** Fazer para salvar as alterações.

   ![Adicionar ativo à caixa de diálogo](assets/author-content-publish/add-asset-dialog.png)

1. Observe que há componentes na página, como o **Title**, **Navigation**, **Search** que são corrigidos. Essas áreas são configuradas como parte do Modelo de página e não podem ser modificadas em uma página individual. Isso será mais explorado no próximo capítulo.

Sinta-se à vontade para experimentar alguns dos outros componentes. A documentação sobre cada [Componente principal pode ser encontrada aqui](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html). Uma série de vídeo detalhada sobre [Criação de página pode ser encontrada aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## Publicar atualizações {#publish-updates}

AEM ambientes são divididos entre um **Serviço de Autor** e um **Serviço de Publicação**. Neste capítulo, fizemos várias modificações no site no **Serviço de Autor**. Para que os visitantes do site visualizem as alterações, precisamos publicá-las no **Serviço de publicação**.

![Diagrama de alto nível](assets/author-content-publish/author-publish-high-level-flow.png)

*Fluxo de alto nível de conteúdo de Autor para Publicação*

**1.** Os autores de conteúdo fazem atualizações no conteúdo do site. As atualizações podem ser visualizadas, revisadas e aprovadas para serem colocadas online.

**2.** O conteúdo foi publicado. A publicação pode ser executada sob demanda ou programada para uma data futura.

**3.** Os visitantes do site verão as alterações refletidas no serviço de publicação.

### Publicar as alterações

Em seguida, vamos publicar as alterações.

1. Na tela inicial AEM, navegue até **Sites** e selecione o **Site WKND**.
1. Clique em **Gerenciar publicação** na barra de menus.

   ![Gerenciar publicação](assets/author-content-publish/click-manage-publiciation.png)

   Como esse site é totalmente novo, queremos publicar todas as páginas e podemos usar o assistente Gerenciar publicação para definir exatamente o que precisa ser publicado.

1. Em **Options** deixe as configurações padrão para **Publish** e agende-as para **Now**. Clique em **Avançar**.
1. Em **Escopo**, selecione o **Site WKND** e clique em **Incluir Filhos**. Na caixa de diálogo , desmarque todas as caixas. Queremos publicar o site completo.

   ![Atualizar escopo de publicação](assets/author-content-publish/update-scope-publish.png)

1. Clique no botão **Referências publicadas**. Na caixa de diálogo, verifique se tudo está marcado. Isso incluirá o **Modelo básico de site AEM** e várias configurações geradas pelo Modelo de site. Clique em **Concluído** para atualizar.

   ![Publicar referências](assets/author-content-publish/publish-references.png)

1. Finalmente, clique em **Publish** no canto superior direito para publicar o conteúdo.

## Exibir conteúdo publicado {#publish}

Em seguida, navegue até o serviço Publicar para exibir as alterações.

1. Uma maneira fácil de obter o URL do Serviço de publicação é copiar o url do autor e substituir a palavra `author` por `publish`. Por exemplo:

   * **URL do autor** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **URL de publicação**  -  `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. Adicione `/content/wknd.html` ao URL de publicação para que o URL final tenha a seguinte aparência: `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > Altere `wknd.html` para corresponder ao nome do site, se você tiver fornecido um nome exclusivo durante [criação do site](create-site.md).

1. Ao navegar até o URL de publicação, você deve ver o site, sem nenhuma das funcionalidades de criação do AEM.

   ![Site publicado](assets/author-content-publish/publish-url-update.png)

1. Usando o menu **Navigation** clique em **Article** > **Hello World** para navegar até a página Hello World criada anteriormente.
1. Retorne ao **AEM Author Service** e faça algumas alterações de conteúdo adicionais no Editor de páginas.
1. Publique essas alterações diretamente no editor de páginas clicando no ícone **Propriedades da página** > **Publicar página**

   ![publicar direto](assets/author-content-publish/page-editor-publish.png)

1. Retorne ao **AEM Publish Service** para visualizar as alterações. Provavelmente, você **não** verá as atualizações imediatamente. Isso ocorre porque o **AEM Publish Service** inclui [armazenamento em cache via um servidor Web Apache e CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html). Por padrão, o conteúdo HTML é armazenado em cache por ~5 minutos.

1. Para ignorar o cache para fins de teste/depuração, basta adicionar um parâmetro de consulta como `?nocache=true`. O URL seria semelhante a `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. Mais detalhes sobre a estratégia de armazenamento em cache e as configurações disponíveis [podem ser encontrados aqui](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. Você também pode encontrar o URL para o Serviço de publicação no Cloud Manager. Navegue até **Cloud Manager Program** > **Ambientes** > **Ambiente**.

   ![Exibir serviço de publicação](assets/author-content-publish/view-environment-segments.png)

   Em **Segmentos do ambiente** você pode encontrar links para os serviços **Author** e **Publish**.

## Parabéns! {#congratulations}

Parabéns, você acabou de criar e publicar as mudanças no seu site AEM!

### Próximas etapas {#next-steps}

Saiba como criar e modificar [Modelos de página](./page-templates.md). Entenda a relação entre um modelo de página e uma página. Saiba como configurar as políticas de um modelo de página para fornecer governança granular e consistência da marca para o conteúdo.  Um modelo bem estruturado de artigo de revista será criado com base em um modelo do Adobe XD.
