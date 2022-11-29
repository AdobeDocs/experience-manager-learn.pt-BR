---
title: Criar componente de fluxo de trabalho para salvar anexos de formulário no sistema de arquivos
description: Gravação de anexos de formulário adaptável no sistema de arquivos usando o componente de fluxo de trabalho personalizado
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-11-28T00:00:00Z
source-git-commit: 09b00a7edf2f4c90c6cb2178161c6d7e0c9432e8
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 1%

---

# Componente de fluxo de trabalho personalizado

Este tutorial é destinado aos clientes do AEM Forms que precisam criar um componente de fluxo de trabalho personalizado. O componente de workflow será configurado para executar o código gravado na etapa anterior. O componente de workflow tem a capacidade de especificar argumentos de processo para o código. Neste artigo, exploraremos componentes de fluxo de trabalho associados ao código.


[Baixar o componente de fluxo de trabalho personalizado](assets/saveFiles.zip)
Importar o componente de fluxo de trabalho [uso do gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)

O componente de fluxo de trabalho personalizado está localizado em /apps/AEMFormsDemoListings/workflowcomponent/SaveFiles

Selecione o nó SaveFiles e examine suas propriedades

**componentGroup** - O valor dessa propriedade determina a categoria do componente do fluxo de trabalho.

**jcr:Title** - Este é o título do componente de fluxo de trabalho.

**sling:resourceSuperType** O valor dessa propriedade determinará a herança desse componente. Nesse caso, estamos herdando do componente de processo


![propriedades de componentes](assets/component-properties1.png)

## cq:dialog

As caixas de diálogo são usadas para permitir que o autor interaja com o componente. A caixa de diálogo cq:está sob o nó SaveFiles
![cq-dialog](assets/cq-dialog.png)

Os nós sob o nó itens representam as guias do componente por meio das quais os autores interagirão com o componente. As guias common e process estão ocultas. As guias Comum e Argumentos ficam visíveis.

Os argumentos do processo para o processo estão no nó processargs

![process-args](assets/process-arguments.png)

O autor especifica os argumentos conforme mostrado na captura de tela abaixo
![componente de fluxo de trabalho](assets/custom-workflow-component.png)

Os valores são armazenados como propriedades do nó de metadados. Por exemplo, o valor **c:\formsattachments** será armazenado na propriedade saveToLocation do nó de metadados
![local de salvamento](assets/save-to-location.png)

## cq:editConfig

O cq:EditConfig é simplesmente um nó com o tipo primário cq:EditConfig e o nome cq:editConfig sob a raiz do componente O comportamento de edição de um componente é configurado adicionando um nó cq:editConfig do tipo cq:EditConfig abaixo do nó do componente (do tipo cq:Component)

![edit-config](assets/cq-edit-config.png)

cq:formParameters (tipo de nó nt:unstructured): O define parâmetros adicionais que são adicionados ao formulário de diálogo.


Observe as propriedades do nó cq:formParameters
![from-parameters-properties](assets/form-parameters-properties.png)

O valor da propriedade PROCESS indica o código java que será associado ao componente do fluxo de trabalho.






