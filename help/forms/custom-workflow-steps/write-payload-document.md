---
title: Gravar o documento de conteúdo no sistema de arquivos
description: Etapa de processo personalizada para adicionar o documento de gravação localizado na pasta de carga útil ao sistema de arquivos
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
exl-id: bab7c403-ba42-4a91-8c86-90b43ca6026c
last-substantial-update: 2020-07-07T00:00:00Z
duration: 33
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Gravar o documento no sistema de arquivos

Um caso de uso comum é gravar os documentos gerados no fluxo de trabalho no sistema de arquivos.
Essa etapa do processo de fluxo de trabalho personalizado facilita a gravação dos documentos do fluxo de trabalho no sistema de arquivos.
O processo personalizado usa os seguintes argumentos separados por vírgula

```java
ChangeBeneficiary.pdf,c:\confirmation
```

O primeiro argumento é o nome do documento que você deseja salvar no sistema de arquivos. O segundo argumento é o local da pasta em que você deseja salvar o documento. Por exemplo, no caso de uso acima, o documento é gravado em `c:\confirmation\ChangeBeneficiary.pdf`

A captura de tela a seguir mostra os argumentos que você precisa passar para a etapa de processo personalizada
![write-payload-file-system](assets/write-payload-file-system.png)

[O pacote personalizado pode ser baixado aqui](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
