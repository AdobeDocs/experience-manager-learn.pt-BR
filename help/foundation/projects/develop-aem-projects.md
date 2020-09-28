---
title: Desenvolver projetos em AEM
description: Um tutorial de desenvolvimento que ilustra como desenvolver projetos AEM.  Neste tutorial, criaremos um modelo de Projeto personalizado que pode ser usado para criar novos projetos dentro do AEM para gerenciar workflows e tarefas de criação de conteúdo.
version: 6.3, 6.4, 6.5
feature: projects, workflow
topics: collaboration, development, governance
activity: develop
audience: developer, implementer, administrator
doc-type: tutorial
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '4652'
ht-degree: 0%

---


# Desenvolver projetos em AEM

Este é um tutorial de desenvolvimento que ilustra como desenvolver [!DNL AEM Projects].  Neste tutorial, criaremos um modelo de Projeto personalizado que pode ser usado para criar novos projetos dentro do AEM para gerenciar workflows e tarefas de criação de conteúdo.

>[!VIDEO](https://video.tv.adobe.com/v/16904/?quality=12&learn=on)

*Este vídeo apresenta uma breve demonstração do fluxo de trabalho concluído criado no tutorial abaixo.*

## Introdução {#introduction}

[O [!DNL AEM Projects]](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html) é um recurso de AEM projetado para facilitar o gerenciamento e o agrupamento de todos os workflows e tarefas associados à criação de conteúdo como parte de uma implementação do AEM Sites ou Assets.

AEM Projetos vêm com vários modelos [](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTemplates)de projeto OOTB. Ao criar um novo projeto, os autores podem escolher entre esses modelos disponíveis. Grandes implementações AEM com requisitos comerciais exclusivos desejarão criar modelos personalizados de Projeto, adaptados às suas necessidades. Ao criar um modelo de projeto personalizado, os desenvolvedores podem configurar o painel do projeto, conectar-se a workflows personalizados e criar funções comerciais adicionais para um projeto. Vamos dar uma olhada na estrutura de um Modelo de Projeto e criar uma amostra.

![Cartão de Projeto Personalizado](./assets/develop-aem-projects/custom-project-card.png)

## Configurar

Este tutorial percorrerá o código necessário para criar um modelo de projeto personalizado. Você pode baixar e instalar o pacote [](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip) anexado em um ambiente local para acompanhar o tutorial. Você também pode acessar o projeto Maven completo hospedado no [GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide).

* [Pacote do tutorial concluído](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [Repositório completo de código no GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)

Este tutorial assume algum conhecimento básico sobre [AEM práticas](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/the-basics.html) de desenvolvimento e alguma familiaridade com [AEM configuração](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ht-projects-maven.html)do projeto Maven. Todos os códigos mencionados devem ser usados como referência e devem ser implantados somente em uma instância [AEM de desenvolvimento](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingStarted)local.

## Estrutura de um modelo de projeto

Os Modelos de projeto devem ser colocados sob o controle de origem e devem estar ativos abaixo da pasta do aplicativo em /apps. Idealmente, eles devem ser colocados em uma subpasta com a convenção de nomenclatura de ***/projects/models/**&lt;my-template>. Ao seguir esta convenção de nomenclatura, todos os novos modelos personalizados serão disponibilizados automaticamente para os autores ao criar um projeto. A configuração dos modelos de projeto disponíveis está definida em: **/content/projects/jcr:content** node pela propriedade **cq:allowTemplates** . Por padrão, essa é uma expressão comum: **/(aplicativos|libs)/.*/projects/models/.***

O nó raiz de um Modelo de projeto terá um **jcr:PrimaryType** de **cq:Template**. Abaixo do nó raiz há 3 nós: **gadgets**, **funções** e **workflows**. Esses nós são todos **não:não estruturados**. Abaixo do nó raiz também pode estar um arquivo thumbnail.png que é exibido ao selecionar o modelo no assistente Criar projeto.

A estrutura completa do nó:

```shell
/apps/<my-app>
    + projects (nt:folder)
         + templates (nt:folder)
              + <project-template-root> (cq:Template)
                   + gadgets (nt:unstructured)
                   + roles (nt:unstructured)
                   + workflows (nt:unstructured)
```

### Raiz do modelo de projeto

O nó raiz do modelo de projeto será do tipo **cq:Template**. Neste nó, você pode configurar as propriedades **jcr:title** e **jcr:description** que serão exibidas no Assistente de criação de projeto. Também há uma propriedade chamada **assistente** que aponta para um formulário que preencherá as Propriedades do projeto. O valor padrão de: **/libs/cq/core/content/projects/wizard/steps/defaultproject.html** deve funcionar normalmente na maioria dos casos, pois permite que o usuário preencha as propriedades básicas do projeto e adicione membros do grupo.

**Observe que o Assistente para criação de projeto não usa o servlet Sling POST. Em vez disso, os valores são postados em um servlet personalizado:**com.adobe.cq.projects.impl.servlet.ProjectServlet**. Isso deve ser considerado ao adicionar campos personalizados.*

Um exemplo de um assistente personalizado pode ser encontrado para o Modelo de Projeto de Tradução: **/libs/cq/core/content/projects/Wizard/translationproject/defaultproject**.

### Gadgets {#gadgets}

Não há propriedades adicionais neste nó, mas os filhos do nó gadgets controlam quais blocos de projeto preenchem o painel do projeto quando um novo projeto é criado. [Os blocos](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#ProjectTiles) do projeto (também conhecidos como gadgets ou pods) são cartões simples que preenchem o local de trabalho de um projeto. Em: **/libs/cq/gui/components/projects/admin/pod. **Os proprietários de projetos sempre podem adicionar/remover blocos depois que um projeto é criado.

### Funções {#roles}

Há 3 funções [padrão](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html#UserRolesinaProject) para cada projeto: **Observadores**, **editores** e **proprietários**. Ao adicionar nós filhos abaixo do nó de funções, você pode adicionar outras Funções de Projeto específicas ao negócio para o modelo. Em seguida, é possível vincular essas funções a workflows específicos associados ao projeto.

### Fluxos de trabalhos {#workflows}

Um dos motivos mais interessantes para criar um modelo de projeto personalizado é que ele oferece a você a capacidade de configurar os workflows disponíveis para uso com o projeto. Eles podem workflows OOTB ou workflows personalizados. Abaixo do nó **workflows** , é necessário haver um nó de **modelos** (também `nt:unstructured`) e nós filhos abaixo de especificar os modelos de fluxo de trabalho disponíveis. A propriedade **modelId **aponta para o modelo de fluxo de trabalho em /etc/workflow e o **assistente** de propriedades aponta para a caixa de diálogo usada ao iniciar o fluxo de trabalho. Uma grande vantagem dos Projetos é a capacidade de adicionar uma caixa de diálogo personalizada (assistente) para capturar metadados específicos da empresa no start do fluxo de trabalho que podem impulsionar outras ações no fluxo de trabalho.

```shell
<projects-template-root> (cq:Template)
    + workflows (nt:unstructured)
         + models (nt:unstructured)
              + <workflow-model> (nt:unstructured)
                   - modelId = points to the workflow model
                   - wizard = dialog used to start the workflow
```

## Creating a project template {#creating-project-template}

Como iremos copiar/configurar principalmente nós, usaremos o CRXDE Lite. Em sua instância AEM local abra [CRXDE Lite](http://localhost:4502/crx/de/index.jsp).

1. Start criando uma nova pasta sob o `/apps/&lt;your-app-folder&gt;` nome `projects`. Crie outra pasta abaixo do nome `templates`.

   ```shell
   /apps/aem-guides/projects-tasks/
                       + projects (nt:folder)
                                + templates (nt:folder)
   ```

1. Para facilitar as coisas, start nosso modelo personalizado do modelo Simple Project existente.

   1. Copie e cole o nó **/libs/cq/core/content/projects/models/default** abaixo da pasta de *modelos* criada na Etapa 1.

   ```shell
   /apps/aem-guides/projects-tasks/
                + templates (nt:folder)
                     + default (cq:Template)
   ```

1. Agora você deve ter um caminho como **/apps/aem-guides/projects-tarefa/projects/models/authoring-project**.

   1. Edite as propriedades **jcr:title** e **jcr:description** do nó autor-projeto para valores personalizados de título e descrição.

      1. Deixe a propriedade do **assistente** apontando para as propriedades padrão do Projeto.

   ```shell
   /apps/aem-guides/projects-tasks/projects/
            + templates (nt:folder)
                 + authoring-project (cq:Template)
                      - jcr:title = "Authoring Project"
                      - jcr:description = "A project to manage approval and publish process for AEM Sites or Assets"
                      - wizard = "/libs/cq/core/content/projects/wizard/steps/defaultproject.html"
   ```

1. Para este modelo de projeto, queremos usar o Tarefa.
   1. Adicione um novo nó **não:não estruturado** abaixo de authoring-project/gadgets chamado **tarefa**.
   1. Adicione as propriedades String ao nó tarefa para **cardWeight** = &quot;100&quot;, **jcr:title**=&quot;Tarefa&quot; e **sling:resourceType**=&quot;cq/gui/components/projects/admin/pod/taskpod&quot;.

   Agora, o bloco [](https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#Tasks) Tarefa será exibido por padrão quando um novo projeto for criado.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
            + team (nt:unstructured)
            + asset (nt:unstructured)
            + work (nt:unstructured)
            + experiences (nt:unstructured)
            + projectinfo (nt:unstructured)
            ..
            + tasks (nt:unstructured)
                 - cardWeight = "100"
                 - jcr:title = "Tasks"
                 - sling:resourceType = "cq/gui/components/projects/admin/pod/taskpod"
   ```

1. Adicionaremos uma função de aprovador personalizada ao nosso modelo de projeto.

   1. Abaixo do nó do modelo de projeto (autoring-project), adicione um novo nó **nt:unstructure** rotulado como **funções**.
   1. Adicione outro nó **não:não estruturado** rotulado aprovadores como filho do nó de funções.
   1. Adicionar propriedades de sequência de caracteres **jcr:title** = &quot;**Aprovadores**&quot;, **roleclass** =&quot;**proprietário**&quot;, **roleid******=&quot;aprovvers&quot;.
      1. O nome do nó aprovvers, bem como jcr:title e roleid podem ser qualquer valor de string (desde que roleid seja exclusivo).
      1. **o roleclass** governa as permissões aplicadas a essa função com base nas [3 funções]OOTB (https://docs.adobe.com/docs/en/aem/6-3/author/projects.html#User Funções em um projeto): **proprietário**, **editor** e **observador**.
      1. Em geral, se o papel de gestão for mais importante, o **proprietário pode ser o chefe da carteira;** se for uma função de autoria mais específica, como o Fotógrafo ou o Designer, o **editor** de roleta deverá ser suficiente. A grande diferença entre o **proprietário** e o **editor** é que os proprietários do projeto podem atualizar as propriedades do projeto e adicionar novos usuários ao projeto.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
           + approvers (nt:unstructured)
                - jcr:title = "Approvers"
                - roleclass = "owner"
                - roleid = "approver"
   ```

1. Ao copiar o modelo de Projeto simples, você terá quatro workflows OOTB configurados. Cada nó abaixo de workflows/modelos aponta para um fluxo de trabalho específico e um assistente de diálogo de start para esse fluxo de trabalho. Mais tarde neste tutorial, criaremos um fluxo de trabalho personalizado para este projeto. Por enquanto, exclua os nós abaixo do fluxo de trabalho/modelos:

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
            + models (nt:unstructured)
               - (remove ootb models)
   ```

1. Para facilitar a identificação dos autores de conteúdo no Modelo de projeto, adicione uma miniatura personalizada. O tamanho recomendado seria de 319x319 pixels.
   1. No CRXDE Lite, crie um novo arquivo como um irmão de gadgets, funções e nós de workflows chamados **thumbnail.png**.
   1. Salve e navegue até o `jcr:content` nó e o duplo clique na `jcr:data` propriedade (evite clicar em &#39;visualização&#39;).
      1. Isso deve avisá-lo com uma caixa de diálogo de edição `jcr:data` e você pode fazer upload de uma miniatura personalizada.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
       + thumbnail.png (nt:file)
   ```

Representação XML concluída do modelo de projeto:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:description="A project to manage approval and publish process for AEM Sites or Assets"
    jcr:primaryType="cq:Template"
    jcr:title="Authoring Project"
    ranking="{Long}1"
    wizard="/libs/cq/core/content/projects/wizard/steps/defaultproject.html">
    <jcr:content
        jcr:primaryType="nt:unstructured"
        detailsHref="/projects/details.html"/>
    <gadgets jcr:primaryType="nt:unstructured">
        <team
            jcr:primaryType="nt:unstructured"
            jcr:title="Team"
            sling:resourceType="cq/gui/components/projects/admin/pod/teampod"
            cardWeight="60"/>
        <tasks
            jcr:primaryType="nt:unstructured"
            jcr:title="Tasks"
            sling:resourceType="cq/gui/components/projects/admin/pod/taskpod"
            cardWeight="100"/>
        <work
            jcr:primaryType="nt:unstructured"
            jcr:title="Workflows"
            sling:resourceType="cq/gui/components/projects/admin/pod/workpod"
            cardWeight="80"/>
        <experiences
            jcr:primaryType="nt:unstructured"
            jcr:title="Experiences"
            sling:resourceType="cq/gui/components/projects/admin/pod/channelpod"
            cardWeight="90"/>
        <projectinfo
            jcr:primaryType="nt:unstructured"
            jcr:title="Project Info"
            sling:resourceType="cq/gui/components/projects/admin/pod/projectinfopod"
            cardWeight="100"/>
    </gadgets>
    <roles jcr:primaryType="nt:unstructured">
        <approvers
            jcr:primaryType="nt:unstructured"
            jcr:title="Approvers"
            roleclass="owner"
            roleid="approvers"/>
    </roles>
    <workflows
        jcr:primaryType="nt:unstructured"
        tags="[]">
        <models jcr:primaryType="nt:unstructured">
        </models>
    </workflows>
</jcr:root>
```

## Teste do modelo de projeto personalizado

Agora podemos testar nosso modelo de projeto criando um novo projeto.

1. Você deve ver o modelo personalizado como uma das opções para a criação do projeto.

   ![Escolher modelo](./assets/develop-aem-projects/choose-template.png)

1. Depois de selecionar o modelo personalizado, clique em &#39;Avançar&#39; e observe que ao preencher os Membros do projeto você pode adicioná-los como uma função de Aprovador.

   ![Aprovar](./assets/develop-aem-projects/user-approver.png)

1. Clique em &quot;Criar&quot; para concluir a criação do projeto com base no modelo personalizado. Você observará no Painel do projeto que o Tarefa Tile e os outros blocos configurados em gadgets são exibidos automaticamente.

   ![Mosaico de tarefas](./assets/develop-aem-projects/tasks-tile.png)


## Por que fluxo de trabalho?

Tradicionalmente, os workflows AEM centralizados em um processo de aprovação usavam as etapas do fluxo de trabalho do Participante. AEM Caixa de entrada inclui detalhes sobre Tarefas e fluxo de trabalho e integração aprimorada com AEM projetos. Esses recursos tornam a utilização das etapas do processo Criar Tarefa de projetos uma opção mais atraente.

### Por que Tarefas?

Usando uma Etapa de criação de Tarefa sobre as etapas tradicionais do participante oferta algumas vantagens:

* **Start e data** de vencimento - facilita o gerenciamento do tempo dos autores, o novo recurso Calendário utiliza essas datas.
* **Prioridade** - prioridades incorporadas de Baixo, Normal e Alto permitem que os autores priorizem o trabalho
* **Comentários** segmentados - à medida que os autores trabalham em uma tarefa, eles têm a capacidade de deixar comentários aumentando a colaboração
* **Visibilidade** - blocos de Tarefa e visualizações com Projetos permitem que os gerentes visualizações como o tempo está sendo gasto
* **Integração** do projeto - O Tarefa já está integrado com funções e painéis do projeto

Como as etapas do Participante, as Tarefas podem ser atribuídas e roteadas dinamicamente. Metadados de tarefa, como Título, Prioridade também podem ser definidos dinamicamente com base em ações anteriores, como veremos com o tutorial a seguir.

Embora as Tarefas tenham algumas vantagens sobre as Etapas do participante, elas transportam sobrecarga adicional e não são tão úteis fora de um projeto. Além disso, todo o comportamento dinâmico das Tarefas deve ser codificado usando scripts de ecma que tenham suas próprias limitações.

## Requisitos do caso de utilização da amostra {#goals-tutorial}

![Diagrama do processo de fluxo de trabalho](./assets/develop-aem-projects/workflow-process-diagram.png)

O diagrama acima descreve os requisitos de alto nível para nosso fluxo de trabalho de aprovação de amostra.

A primeira etapa será criar uma Tarefa para concluir a edição de um conteúdo. Permitiremos que o iniciador do fluxo de trabalho escolha o destinatário dessa primeira tarefa.

Quando a primeira tarefa for concluída, o destinatário terá três opções para o roteamento do fluxo de trabalho:

**Normal **- o roteamento normal cria uma tarefa atribuída ao grupo Aprovador do projeto para revisão e aprovação. A prioridade da tarefa é Normal e a data de vencimento é de 5 dias a partir do momento em que é criada.

**O roteamento Rush** - rush também cria uma tarefa atribuída ao grupo Aprovador do projeto. A prioridade da tarefa é Alta e a data de vencimento é de apenas 1 dia.

**Ignorar** - neste fluxo de trabalho de amostra o participante inicial tem a opção de ignorar o grupo de aprovadores. (sim, isso pode derrotar a finalidade de um fluxo de trabalho de &quot;Aprovação&quot;, mas permite ilustrar recursos adicionais do roteamento)

O Grupo do Aprovador pode aprovar o conteúdo ou enviá-lo de volta ao destinatário inicial para retrabalhar. Em caso de reenvio para retrabalho, uma nova tarefa é criada e adequadamente rotulada como &quot;Enviado para retrabalho&quot;.

A última etapa do fluxo de trabalho usa a etapa do processo Ativar página/ativo da guia e replica a carga.

## Criar o modelo de fluxo de trabalho

1. No menu Start AEM, navegue até Ferramentas -> Fluxo de trabalho -> Modelos. Clique em &quot;Criar&quot; no canto superior direito para criar um novo Modelo de fluxo de trabalho.

   Dê um título ao novo modelo: &quot;Fluxo de trabalho de aprovação de conteúdo&quot; e um nome de url: &quot;fluxo de trabalho de aprovação de conteúdo&quot;.

   ![Caixa de diálogo Criar fluxo de trabalho](./assets/develop-aem-projects/workflow-create-dialog.png)

   Para obter mais informações relacionadas à [criação de workflows, leia aqui](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-models.html).

1. Como prática recomendada, os workflows personalizados devem ser agrupados em sua própria pasta abaixo de /etc/workflow/models. No CRXDE Lite, crie uma nova **&#39;nt:folder&#39;** abaixo de /etc/workflow/models chamada **&quot;aem-guides&quot;**. Adicionar uma subpasta garante que os workflows personalizados não sejam substituídos acidentalmente durante as atualizações ou instalações do Service Pack.

   *Observe que é importante nunca colocar a pasta ou os workflows personalizados abaixo das subpastas da guia como /etc/workflow/models/dam ou /etc/workflow/models/projects, já que a subpasta inteira também pode ser substituída por atualizações ou service packs.

   ![Localização do modelo de fluxo de trabalho na versão 6.3](./assets/develop-aem-projects/custom-workflow-subfolder.png)

   Localização do modelo de fluxo de trabalho na versão 6.3

   >[!NOTE]
   >
   >Se estiver usando AEM 6.4+, a localização do Fluxo de trabalho foi alterada. Consulte [aqui para obter mais detalhes.](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   Se estiver usando AEM 6.4+, o modelo de fluxo de trabalho será criado em `/conf/global/settings/workflow/models`. Repita as etapas acima com o diretório /conf e adicione uma subpasta nomeada `aem-guides` e mova-a para `content-approval-workflow` baixo dela.

   ![Local](./assets/develop-aem-projects/modern-workflow-definition-location.png)de definição de fluxo de trabalho modernoLocal do modelo de fluxo de trabalho em 6.4+

1. Introduzido no AEM 6.3 é a capacidade de adicionar Etapas de fluxo de trabalho a um determinado fluxo de trabalho. Os estágios serão exibidos para o usuário na Caixa de entrada na guia Informações do fluxo de trabalho. Ele mostrará ao usuário o estágio atual no fluxo de trabalho, bem como os estágios anteriores e posteriores.

   Para configurar os estágios, abra a caixa de diálogo Propriedades da página no SideKick. A quarta guia é chamada de &quot;Estágios&quot;. Adicione os seguintes valores para configurar as três etapas deste fluxo de trabalho:

   1. Editar conteúdo
   1. Aprovação
   1. Publicação

   ![configuração dos estágios do fluxo de trabalho](./assets/develop-aem-projects/workflow-model-stage-properties.png)

   Configure os estágios do fluxo de trabalho na caixa de diálogo Propriedades da página.

   ![barra de progresso do fluxo de trabalho](./assets/develop-aem-projects/workflow-info-progress.png)

   A barra de progresso do fluxo de trabalho, como visto na Caixa de entrada AEM.

   Opcionalmente, você pode fazer upload de uma **imagem** para as Propriedades da página que serão usadas como a miniatura do Fluxo de trabalho quando os usuários a selecionarem. As dimensões da imagem devem ter 319x319 pixels. A adição de uma **Descrição** às Propriedades da página também será exibida quando um usuário for selecionar o fluxo de trabalho.

1. O processo de fluxo de trabalho Criar Tarefa do projeto foi criado para criar uma Tarefa como uma etapa no fluxo de trabalho. Somente após a conclusão da tarefa o fluxo de trabalho avançará. Um aspecto poderoso da etapa Criar Tarefa do projeto é que ela pode ler os valores de metadados do fluxo de trabalho e usá-los para criar dinamicamente a tarefa.

   Primeiro exclua a Etapa do participante que é criada por padrão. No Sidekick no menu de componentes, expanda o subtítulo **&quot;Projetos&quot;** e arraste e solte a **&quot;Tarefa de criação de projeto&quot;** no modelo.

   Duplo+Clique na etapa &quot;Criar Tarefa do projeto&quot; para abrir a caixa de diálogo do fluxo de trabalho. Configure as seguintes propriedades:

   Essa guia é comum para todas as etapas do processo de fluxo de trabalho e definiremos o Título e a Descrição (eles não estarão visíveis para o usuário final). A propriedade importante que definiremos é a Etapa do fluxo de trabalho para **&quot;Editar conteúdo&quot;** no menu suspenso.

   ```shell
   Common Tab
   -----------------
       Title = "Start Task Creation"
       Description = "This the first task in the Workflow"
       Workflow Stage = "Edit Content"
   ```

   O processo de fluxo de trabalho Criar Tarefa do projeto foi criado para criar uma Tarefa como uma etapa no fluxo de trabalho. A guia Tarefa permite definir todos os valores da tarefa. Em nosso caso, queremos que o Destinatário seja dinâmico, então deixaremos em branco. O restante dos valores de propriedade:

   ```shell
   Task Tab
   -----------------
       Name* = "Edit Content"
       Task Priority = "Medium"
       Description = "Edit the content and finalize for approval. Once finished submit for approval."
       Due In - Days = "2"
   ```

   A guia roteamento é uma caixa de diálogo opcional que pode especificar ações disponíveis para o usuário que está concluindo a tarefa. Essas ações são apenas valores de sequência de caracteres e serão salvas nos metadados do fluxo de trabalho. Esses valores podem ser lidos por scripts e/ou etapas de processo posteriormente no fluxo de trabalho para &quot;rotear&quot; dinamicamente o fluxo de trabalho. Com base nas metas [do](#goals-tutorial) fluxo de trabalho, adicionaremos três ações a esta guia:

   ```shell
   Routing Tab
   -----------------
       Actions =
           "Normal Approval"
           "Rush Approval"
           "Bypass Approval"
   ```

   Esta guia permite configurar um Script de Tarefa de pré-criação, no qual podemos decidir programaticamente vários valores da Tarefa antes de ela ser criada. Temos a opção de apontar o script para um arquivo externo ou incorporar um script curto diretamente na caixa de diálogo. Em nosso caso, apontaremos o script de Tarefa Pre-Create para um arquivo externo. Na Etapa 5 criaremos esse script.

   ```shell
   Advanced Settings Tab
   -----------------
      Pre-Create Task Script = "/apps/aem-guides/projects/scripts/start-task-config.ecma"
   ```

1. Na etapa anterior, referenciamos um script de Tarefa de pré-criação. Criaremos esse script agora no qual definiremos o Destinatário da Tarefa com base no valor de um valor de metadados de fluxo de trabalho &quot;**destinatário**&quot;. O valor **&quot;destinatário&quot;** será definido quando o fluxo de trabalho for desligado. Nós também leremos os metadados do fluxo de trabalho para escolher dinamicamente a prioridade da tarefa lendo o valor &quot;**taskPriority&quot;** dos metadados do fluxo de trabalho, bem como **&quot;taskdueDate&quot; **para definir dinamicamente quando a primeira tarefa estiver vencida.

   Para fins organizacionais, criamos uma pasta abaixo da pasta do aplicativo para armazenar todos os scripts relacionados ao projeto: **/apps/aem-guides/projects-tarefa/projects/scripts**. Crie um novo arquivo abaixo desta pasta chamado **&quot;start-tarefa-config.ecma&quot;**. *Observe que o caminho para o arquivo start-tarefa-config.ecma corresponde ao caminho definido na guia Configurações avançadas na Etapa 4.

   Adicione o seguinte como conteúdo do arquivo:

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   ```

1. Navegue até o Fluxo de trabalho de aprovação de conteúdo. Arraste e solte o componente **OU Dividir** (localizado no Sidekick abaixo da categoria &#39;Fluxo de trabalho&#39;) abaixo da Etapa de Tarefa **do** Start. Na caixa de diálogo Comum, selecione o botão de opção para 3 Ramificações. A Divisão OR lerá o valor de metadados do fluxo de trabalho **&quot;lastTaskAction&quot;** para determinar a rota do fluxo de trabalho. A propriedade **&quot;lastTaskAction&quot;** será definida como um dos valores da guia Roteamento configurada na Etapa 4. Para cada uma das guias Ramificação, preencha a área de texto **Script** com os seguintes valores:

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Normal Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Rush Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Bypass Approval") {
       return true;
   }
   
   return false;
   }
   ```

   *Observe que estamos fazendo uma correspondência direta de String para determinar a rota, portanto, é importante que os valores definidos nos scripts de Ramificação correspondam aos valores de Rota definidos na Etapa 4.

1. Arraste e solte outra etapa &quot;**Criar Tarefa** do projeto&quot; no modelo à esquerda (Ramificação 1) abaixo da divisão OU. Preencha a caixa de diálogo com as seguintes propriedades:

   ```
   Common Tab
   -----------------
       Title = "Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is Medium."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Approve Content for Publish"
       Task Priority = "Medium"
       Description = "Approve this content for publication."
       Days = "5"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   Como essa é a rota de Aprovação normal, a prioridade da tarefa é definida como Média. Além disso, damos ao grupo Aprovadores 5 dias para completar a Tarefa. O destinatário é deixado em branco na guia Tarefa, pois isso será atribuído dinamicamente na guia Configurações avançadas. Damos ao grupo Aprovadores duas rotas possíveis ao completar esta tarefa: **&quot;Aprovar e publicar&quot;** se eles aprovarem o conteúdo e ele puder ser publicado e **&quot;Enviar para revisão&quot;** se houver problemas que o editor original precise corrigir. O aprovador pode deixar comentários que o editor original verá se o fluxo de trabalho for retornado para ele.

No começo deste tutorial, criamos um Modelo de projeto que incluía uma Função de Aprovadores. Cada vez que um novo Projeto é criado a partir deste Modelo, um Grupo específico do projeto será criado para a função Aprovadores. Assim como uma Etapa do participante, uma Tarefa só pode ser atribuída a um usuário ou grupo. Queremos atribuir essa tarefa ao grupo de projetos que corresponde ao Grupo de Aprovadores. Todos os workflows que são iniciados de dentro de um Projeto terão metadados que mapeiam as Funções do Projeto para o grupo específico do Projeto.

Copie+Cole o seguinte código na área de texto **Script** da guia **Configurações avançadas **tab. Esse código lerá os metadados do fluxo de trabalho e atribuirá a tarefa ao grupo Aprovadores do projeto. Se ele não conseguir localizar o valor do grupo de aprovadores, ele voltará para atribuir a tarefa ao grupo Administradores.

```
var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");

task.setCurrentAssignee(projectApproverGrp);
```

1. Arraste e solte outra etapa &quot;**Criar Tarefa** do projeto&quot; no modelo até a ramificação central (Ramificação 2) abaixo da divisão OU. Preencha a caixa de diálogo com as seguintes propriedades:

   ```
   Common Tab
   -----------------
       Title = "Rush Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is High."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Rush Approve Content for Publish"
       Task Priority = "High"
       Description = "Rush approve this content for publication."
       Days = "1"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   Como esta é a rota de Aprovação de Rush, a prioridade da tarefa é definida como Alta. Além disso, damos ao grupo Aprovadores apenas um dia para completar a tarefa. O destinatário é deixado em branco na guia Tarefa, pois isso será atribuído dinamicamente na guia Configurações avançadas.

   Podemos reutilizar o mesmo snippet de script da Etapa 7 para preencher a área de texto do **Script** na guia** Advanced Settings **s. Copiar+Colar o código abaixo:

   ```
   var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");
   
   task.setCurrentAssignee(projectApproverGrp);
   ```

1. Arraste e solte um componente** Sem operação** na ramificação da extrema direita (Ramificação 3). O componente Sem operação não executa nenhuma ação e será avançado imediatamente, representando o desejo do editor original de ignorar a etapa de aprovação. Tecnicamente, podemos deixar essa Ramificação sem quaisquer etapas do fluxo de trabalho, mas como prática recomendada, adicionaremos uma etapa Sem operação. Isso deixa claro para outros desenvolvedores qual é o objetivo do Branch 3.

   Duplo clique na etapa de fluxo de trabalho e configure o Título e a Descrição:

   ```
   Common Tab
   -----------------
       Title = "Bypass Approval"
       Description = "Placeholder step to indicate that the original editor decided to bypass the approver group."
   ```

   ![modelo OU divisão de fluxo de trabalho](./assets/develop-aem-projects/workflow-stage-after-orsplit.png)

   O Modelo de Fluxo de Trabalho deve ser semelhante a este depois que todas as três ramificações na divisão OU tiverem sido configuradas.

1. Como o grupo Aprovadores tem a opção de enviar o fluxo de trabalho de volta ao editor original para revisões adicionais, contaremos com a etapa **Ir para** ler a última ação executada e direcionar o fluxo de trabalho para o início ou permitir que continue.

   Arraste e solte o componente Etapa de goto (encontrado no Sidekick em Fluxo de trabalho) abaixo da divisão OU onde ele se junta novamente. Clique no duplo e configure as seguintes propriedades na caixa de diálogo:

   ```
   Common Tab
   ----------------
       Title = "Goto Step"
       Description = "Based on the Approver groups action route the workflow to the beginning or continue and publish the payload."
   
   Process Tab
   ---------------
       The step to go to. = "Start Task Creation"
   ```

   A última peça que configuraremos será o Script como parte da etapa de processo Ir para. O valor do Script pode ser incorporado pela caixa de diálogo ou configurado para apontar para um arquivo externo. O Script Goto deve conter uma **função check()** e retornar true se o fluxo de trabalho precisar ir para a etapa especificada. Retorno de falsos resultados em andamento no fluxo de trabalho.

   Se o grupo do aprovador escolher a ação **&quot;Enviar para revisão&quot;** (configurada nas Etapas 7 e 8), retornaremos o fluxo de trabalho para a etapa **&quot;Criação de Tarefa de Start&quot;** .

   Na guia Processo, adicione o seguinte trecho à área de texto Script:

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Send Back for Revision") {
       return true;
   }
   
   return false;
   }
   ```

1. Para publicar a carga, usaremos a etapa **Ativar página/Processo de ativo** da guia. Esta etapa do processo requer pouca configuração e adicionará a carga do fluxo de trabalho à fila de replicação para ativação. Adicionaremos a etapa abaixo da etapa Ir para e, dessa forma, ela só poderá ser atingida se o grupo Aprovador tiver aprovado o conteúdo para publicação ou se o editor original escolher a rota Ignorar aprovação.

   Arraste e solte a etapa **Ativar página/processo de ativo** (encontrada no Sidekick em Fluxo de trabalho do WCM) abaixo da Etapa Ir para no modelo.

   ![modelo de fluxo de trabalho concluído](assets/develop-aem-projects/workflow-model-final.png)

   Como o modelo de fluxo de trabalho deve ser exibido depois de adicionar a etapa Ir para e a etapa Ativar página/ativo.

1. Se o grupo Aprovador enviar o conteúdo de volta para revisão, informaremos o editor original. Isso pode ser feito alterando dinamicamente as propriedades de criação de Tarefas. Nós desativaremos o valor da propriedade lastActionTaken de **&quot;Send Back for Revision&quot;**. Se esse valor estiver presente, modificaremos o título e a descrição para indicar que essa tarefa é o resultado do conteúdo enviado de volta para revisão. Também atualizaremos a prioridade para **&quot;Alto&quot;** para que seja o primeiro item em que o editor trabalha. Por fim, definiremos a data de vencimento da tarefa como um dia a partir do momento em que o fluxo de trabalho foi enviado de volta para revisão.

   Substitua o script do start `start-task-config.ecma` (criado na Etapa 5) pelo seguinte:

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   //change the title and priority if the approver group sent back the content
   if(lastAction == "Send Back for Revision") {
     var taskName = "Review and Revise Content";
   
     //since the content was rejected we will set the priority to High for the revison task
     task.setProperty("taskPriority", "High"); 
   
     //set the Task name (displayed as the task title in the Inbox) 
     task.setProperty("name", taskName);
     task.setProperty("nameHierarchy", taskName);
   
     //set the due date of this task 1 day from current date
     var calDueDate = Packages.java.util.Calendar.getInstance();
     calDueDate.add(Packages.java.util.Calendar.DATE, 1);
     task.setProperty("taskDueDate", calDueDate.getTime());
   
   }
   ```

## Criar o assistente de &quot;fluxo de trabalho do start&quot; {#start-workflow-wizard}

Ao encerrar um fluxo de trabalho de dentro de um projeto, você deve especificar um assistente para start do fluxo de trabalho. O assistente padrão: `/libs/cq/core/content/projects/workflowwizards/default_workflow` permite que o usuário insira um Título do fluxo de trabalho, um comentário do start e um caminho de carga para que o fluxo de trabalho seja executado. Há também vários outros exemplos encontrados em: `/libs/cq/core/content/projects/workflowwizards`.

A criação de um assistente personalizado pode ser muito poderosa, pois você pode coletar informações críticas antes dos start do fluxo de trabalho. Os dados são armazenados como parte dos metadados do fluxo de trabalho e os processos do fluxo de trabalho podem ler isso e alterar dinamicamente o comportamento com base nos valores inseridos. Criaremos um assistente personalizado para atribuir dinamicamente a primeira tarefa no fluxo de trabalho com base em um valor do assistente de start.

1. No CRXDE-Lite, criaremos uma subpasta abaixo da `/apps/aem-guides/projects-tasks/projects` pasta chamada &quot;assistentes&quot;. Copie o assistente padrão de: `/libs/cq/core/content/projects/workflowwizards/default_workflow` abaixo da pasta de assistentes recém-criados e renomeie-a como start **de aprovação de** conteúdo. O caminho completo agora deve ser: `/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start`.

   O assistente padrão é um assistente de 2 colunas com a primeira coluna mostrando Título, Descrição e Miniatura do modelo de fluxo de trabalho selecionado. A segunda coluna inclui campos para o Título do fluxo de trabalho, Comentário do Start e Caminho da carga. O assistente é um formulário de interface de usuário de toque padrão e usa os componentes [padrão de formulário de interface de usuário](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/index.html) Granite para preencher os campos.

   ![assistente de fluxo de trabalho de aprovação de conteúdo](./assets/develop-aem-projects/content-approval-start-wizard.png)

1. Adicionaremos um campo adicional ao assistente que será usado para definir o destinatário da primeira tarefa no fluxo de trabalho (consulte [Criar o modelo](#create-workflow-model)de fluxo de trabalho: Passo 5).

   Abaixo, `../content-approval-start/jcr:content/items/column2/items` crie um novo nó do tipo `nt:unstructured` chamado **&quot;assign&quot;**. Usaremos o componente Seletor de usuários de projetos (que é baseado no Componente [Seletor de usuários](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/userpicker/index.html)Granite). Esse campo de formulário facilita restringir a seleção de usuários e grupos somente àqueles que pertencem ao projeto atual.

   Abaixo está a representação XML do nó de **atribuição** :

   ```xml
   <assign
       granite:class="js-cq-project-user-picker"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="cq/gui/components/projects/admin/userpicker"
       fieldLabel="Assign To"
       hideServiceUsers="{Boolean}true"
       impersonatesOnly="{Boolean}true"
       showOnlyProjectMembers="{Boolean}true"
       name="assignee"
       projectPath="${param.project}"
       required="{Boolean}true"/>
   ```

1. Também adicionaremos um campo de seleção de prioridade que determinará a prioridade da primeira tarefa no fluxo de trabalho (consulte [Criar o modelo](#create-workflow-model)de fluxo de trabalho: Passo 5).

   Abaixo, `/content-approval-start/jcr:content/items/column2/items` crie um novo nó do tipo `nt:unstructured` chamado **priority**. Usaremos o componente [de Seleção de interface do usuário](https://docs.adobe.com/docs/en/aem/6-2/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/select/index.html) Granite para preencher o campo de formulário.

   Abaixo do nó de **prioridade** , adicionaremos um nó **items** de **nt:unstructed**. Abaixo do nó **items** , adicione mais 3 nós para preencher as opções de seleção para Alta, Média e Baixa. Cada nó é do tipo **nt:unstruct** e deve ter uma propriedade **text** e **value** . O texto e o valor devem ser iguais:

   1. Alta
   1. Média
   1. Baixa

   Para o nó Médio, adicione uma propriedade Booliana adicional chamada &quot;**seleted&quot;** com um valor definido como **true**. Isso garantirá que Médio seja o valor padrão no campo de seleção.

   Abaixo está uma representação XML da estrutura e propriedades do nó:

   ```xml
   <priority
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/select"
       fieldLabel="Task Priority"
       name="taskPriority">
           <items jcr:primaryType="nt:unstructured">
               <high
                   jcr:primaryType="nt:unstructured"
                   text="High"
                   value="High"/>
               <medium
                   jcr:primaryType="nt:unstructured"
                   selected="{Boolean}true"
                   text="Medium"
                   value="Medium"/>
               <low
                   jcr:primaryType="nt:unstructured"
                   text="Low"
                   value="Low"/>
               </items>
   </priority>
   ```

1. Permitiremos que o iniciador do fluxo de trabalho defina a data de vencimento da tarefa inicial. Usaremos o campo de formulário DataPicker [da interface do usuário](https://docs.adobe.com/docs/en/aem/6-5/develop/ref/granite-ui/api/jcr_root/libs/granite/ui/components/coral/foundation/form/datepicker/index.html) Granite para capturar essa entrada. Também adicionaremos um campo oculto com uma [TypeHint](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html#typehint) para garantir que a entrada seja armazenada como uma propriedade do tipo Data no JCR.

   Adicione dois nós **nt:não estruturados** com as seguintes propriedades representadas abaixo no XML:

   ```xml
   <duedate
       granite:rel="project-duedate"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/datepicker"
       displayedFormat="YYYY-MM-DD HH:mm"
       fieldLabel="Due Date"
       minDate="today"
       name="taskDueDate"
       type="datetime"/>
   <duedatetypehint
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/hidden"
       name="taskDueDate@TypeHint"
       type="datetime"
       value="Calendar"/>
   ```

1. Você pode visualização o código completo da caixa de diálogo do assistente de start [aqui](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/projects-tasks-guide/ui.apps/src/main/content/jcr_root/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start/.content.xml).

## Conexão do fluxo de trabalho e do modelo de projeto {#connecting-workflow-project}

A última coisa que precisamos fazer é garantir que o modelo de fluxo de trabalho esteja disponível para ser retirado de dentro de um dos Projetos. Para fazer isso, precisamos visitar novamente o Modelo de Projeto que criamos na Parte 1 desta série.

A configuração do Fluxo de trabalho é uma área de um Modelo de projeto que especifica os workflows disponíveis a serem usados com esse projeto. A configuração também é responsável por especificar o Assistente de Fluxo de Trabalho do Start ao encerrar o fluxo de trabalho (criado nas etapas [anteriores)](#start-workflow-wizard). A configuração de Fluxo de Trabalho de um Modelo de Projeto é &quot;ao vivo&quot;, o que significa que a atualização da configuração do fluxo de trabalho afetará novos Projetos criados, bem como Projetos existentes que usam o modelo.

1. No CRXDE-Lite, navegue até o modelo de projeto de criação criado anteriormente em `/apps/aem-guides/projects-tasks/projects/templates/authoring-project/workflows/models`.

   Abaixo do nó modelos, adicione um novo nó chamado **contentApproval** com um tipo de nó **nt:unstructed**. Adicione as seguintes propriedades ao nó:

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/etc/workflow/models/aem-guides/content-approval-workflow/jcr:content/model"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

   >[!NOTE]
   >
   >Se estiver usando AEM 6.4, a localização do Fluxo de trabalho foi alterada. Aponte a `modelId` propriedade para o local do modelo de fluxo de trabalho do tempo de execução em `/var/workflow/models/aem-guides/content-approval-workflow`
   >
   >
   >Consulte [aqui para obter mais detalhes sobre a alteração no local do fluxo de trabalho.](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/workflows-best-practices.html#LocationsWorkflowModels)

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/var/workflow/models/aem-guides/content-approval-workflow"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

1. Depois que o fluxo de trabalho de Aprovação de conteúdo tiver sido adicionado ao Modelo de projeto, ele deverá estar disponível para sair do bloco de fluxo de trabalho do projeto. Vá em frente e lance e brinque com os vários roteamentos que criamos.

## Materiais de suporte

* [Download do pacote de tutorial concluído](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [Repositório completo de código no GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)
* [Documentação de projetos AEM](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects.html)
