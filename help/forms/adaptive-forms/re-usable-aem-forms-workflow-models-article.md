---
title: Crie Modelos De Fluxo De Trabalho Do AEM Forms Reutilizáveis.
seo-title: Crie Modelos De Fluxo De Trabalho Do AEM Forms Reutilizáveis.
description: modelos de fluxo de trabalho independentes do Adaptive Forms.
seo-description: Modelos de fluxo de trabalho independentes do Adaptive Forms.
feature: workflow
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.5
uuid: 3a082743-3e56-42f4-a44b-24fa34165926
discoiquuid: 9f18c314-39d1-4c82-b1bc-d905ea472451
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 0%

---


# Criar modelos de fluxo de trabalho do AEM Forms reutilizáveis{#create-re-usable-aem-forms-workflow-models}

A partir do AEM Forms 6.5, agora podemos criar modelos de fluxo de trabalho que não estão vinculados a um formulário adaptativo específico. Com esse recurso, agora você pode criar um modelo de fluxo de trabalho que pode ser chamado em diferentes envios de formulário adaptáveis. Com esse recurso, você pode ter um fluxo de trabalho genérico para lidar com todos os envios de formulário adaptáveis para revisão e aprovação.

Para projetar esse fluxo de trabalho, execute as seguintes etapas

1. Login no AEM
1. Aponte seu navegador para o modelo [de fluxo de trabalho](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Clique em Criar | Criar modelo para adicionar modelo de fluxo de trabalho
1. Forneça o Nome e o Título apropriados ao modelo de fluxo de trabalho e clique em Concluído
1. Abrir o modelo recém-criado no modo de edição
1. Arrastar e soltar Atribuir componente de Tarefa ao modelo de fluxo de trabalho
1. Abra as propriedades de configuração do componente Atribuir Tarefa
1. Guia para a guia Forms e Documentos
1. Selecione o tipo - Formulário adaptável ou Formulário adaptável somente leitura.

Há três maneiras de especificar o caminho do formulário

1. Disponível em um caminho absoluto - isso significa que o fluxo de trabalho será estreitamente associado ao formulário adaptável. Não é isso que queremos aqui
1. **Submetido ao fluxo de trabalho** - Isso significa que, quando o formulário adaptativo for enviado, o motor de workflow extrairá o nome do formulário dos dados enviados. Esta é a opção que precisa ser selecionada
1. Disponível em um caminho em uma variável - Isso significa que o formulário adaptativo será selecionado da variável de fluxo de trabalho. A captura de tela a seguir mostra a opção correta que você precisa escolher para desacoplar o fluxo de trabalho do formulário adaptável

![modelo de fluxo de trabalho](assets/workflomodel.PNG)