---
title: Captura de comentários do fluxo de trabalho no Adaptive Forms Workflow
seo-title: Captura de comentários do fluxo de trabalho no Adaptive Forms Workflow
description: Captura de comentários do fluxo de trabalho no AEM Fluxo de trabalho
seo-description: Captura de comentários do fluxo de trabalho no AEM Fluxo de trabalho
uuid: df41fc6f-9abf-47b4-a014-b3b9fb58b6f7
feature: workflow
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4
discoiquuid: d5e40650-3b1f-4875-91b2-e22d932b5e7c
translation-type: tm+mt
source-git-commit: f07680e73316efb859a675f4b2212d8c3e03f6a0
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 0%

---


# Captura de comentários do fluxo de trabalho no Adaptive Forms Workflow{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[Aplica-se somente ao AEM Forms 6.4. No AEM Forms 6.5, use o recurso de variáveis para obter esse caso de uso]

Uma solicitação comum é a capacidade de incluir os comentários inseridos pelo revisor de tarefas em um email. No AEM Forms 6.4, não há nenhum mecanismo pronto para capturar os comentários inseridos pelo usuário e incluí-los no email.

Para atender a esse requisito, é fornecido um pacote OSGi de amostra que pode ser usado para capturar comentários e armazenar esses comentários como propriedade de metadados do fluxo de trabalho.

A captura de tela a seguir mostra como usar a etapa do processo em [AEM fluxo de trabalho](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) para capturar comentários e armazená-los como propriedade de metadados. O &quot;Comentários do fluxo de trabalho de captura&quot; é o nome da classe java que precisa ser usada na etapa do processo. É necessário transmitir o nome da propriedade de metadados que manterá os comentários. Na captura de tela abaixo, managerComments é a propriedade de metadados que armazenará os comentários.

![workflowcomments1](assets/workflowcomments1.gif)

Para testar esse recurso em seu sistema, siga as seguintes etapas:
* [Verifique se a etapa do processo no fluxo de trabalho está configurada para usar os Comentários do fluxo de trabalho de captura](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Implantar o pacote Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Implante o conjunto](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)SetValue. Este pacote contém o código de amostra para capturar os comentários e armazená-lo como uma propriedade de metadados

* [Baixe e descompacte os ativos relacionados a este artigo no seu sistema](assets/capturecomments.zip) de arquivos Os ativos contêm modelo de fluxo de trabalho e amostra de Formulário adaptável.

* Importe os 2 arquivos zip para AEM usando o gerenciador de pacotes

* [Pré-visualização do formulário navegando até esse URL](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Preencha os campos do formulário e envie o formulário

* [Verifique sua caixa de entrada de AEM](http://localhost:4502/aem/inbox)

* Abra a tarefa da caixa de entrada e envie o formulário. Digite alguns comentários quando solicitado.

Os comentários serão armazenados na propriedade de metadados chamada managerComments na caixa. Para verificar se os comentários fazem logon no crx como administrador. As instâncias de fluxo de trabalho são armazenadas no seguinte caminho

/var/workflow/instance/server0

Selecione a instância de fluxo de trabalho apropriada e verifique se a propriedade managerComments está no nó de metadados.

