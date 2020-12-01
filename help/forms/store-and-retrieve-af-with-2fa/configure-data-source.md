---
title: Configurar fonte de dados
description: Criar DataSource apontando para o banco de dados MySQL
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 1%

---


# Configurar fonte de dados

Há muitas maneiras de AEM a integração com o banco de dados externo. Uma das práticas mais comuns e padrão de integração de banco de dados é usar as propriedades de configuração da DataSource de pool de conexão Apache Sling por meio do [configMgr](http://localhost:4502/system/console/configMgr).
A primeira etapa é baixar e implantar os [drivers MySQL](https://mvnrepository.com/artifact/mysql/mysql-connector-java) apropriados para AEM.
Em seguida, defina as propriedades da Sling Connection Pooling DataSource específicas ao seu banco de dados. A seguinte captura de tela mostra as configurações usadas para este tutorial. O schema do banco de dados é fornecido a você como parte dos ativos do tutorial.

![fonte de dados](assets/data-source.JPG)


* Classe de driver JDBC: `com.mysql.cj.jdbc.Driver`
* URI de conexão JDBC: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>Certifique-se de nomear sua fonte de dados `StoreAndRetrieveAfData`, pois esse é o nome usado no serviço OSGi.


## Criar banco de dados


A seguinte base de dados foi utilizada para este caso de utilização. O banco de dados tem uma tabela chamada `formdatawithattachments` com as 4 colunas, conforme mostrado na captura de tela abaixo.
![base de dados](assets/table-schema.JPG)

* A coluna **afdata** manterá os dados do formulário adaptável.
* A coluna **attachmentsInfo** manterá as informações sobre os anexos do formulário.
* As colunas **phoneNumber** manterão o número de telefone da pessoa que preenche o formulário.

Crie o banco de dados importando o [schema do banco de dados](assets/data-base-schema.sql)
usando a bancada MySQL.

## Criar modelo de dados do formulário

Crie um modelo de dados de formulário e baseie-o na fonte de dados criada na etapa anterior.
Configure o serviço **get** deste modelo de dados de formulário como mostrado na captura de tela abaixo.
Certifique-se de que não está retornando o storage no serviço **get**.

Esse serviço **get** é usado para buscar o número de telefone associado à ID do aplicativo.

![get-service](assets/get-service.JPG)

Esse modelo de dados de formulário será usado em **MyAccountForm** para buscar o número de telefone associado à ID do aplicativo.
