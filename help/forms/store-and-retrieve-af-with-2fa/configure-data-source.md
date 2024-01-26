---
title: Configurar fonte de dados
description: Criar DataSource apontando para o banco de dados MySQL
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
duration: 87
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 3%

---

# Configurar fonte de dados

Há muitas maneiras pelas quais o AEM permite a integração com um banco de dados externo. Uma das práticas mais comuns e padrão de integração de banco de dados é usar as propriedades de configuração da fonte de dados agrupada da conexão Apache Sling por meio da [configMgr](http://localhost:4502/system/console/configMgr).
A primeira etapa é baixar e implantar a solução [Drivers MySQL](https://mvnrepository.com/artifact/mysql/mysql-connector-java) ao AEM.
Em seguida, defina as propriedades da Fonte de dados agrupada da conexão do Sling específicas para seu banco de dados. A captura de tela a seguir mostra as configurações usadas para este tutorial. O esquema de banco de dados é fornecido a você como parte deste tutorial de ativos.

>[!NOTE]
>Nomeie sua fonte de dados `StoreAndRetrieveAfData` pois esse é o nome usado no serviço OSGi.


![fonte de dados](assets/data-source.JPG)

| Nome da Propriedade | Valor da propriedade |   |
|---------------------|------------------------------------------------------------------------------------|---|
| Nome da fonte de dados | ArmazenarExecutarDadosAf |   |
| Classe de unidade JDBC | jdbc:mysql://localhost:3306/aemformstutorial |   |
| URI da conexão JDBC | jdbc:mysql://localhost:3306/aemformstutorial?serverTimezone=UTC&amp;autoReconnect=true |   |
|                     |                                                                                    |   |


## Criar banco de dados


O seguinte banco de dados foi usado para este caso de uso. O banco de dados tem uma tabela chamada `formdatawithattachments` com as 4 colunas conforme mostrado na captura de tela abaixo.
![data-base](assets/table-schema.JPG)

* A coluna **afdata** manterá os dados do formulário adaptável.
* A coluna **attachmentsInfo** manterá as informações sobre os anexos do formulário.
* As colunas **phoneNumber** manterá o número de celular da pessoa que preenche o formulário.

Crie o banco de dados importando o [esquema de banco de dados](assets/data-base-schema.sql)
usando o MySQL workbench.

## Criar modelo de dados do formulário

Crie o modelo de dados de formulário e baseie-o na fonte de dados criada na etapa anterior.
Configure o **obter** serviço deste modelo de dados de formulário, conforme mostrado na captura de tela abaixo.
Verifique se você não está retornando um storage no **obter** serviço.

O objetivo do presente **obter** serviço é buscar o número de telefone associado à id do aplicativo.

![get-service](assets/get-service.JPG)

Esse modelo de dados de formulário será usado no **FormuláriodeMinhaConta** para obter o número de telefone associado à id do aplicativo.

## Próximas etapas

[Escrever código para salvar anexos de formulário](./store-form-attachments.md)
