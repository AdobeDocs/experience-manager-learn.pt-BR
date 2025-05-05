---
title: Workflows de início automático
description: Os workflows de início automático estendem o processamento de ativos, chamando automaticamente o fluxo de trabalho personalizado após o upload ou o reprocessamento.
feature: Asset Compute Microservices, Workflow
version: Experience Manager as a Cloud Service
jira: KT-4994
thumbnail: 37323.jpg
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-05-14T00:00:00Z
doc-type: Feature Video
exl-id: 5e423f2c-90d2-474f-8bdc-fa15ae976f18
duration: 385
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# Workflows de início automático

Os fluxos de trabalho de início automático estendem o processamento de ativos no AEM as a Cloud Service, chamando automaticamente o fluxo de trabalho personalizado após o upload ou o reprocessamento depois que o processamento de ativos é concluído.

>[!VIDEO](https://video.tv.adobe.com/v/326765?quality=12&learn=on&captions=por_br)

>[!NOTE]
>
>Use Workflows de início automático para personalizar ativos após o processamento em vez de usar Iniciadores de fluxo de trabalho. Os fluxos de trabalho de início automático são _somente_ invocados quando um ativo conclui o processamento em vez de iniciadores que podem ser invocados várias vezes durante o processamento de ativos.

## Personalização do fluxo de trabalho de pós-processamento

Para personalizar o fluxo de trabalho de pós-processamento, copie o [modelo de fluxo de trabalho](../../foundation/workflow/use-the-workflow-editor.md) padrão de pós-processamento da Assets Cloud.

1. Comece na tela Modelos de Fluxo de Trabalho navegando até _Ferramentas_ > _Fluxo de Trabalho_ > _Modelos_
2. Localize e selecione o _modelo de fluxo de trabalho de Pós-processamento_ da Assets Cloud<br/>
   ![Selecione o modelo de fluxo de trabalho de pós-processamento da nuvem do Assets](assets/auto-start-workflow-select-workflow.png)
3. Selecione o botão _Copiar_ para criar seu fluxo de trabalho personalizado
4. Selecione seu modelo de fluxo de trabalho agora (que será chamado de _Pós-processamento da Assets Cloud1_) e clique no botão _Editar_ para editar o fluxo de trabalho
5. Nas Propriedades do fluxo de trabalho, dê um nome significativo<br/> ao fluxo de trabalho de pós-processamento personalizado
   ![Alterando o Nome](assets/auto-start-workflow-change-name.png)
6. Adicione as etapas para atender aos requisitos comerciais, nesse caso, adicionando uma tarefa quando o processamento dos ativos estiver concluído. Certifique-se de que a última etapa do fluxo de trabalho seja sempre a _Etapa do fluxo de trabalho concluído_<br/>
   ![Adicionar etapas do fluxo de trabalho](assets/auto-start-workflow-customize-steps.png)

   >[!NOTE]
   >
   >Os fluxos de trabalho de início automático são executados com cada carregamento ou reprocessamento de ativo, portanto, considere cuidadosamente a implicação de dimensionamento das etapas do fluxo de trabalho, especialmente para operações em massa, como [Importações em massa](../../cloud-service/migration/bulk-import.md) ou migrações.

7. Selecione o botão _Sincronizar_ para salvar suas alterações e sincronizar o modelo de fluxo de trabalho

## Utilização de um fluxo de trabalho de pós-processamento personalizado

O pós-processamento personalizado é configurado nas pastas. Para configurar um Fluxo de trabalho de pós-processamento personalizado em uma pasta:

1. Selecione a pasta para a qual deseja configurar o workflow e edite as propriedades da pasta
2. Mudar para a guia _Processamento de ativos_
3. Selecione o fluxo de trabalho Pós-processamento personalizado na caixa de seleção _Fluxo de trabalho de início automático_<br/>
   ![Definir o Fluxo de Trabalho de Pós-Processamento](assets/auto-start-workflow-set-workflow.png)
4. Salve as alterações

Agora, o fluxo de trabalho de pós-processamento personalizado será executado para todos os ativos carregados ou reprocessados nessa pasta.
