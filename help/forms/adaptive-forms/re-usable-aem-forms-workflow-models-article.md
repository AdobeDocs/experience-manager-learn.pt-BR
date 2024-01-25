---
title: Modelos de fluxo de trabalho reutilizáveis do AEM Forms
description: Saiba como criar modelos de fluxo de trabalho independentes do Adaptive Forms.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 3354a58b-d58e-4ddb-8f90-648554a64db8
last-substantial-update: 2020-06-09T00:00:00Z
duration: 63
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# Criar Modelos De Fluxo De Trabalho Do AEM Forms Reutilizáveis{#create-re-usable-aem-forms-workflow-models}

A partir da versão AEM Forms 6.5, podemos criar modelos de fluxo de trabalho que não estão vinculados a um Formulário adaptável específico. Com esse recurso, agora é possível criar um modelo de fluxo de trabalho que pode ser chamado em diferentes envios de formulários adaptáveis. Com esse recurso, você pode ter um fluxo de trabalho genérico para lidar com todos os envios de formulários adaptáveis para revisão e aprovação.

Para criar esse fluxo de trabalho, execute as seguintes etapas

1. Fazer logon no AEM
1. Aponte seu navegador para [modelo de fluxo de trabalho](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Clique em __Criar > Criar modelo__ para adicionar um modelo de fluxo de trabalho
1. Forneça o Nome e o Título apropriados para o modelo de fluxo de trabalho e clique em Concluído
1. Abrir o modelo recém-criado no modo de edição
1. Arraste e solte o componente Atribuir tarefa no seu modelo de fluxo de trabalho
1. Abra as propriedades de configuração do componente Atribuir tarefa.
1. Acesse a guia Forms e Documentos
1. Selecione o Tipo - Formulário adaptável ou o Formulário adaptável somente leitura.

Há três maneiras de especificar o caminho do formulário

1. Disponível em um caminho absoluto - Isso significa que o fluxo de trabalho é totalmente combinado com o formulário adaptável. Não é isso que queremos aqui
1. **Enviado ao fluxo de trabalho** - Isso significa que quando o formulário adaptável é enviado, o mecanismo de fluxo de trabalho extrai o nome do formulário dos dados enviados. Esta é a opção que precisa ser selecionada
1. Disponível em um caminho em uma variável - Isso significa que o formulário adaptável é retirado da variável do fluxo de trabalho. A captura de tela a seguir mostra a opção correta que você precisa escolher para desvincular o fluxo de trabalho do formulário adaptável

![Modelos de fluxo de trabalho reutilizáveis do AEM Forms](assets/workflomodel.PNG)
