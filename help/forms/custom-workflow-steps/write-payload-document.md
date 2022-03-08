---
title: Gravar o documento de carga no sistema de arquivos
description: Etapa do processo personalizado para adicionar documento de gravação residente na pasta carga ao sistema de arquivos
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
source-git-commit: 160471fdc34439da6c312d65b252eaa941b7c7a2
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Gravar o documento no sistema de arquivos

Caso de uso comum é gravar os documentos gerados no fluxo de trabalho para o sistema de arquivos.
Essa etapa personalizada do processo de fluxo de trabalho facilita a gravação dos documentos do fluxo de trabalho no sistema de arquivos.
O processo personalizado aceita os seguintes argumentos separados por vírgula

```java
ChangeBeneficiary.pdf,c:\confirmation
```

O primeiro argumento é o nome do documento que você deseja salvar no sistema de arquivos. O segundo argumento é o local da pasta em que você deseja salvar o documento. Por exemplo, no caso de uso acima, o documento será gravado em c:\confirmation\ChangeBeneficiary.pdf

A captura de tela a seguir mostra os argumentos que você precisa passar para a etapa do processo personalizado
![write-payload-file-system](assets/write-payload-file-system.png)

[O pacote personalizado pode ser baixado aqui](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)