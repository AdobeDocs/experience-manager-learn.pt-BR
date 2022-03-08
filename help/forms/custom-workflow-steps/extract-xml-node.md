---
title: Gravar o documento de carga no sistema de arquivos
description: Etapa do processo personalizado para adicionar documento de gravação residente na pasta carga ao sistema de arquivos
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9860
source-git-commit: 160471fdc34439da6c312d65b252eaa941b7c7a2
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# Extrair nó do xml de dados enviados

Esta etapa do processo personalizado é criar um novo documento xml extraindo o nó de outro documento xml. Você precisaria usar isso quando quiser mesclar os dados enviados com o template xdp para gerar pdf. Por exemplo, ao enviar um formulário adaptável, os dados que você precisa mesclar com o modelo xdp estão dentro do elemento de dados. Nesse caso, seria necessário criar outro documento xml extraindo o elemento de dados apropriado.

A captura de tela a seguir mostra os argumentos que você precisa passar para a etapa do processo personalizado
![etapa do processo](assets/create-xml-process-step.png)
A seguir estão os parâmetros
* Data.xml - o arquivo xml do qual você deseja extrair o nó
* datatomerge.xml - o novo xml criado com o nó extraído
* /afData/afUnboundData/data - O nó a ser extraído


A seguinte captura de tela mostra o datamerge.xml que está sendo criado na pasta payload
![create-xml](assets/create-xml.png)

[O pacote personalizado pode ser baixado aqui](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)