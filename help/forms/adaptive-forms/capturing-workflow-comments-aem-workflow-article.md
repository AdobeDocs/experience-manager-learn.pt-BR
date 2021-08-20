---
title: Captura de comentários do fluxo de trabalho no Adaptive Forms Workflow
description: Capturando comentários do fluxo de trabalho AEM fluxo de trabalho
feature: Fluxo de trabalho
version: 6.3,6.4
topic: Desenvolvimento
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---


# Captura de comentários do fluxo de trabalho no Adaptive Forms Workflow{#capturing-workflow-comments-in-adaptive-forms-workflow}

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

* Importe os 2 arquivos zip no AEM usando o gerenciador de pacotes

* [Visualize o formulário navegando até este URL](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Preencha os campos de formulário e envie o formulário

* [Marque a caixa de entrada AEM](http://localhost:4502/aem/inbox)

* Abra a tarefa na caixa de entrada e envie o formulário. Insira alguns comentários quando solicitado.

Os comentários serão armazenados na propriedade de metadados chamada managerComments no crx. Para verificar os comentários, faça logon no crx como administrador. As instâncias de fluxo de trabalho são armazenadas no seguinte caminho

/var/workflow/instances/server0

Selecione a instância de fluxo de trabalho apropriada e verifique a propriedade managerComments no nó de metadados.

