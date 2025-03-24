---
title: Criar componente de fluxo de trabalho para salvar anexos de formulário no sistema de arquivos
description: Gravação de anexos de formulário adaptável no sistema de arquivos usando o componente personalizado do fluxo de trabalho
feature: Workflow
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-11-28T00:00:00Z
exl-id: acc701ec-b57d-4c20-8f97-a5a69bb180cd
duration: 70
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 0%

---

# Componente de fluxo de trabalho personalizado

Este tutorial destina-se aos clientes do AEM Forms que precisam criar um componente de fluxo de trabalho personalizado. O componente de fluxo de trabalho será configurado para executar o código escrito na etapa anterior. O componente de fluxo de trabalho pode especificar argumentos de processo para o código. Neste artigo, exploraremos o componente de fluxo de trabalho associado ao código.


[Baixar o componente de fluxo de trabalho personalizado](assets/saveFiles.zip)
Importar o componente de fluxo de trabalho [usando o gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)

O componente de fluxo de trabalho personalizado está localizado em /apps/AEMFormsDemoListings/workflowcomponent/SaveFiles

Selecione o nó SaveFiles e examine suas propriedades

**componentGroup** - O valor dessa propriedade determina a categoria do componente do fluxo de trabalho.

**jcr:Title** - Este é o título do componente do fluxo de trabalho.

**sling:resourceSuperType** O valor dessa propriedade determinará a herança desse componente. Nesse caso, estamos herdando do componente do processo


![propriedades-componente](assets/component-properties1.png)

## cq:dialog

As caixas de diálogo são usadas para permitir que o autor interaja com o componente. O cq:dialog está no nó SaveFiles
![cq-dialog](assets/cq-dialog.png)

Os nós no nó itens representam as guias do componente pelas quais os autores interagem com o componente. As guias comum e processo estão ocultas. As guias Comum e Argumentos ficam visíveis.

Os argumentos do processo estão no nó processargs

![argumentos-processo](assets/process-arguments.png)

O autor especifica os argumentos conforme mostrado na captura de tela abaixo
![componente do fluxo de trabalho](assets/custom-workflow-component.png)

Os valores são armazenados como propriedades do nó de metadados. Por exemplo, o valor **c:\formsattachments** será armazenado na propriedade saveToLocation do nó de metadados
![local para salvar](assets/save-to-location.png)

## cq:editConfig

O cq:EditConfig é simplesmente um nó com o tipo primário cq:EditConfig e o nome cq:editConfig na raiz do componente
O comportamento de edição de um componente é configurado adicionando um nó cq:editConfig do tipo cq:EditConfig abaixo do nó do componente (do tipo cq:Component)

![edit-config](assets/cq-edit-config.png)

cq:formParameters (tipo de nó nt:unstructured): define parâmetros adicionais que são adicionados ao formulário da caixa de diálogo.


Observe as propriedades do nó cq:formParameters
![de-parâmetros-propriedades](assets/form-parameters-properties.png)

O valor da propriedade PROCESS indica o código java que será associado ao componente do fluxo de trabalho.
