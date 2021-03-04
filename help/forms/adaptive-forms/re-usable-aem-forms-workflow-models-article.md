---
title: Crie modelos de fluxo de trabalho de AEM Forms reutilizáveis.
seo-title: Crie modelos de fluxo de trabalho de AEM Forms reutilizáveis.
description: modelos de fluxo de trabalho independentes dos formulários adaptativos.
seo-description: Modelos de fluxo de trabalho independentes dos Formulários adaptativos.
feature: Fluxo de trabalho
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.5
uuid: 3a082743-3e56-42f4-a44b-24fa34165926
discoiquuid: 9f18c314-39d1-4c82-b1bc-d905ea472451
topic: Desenvolvimento
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 1%

---


# Criar modelos de fluxo de trabalho de AEM Forms reutilizáveis{#create-re-usable-aem-forms-workflow-models}

A partir da versão AEM Forms 6.5, agora podemos criar modelos de fluxo de trabalho que não estão vinculados a um formulário adaptável específico. Com esse recurso, agora é possível criar um modelo de fluxo de trabalho que pode ser chamado em diferentes envios de formulários adaptáveis. Com esse recurso, você pode ter um fluxo de trabalho genérico para lidar com todos os envios de formulários adaptáveis para revisão e aprovação.

Para projetar esse workflow, execute as seguintes etapas

1. Faça logon no AEM
1. Aponte seu navegador para [modelo de fluxo de trabalho](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Clique em Criar | Criar modelo para adicionar modelo de fluxo de trabalho
1. Forneça o Nome e o Título apropriados ao modelo de fluxo de trabalho e clique em Concluído
1. Abra o modelo recém-criado no modo de edição
1. Arraste e solte o componente Atribuir tarefa no modelo de fluxo de trabalho
1. Abra as propriedades de configuração do componente Atribuir tarefa
1. Guia para a guia Formulários e Documentos
1. Selecione Tipo - Formulário adaptável ou Formulário adaptável somente leitura.

Existem 3 maneiras pelas quais o caminho do formulário pode ser especificado

1. Disponível em um caminho absoluto - Isso significa que o fluxo de trabalho será totalmente acoplado ao formulário adaptável. Não é isso que queremos aqui
1. **Enviado para o fluxo de trabalho**  - Isso significa que, quando o formulário adaptável for enviado, o mecanismo de fluxo de trabalho extrairá o nome do formulário dos dados enviados. Essa é a opção que precisa ser selecionada
1. Disponível em um caminho em uma variável - Isso significa que o formulário adaptável será extraído da variável de fluxo de trabalho
A captura de tela a seguir mostra a opção correta que você precisa escolher para o fluxo de trabalho de desacoplamento do formulário adaptável

![workflowmodel](assets/workflomodel.PNG)