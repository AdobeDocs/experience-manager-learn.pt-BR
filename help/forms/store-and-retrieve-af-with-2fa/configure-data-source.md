---
title: Configurar o Data Source
description: Criar DataSource apontando para o banco de dados MySQL
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
duration: 64
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 3%

---

# Configurar o Data Source

Há muitas maneiras pelas quais o AEM permite a integração com um banco de dados externo. Uma das práticas mais comuns e padrão de integração de banco de dados é usar as propriedades de configuração de DataSource agrupada da conexão Apache Sling por meio do [configMgr](http://localhost:4502/system/console/configMgr).
A primeira etapa é baixar e implantar os [drivers MySQL](https://mvnrepository.com/artifact/mysql/mysql-connector-java) apropriados no AEM.
Em seguida, defina as propriedades da Fonte de dados agrupada da conexão do Sling específicas para seu banco de dados. A captura de tela a seguir mostra as configurações usadas para este tutorial. O esquema de banco de dados é fornecido a você como parte deste tutorial de ativos.

>[!NOTE]
>Nomeie sua fonte de dados `StoreAndRetrieveAfData`, pois este é o nome usado no serviço OSGi.


![fonte-de-dados](assets/data-source.JPG)

| Nome de propriedade | Valor de propriedade |   |
|---------------------|------------------------------------------------------------------------------------|---|
| Nome da fonte de dados | `StoreAndRetrieveAfData` |   |
| Classe de unidade JDBC | `jdbc:mysql://localhost:3306/aemformstutorial` |   |
| URI da conexão JDBC | `jdbc:mysql://localhost:3306/aemformstutorial?serverTimezone=UTC&autoReconnect=true` |   |
|                     |                                                                                    |   |


## Criar banco de dados


O seguinte banco de dados foi usado para este caso de uso. O banco de dados tem uma tabela chamada `formdatawithattachments` com as 4 colunas conforme mostrado na captura de tela abaixo.
![banco de dados](assets/table-schema.JPG)

* A coluna **afdata** conterá os dados de formulário adaptáveis.
* A coluna **attachmentsInfo** conterá as informações sobre os anexos de formulário.
* As colunas **telephoneNumber** conterão o número de celular da pessoa que está preenchendo o formulário.

Crie o banco de dados importando o [esquema de banco de dados](assets/data-base-schema.sql)
usando o MySQL workbench.

## Criar modelo de dados do formulário

Crie o modelo de dados de formulário e baseie-o na fonte de dados criada na etapa anterior.
Configure o serviço **get** desse modelo de dados de formulário conforme mostrado na captura de tela abaixo.
Verifique se você não está retornando uma matriz no serviço **get**.

A finalidade deste serviço **get** é buscar o número de telefone associado à identificação do aplicativo.

![obter-serviço](assets/get-service.JPG)

Este modelo de dados de formulário será usado no **MyAccountForm** para buscar o número de telefone associado à ID do aplicativo.

## Próximas etapas

[Escrever código para salvar anexos de formulário](./store-form-attachments.md)
