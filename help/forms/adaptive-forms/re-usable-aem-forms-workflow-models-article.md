---
title: Modelos de fluxo de trabalho do AEM Forms reutilizáveis
description: Saiba como criar modelos de fluxo de trabalho independentes do Adaptive Forms.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 3354a58b-d58e-4ddb-8f90-648554a64db8
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 0%

---

# Criar modelos de fluxo de trabalho do AEM Forms reutilizáveis{#create-re-usable-aem-forms-workflow-models}

A partir da versão 6.5 do AEM Forms, agora podemos criar modelos de fluxo de trabalho que não estão vinculados a um formulário adaptável específico. Com esse recurso, agora é possível criar um modelo de fluxo de trabalho que pode ser chamado em diferentes envios de formulários adaptáveis. Com esse recurso, você pode ter um fluxo de trabalho genérico para lidar com todos os envios de formulários adaptáveis para revisão e aprovação.

Para projetar esse workflow, execute as seguintes etapas

1. Faça logon no AEM
1. Aponte seu navegador para [modelo de fluxo de trabalho](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Clique em __Criar > Criar modelo__ para adicionar um modelo de fluxo de trabalho
1. Forneça o Nome e o Título apropriados ao modelo de fluxo de trabalho e clique em Concluído
1. Abra o modelo recém-criado no modo de edição
1. Arraste e solte o componente Atribuir tarefa no modelo de fluxo de trabalho
1. Abra as propriedades de configuração do componente Atribuir tarefa
1. Guia para a guia Forms e Documentos
1. Selecione Tipo - Formulário adaptável ou Formulário adaptável somente leitura.

Há três maneiras de especificar o caminho do formulário

1. Disponível em um caminho absoluto - Isso significa que o fluxo de trabalho está totalmente acoplado ao formulário adaptável. Não é isso que queremos aqui
1. **Enviado para o workflow** - Isso significa que, quando o formulário adaptável é enviado, o motor de fluxo de trabalho extrai o nome do formulário dos dados enviados. Essa é a opção que precisa ser selecionada
1. Disponível em um caminho em uma variável - Isso significa que o formulário adaptável é extraído da variável de fluxo de trabalho A captura de tela a seguir mostra a opção correta que você precisa escolher para o fluxo de trabalho de desacoplamento do formulário adaptável

![Modelos de fluxo de trabalho do AEM Forms reutilizáveis](assets/workflomodel.PNG)
