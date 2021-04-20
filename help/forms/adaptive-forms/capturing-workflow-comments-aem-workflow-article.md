---
title: Capturando comentários do fluxo de trabalho no fluxo de trabalho de formulários adaptáveis
seo-title: Capturando comentários do fluxo de trabalho no fluxo de trabalho de formulários adaptáveis
description: Capturando comentários do fluxo de trabalho no fluxo de trabalho do AEM
seo-description: Capturando comentários do fluxo de trabalho no fluxo de trabalho do AEM
uuid: df41fc6f-9abf-47b4-a014-b3b9fb58b6f7
feature: Workflow
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4
discoiquuid: d5e40650-3b1f-4875-91b2-e22d932b5e7c
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 1%

---


# Capturando comentários do fluxo de trabalho no fluxo de trabalho de formulários adaptáveis{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[Aplica-se somente ao AEM Forms 6.4. No AEM Forms 6.5, use o recurso de variáveis para obter esse caso de uso]

Uma solicitação comum é a capacidade de incluir os comentários inseridos pelo revisor da tarefa em um email. No AEM Forms 6.4, não há nenhum mecanismo pronto para uso para capturar os comentários inseridos pelo usuário e incluir esses comentários no email.

Para atender a esse requisito, é fornecido um pacote OSGi de amostra que pode ser usado para capturar comentários e armazenar esses comentários como propriedade de metadados de workflow.

A captura de tela a seguir mostra como usar a etapa do processo em [AEM Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) para capturar comentários e armazená-los como propriedade de metadados. O &quot;Capture Workflow Comments&quot; é o nome da classe java que precisa ser usada na etapa do processo. Você precisa transmitir o nome da propriedade de metadados que manterá os comentários. Na captura de tela abaixo, managerComments é a propriedade de metadados que armazenará os comentários.

![workflowcomments1](assets/workflowcomments1.gif)

Para testar esse recurso em seu sistema, siga as seguintes etapas:
* [Verifique se a etapa do processo no fluxo de trabalho está configurada para usar os comentários do fluxo de trabalho de captura](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Implantar o pacote Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Implante o pacote](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) SetValue. Este pacote contém o código de amostra para capturar os comentários e armazená-lo como uma propriedade de metadados

* [Baixe e descompacte os ativos relacionados a este artigo no seu ](assets/capturecomments.zip) sistema de arquivos. Os ativos contêm o modelo de fluxo de trabalho e o formulário adaptável de amostra.

* Importe os 2 arquivos zip para o AEM usando o gerenciador de pacotes

* [Visualize o formulário navegando até este URL](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Preencha os campos de formulário e envie o formulário

* [Marque a caixa de entrada do AEM](http://localhost:4502/aem/inbox)

* Abra a tarefa na caixa de entrada e envie o formulário. Insira alguns comentários quando solicitado.

Os comentários serão armazenados na propriedade de metadados chamada managerComments no crx. Para verificar os comentários, faça logon no crx como administrador. As instâncias de fluxo de trabalho são armazenadas no seguinte caminho

/var/workflow/instances/server0

Selecione a instância de fluxo de trabalho apropriada e verifique a propriedade managerComments no nó de metadados.

