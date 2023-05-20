---
title: Extrair nó do XML de dados enviado
description: Etapa de processo personalizada para adicionar o documento de gravação localizado na pasta de carga útil ao sistema de arquivos
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
exl-id: 5282034f-275a-479d-aacb-fc5387da793d
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 0%

---

# Extrair nó do XML de dados enviado

Esta etapa do processo personalizado é criar um novo documento xml extraindo o nó de outro documento xml. Você precisaria usá-la quando quiser mesclar os dados enviados com o modelo xdp para gerar o pdf. Por exemplo, quando você envia um formulário adaptável, os dados que você precisa mesclar com o modelo xdp estão dentro do elemento de dados. Nesse caso, seria necessário criar outro documento xml extraindo o elemento de dados apropriado.

A captura de tela a seguir mostra os argumentos que você precisa passar para a etapa de processo personalizada
![etapa do processo](assets/create-xml-process-step.png)
Veja a seguir os parâmetros
* Data.xml - O arquivo xml do qual você deseja extrair o nó
* datatomerge.xml - O novo xml criado com o nó extraído
* /afData/afUnboundData/data - O nó a ser extraído


A captura de tela a seguir mostra o datamerge.xml que está sendo criado na pasta de carga útil
![create-xml](assets/create-xml.png)

[O pacote personalizado pode ser baixado aqui](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
