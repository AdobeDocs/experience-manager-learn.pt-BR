---
title: Inserir anexo do formulário no banco de dados
description: Inserir anexo de formulário no banco de dados usando o fluxo de trabalho AEM.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-10488
exl-id: e8a6cab8-423b-4a8e-b2b7-9b24ebe23834
last-substantial-update: 2020-06-09T00:00:00Z
duration: 114
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 1%

---

# Inserção do anexo do formulário no banco de dados

Este artigo abordará o caso de uso de armazenamento de anexos de formulário no banco de dados MySQL.

Uma tarefa comum dos clientes é armazenar os dados capturados do formulário e o anexo do formulário em uma tabela de banco de dados.
Para realizar esse caso de uso, as seguintes etapas foram seguidas

## Criar tabela de banco de dados para armazenar os dados do formulário e o anexo

Uma tabela chamada newhire foi criada para conter os dados de formulário. Observe a imagem do nome da coluna do tipo **LONGBLOB** para armazenar o anexo do formulário
![esquema de tabela](assets/insert-picture-table.png)

## Criar modelo de dados do formulário

Um modelo de dados de formulário foi criado para se comunicar com o banco de dados MySQL. Será necessário criar o seguinte

* [Fonte de dados JDBC no AEM](./data-integration-technical-video-setup.md)
* [Modelo de dados de formulário com base na fonte de dados JDBC](./jdbc-data-model-technical-video-use.md)

## Criar fluxo de trabalho

Ao configurar o formulário adaptável para enviar para um fluxo de trabalho AEM, você tem a opção de salvar os anexos do formulário em uma variável de fluxo de trabalho ou salvar os anexos em uma pasta especificada na carga. Para esse caso de uso, precisamos salvar os anexos em uma variável de fluxo de trabalho do tipo ArrayList of Document. Desse ArrayList, precisamos extrair o primeiro item e inicializar uma variável de documento. As variáveis de fluxo de trabalho chamadas **listOfDocuments** e **employeePhoto** foram criados.
Quando o formulário adaptável é enviado para acionar o fluxo de trabalho, uma etapa no fluxo de trabalho inicializará a variável employeePhoto usando o script ECMA. Veja a seguir o código de script ECMA

```javascript
log.info("executing script now...");
var metaDataMap = graniteWorkItem.getWorkflow().getWorkflowData().getMetaDataMap();
var listOfAttachments = [];
// Make sure you have a workflow variable caled listOfDocuments defined
listOfAttachments = metaDataMap.get("listOfDocuments");
log.info("$$$  got listOfAttachments");
//Make sure you have a workflow variable caled employeePhoto defined
var employeePhoto = listOfAttachments[0];
metaDataMap.put("employeePhoto", employeePhoto);
log.info("Employee Photo updated");
```

A próxima etapa do fluxo de trabalho é inserir dados e o anexo do formulário na tabela usando o componente de serviço Chamar modelo de dados de formulário.
![insert-pic](assets/fdm-insert-pic.png)
[O fluxo de trabalho completo com o exemplo de script ecma pode ser baixado daqui](assets/add-new-employee.zip).

>[!NOTE]
> Será necessário criar um novo modelo de dados de formulário baseado em JDBC e usar esse modelo de dados de formulário no fluxo de trabalho

## Criar formulário adaptável

Crie o formulário adaptável com base no modelo de dados de formulário criado na etapa anterior. Arraste e solte os elementos do modelo de dados de formulário no formulário. Configure o envio do formulário para acionar o fluxo de trabalho e especifique as seguintes propriedades, conforme mostrado na captura de tela abaixo
![anexos de formulário](assets/form-attachments.png)
