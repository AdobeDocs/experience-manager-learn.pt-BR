---
title: Captura de comentários do fluxo de trabalho no Adaptive Forms Workflow
description: Captura de comentários do workflow no AEM Workflow
feature: Workflow
version: Experience Manager 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
last-substantial-update: 2020-10-10T00:00:00Z
duration: 73
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---

# Captura de comentários do fluxo de trabalho no Adaptive Forms Workflow{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[Aplicável somente ao AEM Forms 6.4. No AEM Forms 6.5, use o recurso de variáveis para obter esse caso de uso]

Uma solicitação comum é a capacidade de incluir os comentários inseridos pelo revisor da tarefa em um email. No AEM Forms 6.4, não há um mecanismo pronto para capturar os comentários inseridos pelo usuário e incluí-los no email.

Para atender a esse requisito, é fornecido um pacote OSGi de amostra que pode ser usado para capturar comentários e armazenar esses comentários como uma propriedade de metadados do fluxo de trabalho.

A captura de tela a seguir mostra como usar a etapa do processo no [Fluxo de Trabalho do AEM](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) para capturar comentários e armazená-los como propriedade de metadados. O &quot;Comentários do fluxo de trabalho de captura&quot; é o nome da classe java que precisa ser usada na etapa do processo. É necessário passar o nome da propriedade de metadados que conterá os comentários. Na captura de tela abaixo, managerComments é a propriedade de metadados que armazenará os comentários.

![comentários do fluxo de trabalho1](assets/workflowcomments1.gif)

Para testar esse recurso em seu sistema, siga as seguintes etapas:
* [Verifique se a etapa do processo no fluxo de trabalho está configurada para usar Capture Workflow Comments](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Implantar o pacote Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Implante o conjunto SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este pacote contém o código de amostra para capturar os comentários e armazená-los como uma propriedade de metadados

* [Baixe e descompacte os ativos relacionados a este artigo no sistema de arquivos](assets/capturecomments.zip). Os ativos contêm um modelo de fluxo de trabalho e um formulário adaptável de amostra.

* Importe os 2 arquivos zip para o AEM usando o gerenciador de pacotes

* [Visualizar o formulário navegando até esta URL](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* Preencha os campos do formulário e envie o formulário

* [Verifique sua caixa de entrada do AEM](http://localhost:4502/aem/inbox)

* Abra a tarefa da caixa de entrada e envie o formulário. Insira alguns comentários quando solicitado.

Os comentários são armazenados na propriedade de metadados chamada `managerComments` no repositório do AEM. Para verificar os comentários, faça logon no crx como administrador. As instâncias de fluxo de trabalho são armazenadas no seguinte caminho:

`/var/workflow/instances/server0`

Selecione a instância de fluxo de trabalho apropriada e verifique se há comentários do gerenciador de propriedades no nó de metadados.
