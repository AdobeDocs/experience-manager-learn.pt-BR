---
title: Workflows de início automático
description: Os workflows de início automático estendem o processamento de ativos, chamando automaticamente o fluxo de trabalho personalizado após o upload ou o reprocessamento.
feature: Asset Compute Microservices, Workflow
version: Cloud Service
kt: 4994
thumbnail: 37323.jpg
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-05-14T00:00:00Z
exl-id: 5e423f2c-90d2-474f-8bdc-fa15ae976f18
source-git-commit: 929fd045b81652463034b54c557de04df3d4e64a
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# Workflows de início automático

Os fluxos de trabalho de início automático estendem o processamento de ativos no AEM as a Cloud Service, chamando automaticamente o fluxo de trabalho personalizado após o upload ou o reprocessamento depois que o processamento de ativos é concluído.

>[!VIDEO](https://video.tv.adobe.com/v/37323?quality=12&learn=on)

>[!NOTE]
>
>Use Workflows de início automático para personalizar ativos após o processamento em vez de usar Iniciadores de fluxo de trabalho. Os workflows de início automático são _somente_ chamado quando um ativo conclui o processamento em vez de iniciadores, que podem ser chamados várias vezes durante o processamento de ativos.

## Personalização do fluxo de trabalho de pós-processamento

Para personalizar o fluxo de trabalho de pós-processamento, copie o pós-processamento padrão da Assets Cloud [modelo de fluxo de trabalho](../../foundation/workflow/use-the-workflow-editor.md).

1. Comece na tela Modelos de fluxo de trabalho navegando até _Ferramentas_ > _Fluxo de trabalho_ > _Modelos_
2. Localize e selecione o _Pós-processamento da nuvem de ativos_ modelo de fluxo de trabalho<br/>
   ![Selecionar o modelo de fluxo de trabalho de pós-processamento da nuvem de ativos](assets/auto-start-workflow-select-workflow.png)
3. Selecione o _Copiar_ botão para criar seu fluxo de trabalho personalizado
4. Selecione seu modelo de fluxo de trabalho now (que será chamado _Pós-processamento da nuvem de ativos1_) e clique no link _Editar_ botão para editar o fluxo de trabalho
5. Nas Propriedades do fluxo de trabalho, dê um nome significativo ao fluxo de trabalho de pós-processamento personalizado<br/>
   ![Alterar o nome](assets/auto-start-workflow-change-name.png)
6. Adicione as etapas para atender aos requisitos comerciais, nesse caso, adicionando uma tarefa quando o processamento dos ativos estiver concluído. Certifique-se de que a última etapa do fluxo de trabalho seja sempre a _Fluxo de trabalho concluído_ etapa<br/>
   ![Adicionar etapas do fluxo de trabalho](assets/auto-start-workflow-customize-steps.png)

   >[!NOTE]
   >
   >Os workflows de início automático são executados com cada upload ou reprocessamento de ativo, considere cuidadosamente a implicação de dimensionamento das etapas do fluxo de trabalho, especialmente para operações em massa, como [Importações em massa](../../cloud-service/migration/bulk-import.md) ou migrações.

7. Selecione o _Sincronizar_ botão para salvar suas alterações e sincronizar o modelo de fluxo de trabalho

## Utilização de um fluxo de trabalho de pós-processamento personalizado

O pós-processamento personalizado é configurado nas pastas. Para configurar um Fluxo de trabalho de pós-processamento personalizado em uma pasta:

1. Selecione a pasta para a qual deseja configurar o workflow e edite as propriedades da pasta
2. Alterne para a _Processamento de ativos_ guia
3. Selecione o fluxo de trabalho de pós-processamento personalizado na _Fluxo de trabalho de início automático_ caixa seleção<br/>
   ![Definir o fluxo de trabalho de pós-processamento](assets/auto-start-workflow-set-workflow.png)
4. Salve as alterações

Agora, o fluxo de trabalho de pós-processamento personalizado será executado para todos os ativos carregados ou reprocessados nessa pasta.
