---
title: Configurar fonte de dados
description: Criar DataSource apontando para o banco de dados MySQL
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 3%

---

# Configurar fonte de dados

Há muitas maneiras de AEM a integração com um banco de dados externo. Uma das práticas mais comuns e padrão da integração de banco de dados é usar as propriedades de configuração do DataSource Pool do Apache Sling Connection por meio do [configMgr](http://localhost:4502/system/console/configMgr).
A primeira etapa é baixar e implantar o [Drivers MySQL](https://mvnrepository.com/artifact/mysql/mysql-connector-java) para AEM.
Em seguida, defina as propriedades da fonte de dados agrupadas da conexão do Sling específicas ao seu banco de dados. A captura de tela a seguir mostra as configurações usadas para este tutorial. O schema do banco de dados é fornecido a você como parte desses ativos tutoriais.

![fonte de dados](assets/data-source.JPG)


* Classe de Driver JDBC: `com.mysql.cj.jdbc.Driver`
* URI da conexão JDBC: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>Certifique-se de nomear sua fonte de dados `StoreAndRetrieveAfData` como este é o nome usado no serviço OSGi.


## Criar banco de dados


O banco de dados a seguir foi usado para o propósito deste caso de uso. O banco de dados tem uma tabela chamada `formdatawithattachments` com as 4 colunas, conforme mostrado na captura de tela abaixo.
![base de dados](assets/table-schema.JPG)

* A coluna **afdata** manterá os dados do formulário adaptável.
* A coluna **attachmentInfo** manterá as informações sobre os anexos do formulário.
* As colunas **phoneNumber** manterá o número do celular da pessoa que preenche o formulário.

Crie o banco de dados importando o [esquema de banco de dados](assets/data-base-schema.sql)
usando o Workbench MySQL.

## Criar modelo de dados do formulário

Crie o modelo de dados de formulário e o baseie na fonte de dados criada na etapa anterior.
Configure o **get** serviço desse modelo de dados de formulário, conforme mostrado na captura de tela abaixo.
Certifique-se de que você não esteja retornando uma matriz no **get** serviço.

O objetivo deste **get** O serviço é buscar o número de telefone associado à id do aplicativo.

![get-service](assets/get-service.JPG)

Esse modelo de dados de formulário será usado no **MyAccountForm** para buscar o número de telefone associado à id do aplicativo.

## Próximas etapas

[Gravar código para salvar anexos de formulário](./store-form-attachments.md)
