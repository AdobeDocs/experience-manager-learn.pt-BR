---
title: Desenvolver projetos no AEM
description: Um tutorial de desenvolvimento que ilustra como desenvolver projetos no AEM. Neste tutorial, criaremos um modelo de projeto personalizado que pode ser usado para criar novos projetos dentro do AEM para gerenciar fluxos de trabalho e tarefas de criação de conteúdo.
version: 6.4, 6.5
feature: Projects, Workflow
doc-type: Tutorial
topic: Development
role: Developer
level: Beginner
exl-id: 9bfe3142-bfc1-4886-85ea-d1c6de903484
duration: 1417
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '4441'
ht-degree: 0%

---

# Desenvolver projetos no AEM

Este é um tutorial de desenvolvimento que ilustra como desenvolver para [!DNL AEM Projects]. Neste tutorial, criaremos um modelo de projeto personalizado que pode ser usado para criar projetos dentro do AEM para gerenciar fluxos de trabalho e tarefas de criação de conteúdo.

>[!VIDEO](https://video.tv.adobe.com/v/16904?quality=12&learn=on)

*Este vídeo fornece uma breve demonstração do fluxo de trabalho concluído que é criado no tutorial abaixo.*

## Introdução {#introduction}

[[!DNL AEM Projects]](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects) é um recurso do AEM criado para facilitar o gerenciamento e o agrupamento de todos os fluxos de trabalho e tarefas associados à criação de conteúdo como parte de uma implementação do AEM Sites ou do Assets.

Projetos AEM vem com vários [modelos de projeto OOTB](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects). Ao criar um projeto, os autores podem escolher entre esses modelos disponíveis. Grandes implementações de AEM com requisitos comerciais exclusivos desejarão criar modelos de projeto personalizados, personalizados para atender às suas necessidades. Ao criar um modelo de projeto personalizado, os desenvolvedores podem configurar o painel do projeto, entrar em fluxos de trabalho personalizados e criar funções de negócios adicionais para um projeto. Examinaremos a estrutura de um modelo de projeto e criaremos um modelo de amostra.

![Cartão de Projeto Personalizado](./assets/develop-aem-projects/custom-project-card.png)

## Configurar

Este tutorial guiará o código necessário para criar um modelo de projeto personalizado. Você pode baixar e instalar o [pacote anexado](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip) em um ambiente local para seguir junto com o tutorial. Você também pode acessar o projeto Maven completo hospedado em [GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide).

* [Pacote de Tutorial Concluído](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [Repositório de Código Completo no GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)

Este tutorial presume algum conhecimento básico sobre as [práticas de desenvolvimento do AEM](https://experienceleague.adobe.com/br/docs/experience-manager-65/content/implementing/developing/introduction/the-basics) e alguma familiaridade com a [configuração do projeto AEM Maven](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html?lang=en). Todos os códigos mencionados devem ser usados como referência e só devem ser implantados em uma [instância de AEM de desenvolvimento local](https://experienceleague.adobe.com/br/docs/experience-manager-65/content/implementing/deploying/deploying/deploy).

## Estrutura de um modelo de projeto

Os modelos de projeto devem ser colocados no controle do código-fonte e devem estar localizados abaixo da pasta do aplicativo em /apps. Idealmente, eles devem ser colocados em uma subpasta com a convenção de nomenclatura de **&#42;/projetos/modelos/**&lt;meu-modelo>. Ao seguir essa convenção de nomenclatura, qualquer novo modelo personalizado ficará disponível automaticamente para os autores ao criar um projeto. A configuração de Modelos de Projeto disponíveis foi definida no nó: **/content/projects/jcr:content** pela propriedade **cq:allowedTemplates**. Por padrão, esta é uma expressão regular: **/(apps|libs)/.&#42;/projetos/modelos/.&#42;**

O nó raiz de um Modelo de Projeto terá um **jcr:primaryType** de **cq:Template**. Abaixo do nó raiz há três nós: **gadgets**, **funções** e **fluxos de trabalho**. Estes nós são todos **nt:unstructured**. Abaixo do nó raiz também pode haver um arquivo thumbnail.png que é exibido ao selecionar o modelo no assistente Criar projeto.

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

O nó raiz do modelo de projeto é do tipo **cq:Template**. Neste nó, você pode configurar as propriedades **jcr:title** e **jcr:description** exibidas no Assistente para Criar Projeto. Também há uma propriedade chamada **assistente** que aponta para um formulário que irá preencher as Propriedades do projeto. O valor padrão de: **/libs/cq/core/content/projects/wizard/steps/defaultproject.html** deve funcionar bem para a maioria dos casos, pois permite que o usuário preencha as propriedades básicas do Projeto e adicione membros do grupo.

*&#42;Observe que o Assistente para Criar Projeto não usa o servlet Sling POST. Em vez disso, os valores são postados em um servlet personalizado:**com.adobe.cq.projects.impl.servlet.ProjectServlet**. Isso deve ser levado em conta ao adicionar campos personalizados.*

Um exemplo de assistente personalizado pode ser encontrado para o Modelo de projeto de tradução: **/libs/cq/core/content/projects/wizard/translationproject/defaultproject**.

### Gadgets {#gadgets}

Não há propriedades adicionais nesse nó, mas os filhos do nó gadgets controlam quais Blocos de projeto preenchem o painel do projeto quando um novo projeto é criado. [Os Blocos de Projeto](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects) (também conhecidos como gadgets ou pods) são cartões simples que preenchem o local de trabalho de um Projeto. Uma lista completa de blocos ootb pode ser encontrada em: **/libs/cq/gui/components/projects/admin/pod. **Os proprietários do projeto sempre podem adicionar/remover blocos após a criação de um projeto.

### Funções {#roles}

Há três [Funções padrão](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects) para cada projeto: **Observadores**, **Editores** e **Proprietários**. Ao adicionar nós filhos abaixo do nó funções, é possível adicionar outras Funções do projeto específicas da empresa para o modelo. Em seguida, é possível vincular essas funções a fluxos de trabalho específicos associados ao projeto.

### Fluxos de trabalhos {#workflows}

Um dos motivos mais atraentes para criar um modelo de projeto personalizado é que ele oferece a capacidade de configurar os fluxos de trabalho disponíveis para uso com o projeto. Eles podem usar workflows OOTB ou personalizados. Abaixo do nó **workflows**, deve haver um nó **models** (também `nt:unstructured`) e nós filhos abaixo de especificar os modelos de fluxo de trabalho disponíveis. A propriedade **modelId **aponta para o modelo de fluxo de trabalho em /etc/workflow e a propriedade **wizard** aponta para a caixa de diálogo usada ao iniciar o fluxo de trabalho. Uma vantagem significativa dos Projetos é a capacidade de adicionar uma caixa de diálogo personalizada (assistente) para capturar metadados específicos de negócios no início do fluxo de trabalho que podem impulsionar outras ações dentro do fluxo de trabalho.

```shell
<projects-template-root> (cq:Template)
    + workflows (nt:unstructured)
         + models (nt:unstructured)
              + <workflow-model> (nt:unstructured)
                   - modelId = points to the workflow model
                   - wizard = dialog used to start the workflow
```

## Criação de um modelo de projeto {#creating-project-template}

Como estamos principalmente copiando/configurando nós, usaremos o CRXDE Lite. Na instância local do AEM, abra [CRXDE Lite](http://localhost:4502/crx/de/index.jsp).

1. Comece criando uma pasta abaixo de `/apps/&lt;your-app-folder&gt;` chamada `projects`. Crie outra pasta abaixo de `templates`.

   ```shell
   /apps/aem-guides/projects-tasks/
                       + projects (nt:folder)
                                + templates (nt:folder)
   ```

1. Para facilitar as coisas, iniciaremos nosso modelo personalizado a partir do modelo existente de Projeto simples.

   1. Copie e cole o nó **/libs/cq/core/content/projects/templates/default** abaixo da pasta *templates* criada na Etapa 1.

   ```shell
   /apps/aem-guides/projects-tasks/
                + templates (nt:folder)
                     + default (cq:Template)
   ```

1. Agora você deve ter um caminho como **/apps/aem-guides/projects-tasks/projects/templates/authoring-project**.

   1. Edite as propriedades **jcr:title** e **jcr:description** do nó author-project para personalizar os valores de título e descrição.

      1. Deixe a propriedade **wizard** apontando para as propriedades padrão do Projeto.

   ```shell
   /apps/aem-guides/projects-tasks/projects/
            + templates (nt:folder)
                 + authoring-project (cq:Template)
                      - jcr:title = "Authoring Project"
                      - jcr:description = "A project to manage approval and publish process for AEM Sites or Assets"
                      - wizard = "/libs/cq/core/content/projects/wizard/steps/defaultproject.html"
   ```

1. Neste modelo de projeto, queremos usar Tarefas.
   1. Adicione um novo nó **nt:unstructured** abaixo de authoring-project/gadgets chamado **tasks**.
   1. Adicione propriedades de Cadeia de Caracteres ao nó de tarefas para **cardWeight** = &quot;100&quot;, **jcr:title**=&quot;Tasks&quot; e **sling:resourceType**=&quot;cq/gui/components/projects/admin/pod/taskpod&quot;.

   Agora, o [bloco Tarefas](https://experienceleague.adobe.com/en/docs) será exibido por padrão quando um novo projeto for criado.

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

1. Adicionaremos uma Função de aprovador personalizada ao modelo do projeto.

   1. Abaixo do nó do modelo de projeto (authoring-project) adicione um novo nó **nt:unstructured** com rótulo de nó **roles**.
   1. Adicione outro nó **nt:unstructured** rotulado como aprovadores secundários do nó de funções.
   1. Adicionar propriedades de cadeia de caracteres **jcr:title** = &quot;**Approvers**&quot;, **roleclass** =&quot;**owner**&quot;, **roleid**=&quot;**approvers**&quot;.
      1. O nome do nó de aprovadores, bem como jcr:title e roleid podem ser qualquer valor de string (desde que roleid seja exclusivo).
      1. A **classe de função** rege as permissões aplicadas para essa função com base nas [três funções OOTB](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects): **proprietário**, **editor** e **observador**.
      1. Em geral, se a função personalizada for mais de uma função gerencial, a classe de função poderá ser **proprietário;** se for uma função de criação mais específica, como Fotógrafo ou Designer, a classe de função **editor** será suficiente. A grande diferença entre o **proprietário** e o **editor** é que os proprietários do projeto podem atualizar as propriedades do projeto e adicionar novos usuários ao projeto.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
           + approvers (nt:unstructured)
                - jcr:title = "Approvers"
                - roleclass = "owner"
                - roleid = "approver"
   ```

1. Ao copiar o modelo Projeto simples, você obterá quatro fluxos de trabalho OOTB configurados. Cada nó abaixo de workflows/models aponta para um workflow específico e um assistente de caixa de diálogo inicial para esse workflow. Posteriormente neste tutorial, criaremos um fluxo de trabalho personalizado para este projeto. Por enquanto, exclua os nós abaixo de workflow/models:

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
            + models (nt:unstructured)
               - (remove ootb models)
   ```

1. Para facilitar para que os autores de conteúdo identifiquem o Modelo de projeto, você pode adicionar uma miniatura personalizada. O tamanho recomendado é de 319x319 pixels.
   1. No CRXDE Lite, crie um arquivo como um irmão de gadgets, funções e nós de fluxos de trabalho chamados **thumbnail.png**.
   1. Salve, navegue até o nó `jcr:content` e clique duas vezes na propriedade `jcr:data` (evite clicar em &quot;exibir&quot;).
      1. Isso deve solicitar uma caixa de diálogo para editar arquivo `jcr:data` e você pode carregar uma miniatura personalizada.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
       + thumbnail.png (nt:file)
   ```

Representação XML concluída do Modelo de Projeto:

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

Agora podemos testar nosso Modelo de projeto criando um projeto.

1. Você deve ver o modelo personalizado como uma das opções para a criação do projeto.

   ![Escolher Modelo](./assets/develop-aem-projects/choose-template.png)

1. Depois de selecionar o modelo personalizado, clique em &#39;Próximo&#39; e observe que, ao preencher Membros do projeto, você pode adicioná-los como uma função de Aprovador.

   ![Aprovar](./assets/develop-aem-projects/user-approver.png)

1. Clique em &quot;Criar&quot; para concluir a criação do projeto com base no Modelo personalizado. Você observará no Painel do projeto que o Bloco de tarefas e os outros blocos configurados em gadgets são exibidos automaticamente.

   ![Bloco de Tarefas](./assets/develop-aem-projects/tasks-tile.png)


## Por que o fluxo de trabalho?

Tradicionalmente, os workflows do AEM que se centralizam em um processo de aprovação usavam as etapas do fluxo de trabalho do Participante. A Caixa de entrada do AEM inclui detalhes sobre Tarefas e Fluxo de trabalho e integração aprimorada com Projetos AEM. Esses recursos tornam o uso das etapas do processo Criar tarefa do projeto uma opção mais atraente.

### Por que tarefas?

O uso de uma Etapa de criação de tarefa em relação às etapas tradicionais do Participante oferece algumas vantagens:

* **Data de Início e de Conclusão** - facilita o gerenciamento de horas por parte dos autores; o novo recurso Calendário utiliza essas datas.
* **Prioridade** - as prioridades internas de Baixa, Normal e Alta permitem que os autores priorizem o trabalho
* **Comentários encadeados** - à medida que os autores trabalham em uma tarefa, eles podem deixar comentários aumentando a colaboração
* **Visibilidade** - Os mosaicos e as exibições de tarefas com Projetos permitem que os gerentes vejam como o tempo está sendo gasto
* **Integração do Project** - as tarefas já estão integradas às funções e aos painéis do Project

Como as etapas do Participante, as Tarefas podem ser atribuídas e roteadas dinamicamente. Metadados de tarefas como Título, Prioridade também podem ser definidos dinamicamente com base em ações anteriores, como veremos no tutorial a seguir.

Embora as Tarefas tenham algumas vantagens em relação às Etapas do participante, elas carregam uma sobrecarga adicional e não são tão úteis fora de um Projeto. Além disso, todo comportamento dinâmico de Tarefas deve ser codificado usando scripts ecma que têm suas próprias limitações.

## Requisitos de caso de uso de exemplo {#goals-tutorial}

![Diagrama de processo de fluxo de trabalho](./assets/develop-aem-projects/workflow-process-diagram.png)

O diagrama acima descreve os requisitos de alto nível para nosso fluxo de trabalho de aprovação de amostra.

A primeira etapa é criar uma Tarefa para concluir a edição de um conteúdo. Permitiremos que o iniciador do fluxo de trabalho escolha o destinatário desta primeira tarefa.

Quando a primeira tarefa for concluída, o destinatário terá três opções para rotear o workflow:

**Normal **- o roteamento normal cria uma tarefa atribuída ao grupo Aprovador do projeto para análise e aprovação. A prioridade da tarefa é Normal e o prazo é de cinco dias a partir de quando ela é criada.

**Atrasar** - o roteamento apressado também cria uma tarefa atribuída ao grupo de Aprovadores do projeto. A prioridade da tarefa é Alta e o prazo é de apenas um dia.

**Ignorar** - nesta amostra de fluxo de trabalho, o participante inicial tem a opção de ignorar o grupo de aprovação. (sim, isso pode anular a finalidade de um workflow de &quot;Aprovação&quot;, mas permite ilustrar recursos adicionais de roteamento)

O Grupo de aprovadores pode aprovar o conteúdo ou enviá-lo de volta ao destinatário inicial para retrabalho. No caso de ser reenviada de volta para, o retrabalho de uma nova tarefa é criado e rotulado apropriadamente como &#39;Reenviada de volta para retrabalho&#39;.

A última etapa do fluxo de trabalho usa a etapa de processo Ativar página/ativo OBT e replica a carga.

## Criar o modelo de fluxo de trabalho

1. No menu inicial do AEM, navegue até Ferramentas -> Fluxo de trabalho -> Modelos. Clique em &quot;Criar&quot; no canto superior direito para criar um Modelo de fluxo de trabalho.

   Dê um título ao novo modelo: &quot;Fluxo de trabalho de aprovação de conteúdo&quot; e um nome de URL: &quot;content-approval-workflow&quot;.

   ![Caixa de diálogo de criação do fluxo de trabalho](./assets/develop-aem-projects/workflow-create-dialog.png)

   [Para obter mais informações relacionadas à criação de fluxos de trabalho, leia aqui](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-models).

1. Como prática recomendada, os workflows personalizados devem ser agrupados em sua própria pasta em /etc/workflow/models. No CRXDE Lite, crie uma **&#39;nt:folder&#39;** abaixo de /etc/workflow/models chamada **&quot;aem-guides&quot;**. Adicionar uma subpasta garante que os workflows personalizados não sejam substituídos acidentalmente durante atualizações ou instalações do Service Pack.

   &#42;Observe que é importante nunca colocar a pasta ou os fluxos de trabalho personalizados abaixo das subpastas ootb, como /etc/workflow/models/dam ou /etc/workflow/models/projects, pois a subpasta inteira também pode ser substituída por atualizações ou service packs.

   ![Local do modelo de fluxo de trabalho em 6.3](./assets/develop-aem-projects/custom-workflow-subfolder.png)

   Localização do modelo de fluxo de trabalho no 6.3

   >[!NOTE]
   >
   >Se estiver usando o AEM 6.4+, a localização do Fluxo de trabalho foi alterada. Clique [aqui para obter mais detalhes.](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-best-practices)

   Se estiver usando AEM 6.4+, o modelo de fluxo de trabalho será criado em `/conf/global/settings/workflow/models`. Repita as etapas acima com o diretório /conf e adicione uma subpasta chamada `aem-guides` e mova o `content-approval-workflow` para baixo dela.

   ![Local de definição de fluxo de trabalho moderno](./assets/develop-aem-projects/modern-workflow-definition-location.png)
Localização do modelo de fluxo de trabalho no 6.4+

1. Introduzido no AEM 6.3 é a capacidade de adicionar Estágios de fluxo de trabalho a um determinado fluxo de trabalho. Os estágios serão exibidos para o usuário na Caixa de entrada da guia Informações do fluxo de trabalho. Ele mostrará ao usuário o estágio atual no workflow, bem como os estágios anteriores e posteriores.

   Para configurar os estágios, abra a caixa de diálogo Propriedades da página no Sidekick. A quarta guia é rotulada como &quot;Estágios&quot;. Adicione os seguintes valores para configurar os três estágios desse workflow:

   1. Editar conteúdo
   1. Aprovação
   1. Publicação

   ![configuração de estágios de fluxo de trabalho](./assets/develop-aem-projects/workflow-model-stage-properties.png)

   Configure os Estágios do fluxo de trabalho na caixa de diálogo Propriedades da página.

   ![barra de progresso do fluxo de trabalho](./assets/develop-aem-projects/workflow-info-progress.png)

   A barra de progresso do fluxo de trabalho conforme vista da Caixa de entrada AEM.

   Opcionalmente, é possível carregar uma **Imagem** nas Propriedades da Página, que são usadas como miniatura do Fluxo de Trabalho quando os usuários a selecionam. As dimensões da imagem devem ser de 319x319 pixels. Adicionar uma **Descrição** às Propriedades da página também aparecerá quando um usuário selecionar o fluxo de trabalho.

1. O processo de fluxo de trabalho Criar tarefa do projeto foi projetado para criar uma Tarefa como uma etapa no fluxo de trabalho. Somente após a conclusão da tarefa é que o workflow avançará. Um aspecto importante da etapa Criar tarefa do projeto é que ela pode ler valores de metadados de fluxo de trabalho e usá-los para criar a tarefa dinamicamente.

   Primeiro exclua a Etapa do participante que é criada por padrão. No menu Componentes, do Sidekick, expanda o subtítulo **&quot;Projetos&quot;** e arraste e solte a **&quot;Criar Tarefa de Projeto&quot;** no modelo.

   Clique duas vezes na etapa &quot;Criar tarefa do projeto&quot; para abrir a caixa de diálogo do fluxo de trabalho. Configure as seguintes propriedades:

   Essa guia é comum a todas as etapas do processo de workflow e definiremos o Título e a Descrição (eles não estarão visíveis para o usuário final). A propriedade importante que definiremos é Estágio do Fluxo de Trabalho para **&quot;Editar Conteúdo&quot;** no menu suspenso.

   ```shell
   Common Tab
   -----------------
       Title = "Start Task Creation"
       Description = "This the first task in the Workflow"
       Workflow Stage = "Edit Content"
   ```

   O processo de fluxo de trabalho Criar tarefa do projeto foi projetado para criar uma Tarefa como uma etapa no fluxo de trabalho. A guia Task permite definir todos os valores da tarefa. No nosso caso, queremos que o Destinatário seja dinâmico, portanto, vamos deixá-lo em branco. O restante dos valores de propriedade:

   ```shell
   Task Tab
   -----------------
       Name* = "Edit Content"
       Task Priority = "Medium"
       Description = "Edit the content and finalize for approval. Once finished submit for approval."
       Due In - Days = "2"
   ```

   A guia Roteamento é uma caixa de diálogo opcional que pode especificar as ações disponíveis para o usuário que está concluindo a tarefa. Essas ações são apenas valores de sequência de caracteres e são salvas nos metadados do fluxo de trabalho. Esses valores podem ser lidos por scripts e/ou etapas do processo posteriormente no workflow para &quot;rotear&quot; dinamicamente o workflow. Com base nas metas do workflow, adicionaremos três ações a esta guia:

   ```shell
   Routing Tab
   -----------------
       Actions =
           "Normal Approval"
           "Rush Approval"
           "Bypass Approval"
   ```

   Essa guia permite configurar um Script de tarefa de pré-criação, no qual podemos decidir programaticamente vários valores da Tarefa antes de ela ser criada. Temos a opção de apontar o script para um arquivo externo ou incorporar um script curto diretamente na caixa de diálogo. Em nosso caso, apontaremos o script de tarefa de pré-criação para um arquivo externo. Na Etapa 5, criaremos esse script.

   ```shell
   Advanced Settings Tab
   -----------------
      Pre-Create Task Script = "/apps/aem-guides/projects/scripts/start-task-config.ecma"
   ```

1. Na etapa anterior, referenciamos um script de tarefa de pré-criação. Criaremos esse script agora no qual definiremos o Destinatário da tarefa com base no valor de um valor de metadados de fluxo de trabalho &quot;**destinatário**&quot;. O valor **&quot;destinatário&quot;** é definido quando o fluxo de trabalho é iniciado. Também leremos os metadados do fluxo de trabalho para escolher dinamicamente a prioridade da tarefa lendo o valor &quot;**taskPriority&quot;** dos metadados do fluxo de trabalho, bem como o **&quot;taskDueDate&quot; **para definir dinamicamente quando a primeira tarefa vencer.

   Para fins organizacionais, criamos uma pasta abaixo da pasta do aplicativo para armazenar todos os scripts relacionados ao projeto: **/apps/aem-guides/projects-tasks/projects/scripts**. Crie um arquivo abaixo desta pasta chamado **&quot;start-task-config.ecma&quot;**. &#42;Observe que o caminho para o arquivo start-task-config.ecma corresponde ao caminho definido na guia Configurações Avançadas na Etapa 4.

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

1. Volte para o Fluxo de trabalho de aprovação de conteúdo. Arraste e solte o componente **OU Split** (encontrado no Sidekick abaixo da categoria &#39;Fluxo de trabalho&#39;) abaixo da etapa **Iniciar tarefa**. Na caixa de diálogo comum, selecione o botão de opção para 3 ramificações. A Divisão OR lerá o valor de metadados do fluxo de trabalho **&quot;lastTaskAction&quot;** para determinar a rota do fluxo de trabalho. A propriedade **&quot;lastTaskAction&quot;** está definida como um dos valores da Guia Roteamento configurada na Etapa 4. Para cada uma das guias de Ramificação, preencha a área de texto **Script** com os seguintes valores:

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

   &#42;Observe que estamos fazendo uma correspondência direta de sequência de caracteres para determinar a rota. Portanto, é importante que os valores definidos nos scripts de Ramificação correspondam aos valores de Rota definidos na Etapa 4.

1. Arraste e solte outra etapa &quot;**Criar tarefa do projeto**&quot; no modelo na extremidade esquerda (Ramificação 1) abaixo da divisão OU. Preencha a caixa de diálogo com as seguintes propriedades:

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

   Como esse é o roteiro de Aprovação normal, a prioridade da tarefa é definida como Medium. Além disso, concedemos ao grupo de Aprovadores 5 dias para concluir a Tarefa. O responsável é deixado em branco na guia Tarefa, pois atribuiremos isso dinamicamente na guia Configurações avançadas. Fornecemos ao grupo de Aprovadores duas rotas possíveis ao concluir esta tarefa: **&quot;Aprovar e Publish&quot;** se eles aprovarem o conteúdo e ele puder ser publicado e **&quot;Enviar de volta para revisão&quot;** se houver problemas que o editor original precise corrigir. O aprovador pode deixar comentários que o editor original verá se o workflow foi retornado a ele.

Anteriormente neste tutorial, criamos um Modelo de projeto que incluía uma Função de aprovadores. Cada vez que um novo Projeto é criado a partir deste Modelo, um Grupo específico do projeto é criado para a função Aprovadores. Assim como uma Etapa do participante, uma Tarefa só pode ser atribuída a um usuário ou grupo. Queremos atribuir esta tarefa ao grupo de projeto que corresponde ao Grupo de aprovadores. Todos os fluxos de trabalho iniciados em um Projeto terão metadados que mapeiam as Funções do projeto para o grupo específico do Projeto.

Copie e cole o seguinte código na área de texto **Script** da guia **Configurações Avançadas. Este código lê os metadados do fluxo de trabalho e atribui a tarefa ao grupo de Aprovadores do projeto. Se não conseguir encontrar o valor do grupo de aprovadores, ele voltará a atribuir a tarefa ao grupo Administradores.

```
var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");

task.setCurrentAssignee(projectApproverGrp);
```

1. Arraste e solte outra etapa &quot;**Criar tarefa do projeto**&quot; no modelo na ramificação central (ramificação 2) abaixo da divisão OU. Preencha a caixa de diálogo com as seguintes propriedades:

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

   Como essa é a rota de Aprovação Rápida, a prioridade da tarefa é definida como Alta. Além disso, oferecemos ao grupo de Aprovadores apenas um único dia para concluir a tarefa. O responsável é deixado em branco na guia Tarefa, pois atribuiremos isso dinamicamente na guia Configurações avançadas.

   Podemos reutilizar o mesmo trecho de script da Etapa 7 para preencher a área de texto **Script** na guia **Configurações avançadas. Copie e cole o código abaixo:

   ```
   var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");
   
   task.setCurrentAssignee(projectApproverGrp);
   ```

1. Arraste e solte um componente ** Sem operação ** na ramificação mais à direita (ramificação 3). O componente de Nenhuma operação não executa nenhuma ação e avançará imediatamente, representando o desejo do editor original de ignorar a etapa de aprovação. Tecnicamente, podemos deixar essa Ramificação sem etapas do fluxo de trabalho, mas, como prática recomendada, adicionaremos uma etapa Sem operação. Isso deixa claro para outros desenvolvedores qual é a finalidade da Ramificação 3.

   Clique duas vezes na etapa do fluxo de trabalho e configure o Título e a Descrição:

   ```
   Common Tab
   -----------------
       Title = "Bypass Approval"
       Description = "Placeholder step to indicate that the original editor decided to bypass the approver group."
   ```

   ![modelo de fluxo de trabalho OU divisão](./assets/develop-aem-projects/workflow-stage-after-orsplit.png)

   O modelo de fluxo de trabalho deve ter essa aparência depois que todas as três ramificações na divisão OU forem configuradas.

1. Como o grupo Aprovadores tem a opção de enviar o fluxo de trabalho de volta ao editor original para novas revisões, dependeremos da etapa **Ir para** para ler a última ação realizada e rotear o fluxo de trabalho para o início ou deixá-lo continuar.

   Arraste e solte o componente Etapa Ir para (encontrado no Sidekick em Fluxo de trabalho) abaixo da divisão OR onde ele se une novamente. Clique duas vezes e configure as seguintes propriedades na caixa de diálogo:

   ```
   Common Tab
   ----------------
       Title = "Goto Step"
       Description = "Based on the Approver groups action route the workflow to the beginning or continue and publish the payload."
   
   Process Tab
   ---------------
       The step to go to. = "Start Task Creation"
   ```

   A última parte que configuraremos é o Script como parte da etapa do processo Ir para. O valor do Script pode ser incorporado por meio da caixa de diálogo ou configurado para apontar para um arquivo externo. O Script Goto deve conter uma **função check()** e retornar true se o fluxo de trabalho for para a etapa especificada. Um retorno de resultados falsos no fluxo de trabalho avançando.

   Se o grupo de aprovadores escolher a ação **&quot;Enviar de volta para revisão&quot;** (configurada nas etapas 7 e 8), queremos retornar o fluxo de trabalho para a etapa **&quot;Iniciar criação de tarefa&quot;**.

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

1. Para publicar a carga, usaremos a etapa de processo **Ativar página/ativo**. Essa etapa do processo requer pouca configuração e adicionará a carga do fluxo de trabalho à fila de replicação para ativação. Adicionaremos a etapa abaixo de Ir para e, dessa forma, ela só poderá ser atingida se o grupo Aprovador tiver aprovado o conteúdo para publicação ou o editor original escolher a rota Ignorar aprovação.

   Arraste e solte a etapa do processo **Ativar página/ativo** (localizada no Sidekick sob o fluxo de trabalho WCM) abaixo da etapa Ir para no modelo.

   ![modelo de fluxo de trabalho concluído](assets/develop-aem-projects/workflow-model-final.png)

   Como deve ser o modelo de fluxo de trabalho depois de adicionar as etapas Ir para e Ativar página/ativo.

1. Se o grupo Aprovador enviar o conteúdo de volta para revisão, queremos informar o editor original. Podemos fazer isso alterando dinamicamente as propriedades de criação da Tarefa. Vamos desativar o valor da propriedade lastActionTaken de **&quot;Enviar de volta para revisão&quot;**. Se esse valor estiver presente, modificaremos o título e a descrição para indicar que essa tarefa é o resultado do conteúdo ser enviado de volta para revisão. Também atualizaremos a prioridade para **&quot;Alta&quot;** para que seja o primeiro item no qual o editor trabalha. Por fim, definiremos o prazo da tarefa como um dia a partir de quando o workflow foi enviado de volta para revisão.

   Substitua o script de início `start-task-config.ecma` (criado na Etapa 5) pelo seguinte:

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

## Criar o assistente &quot;start workflow&quot; {#start-workflow-wizard}

Ao iniciar um fluxo de trabalho de um projeto, você deve especificar um assistente para iniciar o fluxo de trabalho. O assistente padrão: `/libs/cq/core/content/projects/workflowwizards/default_workflow` permite que o usuário insira um Título de Fluxo de Trabalho, um comentário inicial e um caminho de carga para o fluxo de trabalho ser executado. Há também vários outros exemplos encontrados em: `/libs/cq/core/content/projects/workflowwizards`.

A criação de um assistente personalizado pode ser muito eficiente, pois você pode coletar informações críticas antes que o fluxo de trabalho seja iniciado. Os dados são armazenados como parte dos metadados do fluxo de trabalho, e os processos de fluxo de trabalho podem ler isso e alterar dinamicamente o comportamento com base nos valores inseridos. Criaremos um assistente personalizado para atribuir dinamicamente a primeira tarefa no fluxo de trabalho com base em um valor de assistente de início.

1. No CRXDE-Lite, criaremos uma subpasta abaixo da pasta `/apps/aem-guides/projects-tasks/projects` chamada &quot;assistentes&quot;. Copie o assistente padrão de: `/libs/cq/core/content/projects/workflowwizards/default_workflow` abaixo da pasta de assistentes recém-criada e renomeie-o para **content-approval-start**. O caminho completo agora deve ser: `/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start`.

   O assistente padrão é um assistente de 2 colunas, com a primeira coluna mostrando o Título, a Descrição e a Miniatura do modelo de fluxo de trabalho selecionado. A segunda coluna inclui campos para o Título do fluxo de trabalho, Comentário inicial e Caminho da carga útil. O assistente é um formulário de interface para toque padrão e usa os [componentes de formulário de interface do usuário do Granite](https://experienceleague.adobe.com/en/docs) padrão para preencher os campos.

   ![assistente de fluxo de trabalho de aprovação de conteúdo](./assets/develop-aem-projects/content-approval-start-wizard.png)

1. Adicionaremos um campo adicional ao assistente usado para definir o destinatário da primeira tarefa no fluxo de trabalho (consulte [Criar o modelo de fluxo de trabalho](#create-workflow-model): Etapa 5).

   Abaixo de `../content-approval-start/jcr:content/items/column2/items`, crie um novo nó do tipo `nt:unstructured` chamado **&quot;assign&quot;**. Usaremos o componente Seletor de usuários de projetos (que é baseado no [Componente Seletor de usuários do Granite](https://experienceleague.adobe.com/en/docs)). Esse campo de formulário facilita a restrição da seleção do usuário e do grupo somente aos que pertencem ao projeto atual.

   Abaixo está a representação XML do nó **assign**:

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

1. Também adicionaremos um campo de seleção de prioridade que determinará a prioridade da primeira tarefa no fluxo de trabalho (consulte [Criar o modelo de fluxo de trabalho](#create-workflow-model): Etapa 5).

   Abaixo de `/content-approval-start/jcr:content/items/column2/items`, crie um novo nó do tipo `nt:unstructured` chamado **prioridade**. Usaremos o [componente Selecionar da interface do usuário do Granite](https://experienceleague.adobe.com/br/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions) para preencher o campo de formulário.

   Abaixo do nó **priority** adicionaremos um nó **items** de **nt:unstructured**. Abaixo do nó **items** adicione mais 3 nós para preencher as opções de seleção de Alto, Medium e Baixo. Cada nó é do tipo **nt:unstructured** e deve ter uma propriedade **text** e **value**. O texto e o valor devem ser iguais:

   1. Alta
   1. Média
   1. Baixa

   Para o nó Medium, adicione uma propriedade booleana adicional denominada &quot;**seleted&quot;** com um valor definido como **true**. Isso garantirá que Medium seja o valor padrão no campo de seleção.

   Abaixo está uma representação XML da estrutura do nó e das propriedades:

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

1. Permitiremos que o iniciador do fluxo de trabalho defina a data de vencimento da tarefa inicial. Usaremos o campo de formulário [DatePicker](https://experienceleague.adobe.com/en/docs) da interface do usuário do Granite para capturar essa entrada. Também adicionaremos um campo oculto com um [TypeHint](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html#typehint) para garantir que a entrada seja armazenada como uma propriedade de tipo Date no JCR.

   Adicione dois nós **nt:unstructured** com as seguintes propriedades representadas abaixo em XML:

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

1. Você pode exibir o código completo da caixa de diálogo de início do assistente [aqui](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/projects-tasks-guide/ui.apps/src/main/content/jcr_root/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start/.content.xml).

## Conectar o fluxo de trabalho e o modelo do projeto {#connecting-workflow-project}

A última coisa que precisamos fazer é garantir que o modelo de fluxo de trabalho esteja disponível para ser lançado de dentro de um dos Projetos. Para fazer isso, precisamos visitar novamente o Modelo de projeto que criamos na Parte 1 desta série.

A configuração do fluxo de trabalho é uma área de um modelo de projeto que especifica os fluxos de trabalho disponíveis a serem usados com esse projeto. A configuração também é responsável por especificar o Assistente para Iniciar Fluxo de Trabalho ao iniciar o fluxo de trabalho (que criamos nas [etapas anteriores)](#start-workflow-wizard). A configuração de fluxo de trabalho de um modelo de projeto está &quot;ativa&quot;, o que significa que a atualização da configuração de fluxo de trabalho afetará novos projetos criados, bem como projetos existentes que usam o modelo.

1. No CRXDE-Lite, navegue até o modelo de projeto de criação criado anteriormente em `/apps/aem-guides/projects-tasks/projects/templates/authoring-project/workflows/models`.

   Abaixo do nó de modelos adicione um novo nó chamado **contentapproval** com um tipo de nó de **nt:unstructured**. Adicione as seguintes propriedades ao nó:

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/etc/workflow/models/aem-guides/content-approval-workflow/jcr:content/model"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

   >[!NOTE]
   >
   >Se estiver usando o AEM 6.4, a localização do Fluxo de trabalho foi alterada. Aponte a propriedade `modelId` para o local do modelo de fluxo de trabalho de tempo de execução em `/var/workflow/models/aem-guides/content-approval-workflow`
   >
   >
   >Clique [aqui para obter mais detalhes sobre a alteração no local do fluxo de trabalho.](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-best-practices)

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/var/workflow/models/aem-guides/content-approval-workflow"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

1. Depois que o fluxo de trabalho de Aprovação de conteúdo for adicionado ao Modelo de projeto, ele deverá estar disponível para ser lançado do Bloco de fluxo de trabalho do projeto. Vá em frente, inicie e brinque com os vários roteamentos que criamos.

## Materiais de suporte

* [Baixar pacote de tutorial concluído](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [Repositório de Código Completo no GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)
* [Documentação de projetos do AEM](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/authoring/projects/projects)
