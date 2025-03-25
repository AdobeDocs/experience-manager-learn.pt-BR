---
title: Armazenamento e recuperação de dados de formulário do banco de dados MySQL - Configurar o Data Source
description: Tutorial em várias partes para orientá-lo pelas etapas envolvidas no armazenamento e na recuperação de dados de formulário
version: Experience Manager 6.4, Experience Manager 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
duration: 36
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 1%

---

# Configurar o Data Source

Há muitas maneiras pelas quais o AEM permite a integração com o banco de dados externo. Uma das práticas mais comuns e padrão de integração de banco de dados é usar as propriedades de configuração de DataSource agrupada da conexão Apache Sling por meio do [configMgr](http://localhost:4502/system/console/configMgr).
A primeira etapa é baixar e implantar os [drivers MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) apropriados no AEM.
Crie a fonte de dados agrupada da conexão Apache Sling e forneça as propriedades conforme especificado na captura de tela abaixo. O esquema de banco de dados é fornecido a você como parte deste tutorial de ativos.

![fonte-de-dados](assets/save-continue.PNG)

O banco de dados tem uma tabela chamada formdata com as 3 colunas conforme mostrado na captura de tela abaixo.

![banco de dados](assets/data-base-tables.PNG)

O arquivo SQL para criar o esquema pode ser [baixado daqui](assets/form-data-db.sql). Você precisará importar esse arquivo usando o MySql workbench para criar o esquema e a tabela.

>[!NOTE]
>Nomeie sua fonte de dados **SaveAndContinue**. O código de amostra usa o nome para se conectar ao banco de dados.

| Nome de propriedade | Valor |
| ------------------------|---------------------------------------|
| Nome da fonte de dados | `SaveAndContinue` |
| Classe de driver JDBC | `com.mysql.cj.jdbc.Driver` |
| URI de conexão JDBC | `jdbc:mysql://localhost:3306/aemformstutorial` |
