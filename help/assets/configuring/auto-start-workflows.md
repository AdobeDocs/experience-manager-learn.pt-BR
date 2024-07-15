---
title: Workflows de início automático
description: Os workflows de início automático estendem o processamento de ativos, chamando automaticamente o fluxo de trabalho personalizado após o upload ou o reprocessamento.
feature: Asset Compute Microservices, Workflow
version: Cloud Service
jira: KT-4994
thumbnail: 37323.jpg
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-05-14T00:00:00Z
doc-type: Feature Video
exl-id: 5e423f2c-90d2-474f-8bdc-fa15ae976f18
duration: 385
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# Workflows de início automático

Os fluxos de trabalho de início automático estendem o processamento de ativos no AEM as a Cloud Service, chamando automaticamente o fluxo de trabalho personalizado após o upload ou o reprocessamento depois que o processamento de ativos é concluído.

>[!VIDEO](https://video.tv.adobe.com/v/37323?quality=12&learn=on)

>[!NOTE]
>
>Use Workflows de início automático para personalizar ativos após o processamento em vez de usar Iniciadores de fluxo de trabalho. Os fluxos de trabalho de início automático são _somente_ invocados quando um ativo conclui o processamento em vez de iniciadores que podem ser invocados várias vezes durante o processamento de ativos.

## Personalização do fluxo de trabalho de processamento Post

Para personalizar o fluxo de trabalho de Processamento Post, copie o [modelo de fluxo de trabalho](../../foundation/workflow/use-the-workflow-editor.md) padrão de Processamento Post na Nuvem da Assets.

1. Comece na tela Modelos de Fluxo de Trabalho navegando até _Ferramentas_ > _Fluxo de Trabalho_ > _Modelos_
2. Localize e selecione o _modelo de fluxo de trabalho Assets Cloud Post-Processing_<br/>
   ![Selecione o modelo de Fluxo de Trabalho de Processamento Post da Assets Cloud](assets/auto-start-workflow-select-workflow.png)
3. Selecione o botão _Copiar_ para criar seu fluxo de trabalho personalizado
4. Selecione seu modelo de fluxo de trabalho agora (que será chamado de _Assets Cloud Post-Processing1_) e clique no botão _Editar_ para editar o fluxo de trabalho
5. Nas Propriedades do Fluxo de Trabalho, dê um nome significativo<br/> ao seu fluxo de trabalho personalizado de processamento de Post
   ![Alterando o Nome](assets/auto-start-workflow-change-name.png)
6. Adicione as etapas para atender aos requisitos comerciais, nesse caso, adicionando uma tarefa quando o processamento dos ativos estiver concluído. Certifique-se de que a última etapa do fluxo de trabalho seja sempre a _Etapa do fluxo de trabalho concluído_<br/>
   ![Adicionar etapas do fluxo de trabalho](assets/auto-start-workflow-customize-steps.png)

   >[!NOTE]
   >
   >Os fluxos de trabalho de início automático são executados com cada carregamento ou reprocessamento de ativo, portanto, considere cuidadosamente a implicação de dimensionamento das etapas do fluxo de trabalho, especialmente para operações em massa, como [Importações em massa](../../cloud-service/migration/bulk-import.md) ou migrações.

7. Selecione o botão _Sincronizar_ para salvar suas alterações e sincronizar o modelo de fluxo de trabalho

## Utilização de um fluxo de trabalho de processamento Post personalizado

O Post-Processing personalizado é configurado em pastas. Para configurar um Fluxo de trabalho de processamento Post personalizado em uma pasta:

1. Selecione a pasta para a qual deseja configurar o workflow e edite as propriedades da pasta
2. Mudar para a guia _Processamento de ativos_
3. Selecione o fluxo de trabalho de Processamento Post Personalizado na caixa de seleção _Fluxo de trabalho de início automático_<br/>
   ![Definir o Fluxo de Trabalho de Processamento Post](assets/auto-start-workflow-set-workflow.png)
4. Salve as alterações

Agora, o fluxo de trabalho de processamento Post personalizado será executado para todos os ativos carregados ou reprocessados nessa pasta.
