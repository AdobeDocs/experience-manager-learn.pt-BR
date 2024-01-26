---
title: Armazenamento e recuperação de dados de formulário do banco de dados MySQL - Configurar fonte de dados
description: Tutorial em várias partes para orientá-lo pelas etapas envolvidas no armazenamento e na recuperação de dados de formulário
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
duration: 49
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 1%

---

# Configurar fonte de dados

Há muitas maneiras com as quais o AEM permite a integração com o banco de dados externo. Uma das práticas mais comuns e padrão de integração de banco de dados é usar as propriedades de configuração da fonte de dados agrupada da conexão Apache Sling por meio da [configMgr](http://localhost:4502/system/console/configMgr).
A primeira etapa é baixar e implantar a solução [Drivers MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) no AEM.
Crie a fonte de dados agrupada da conexão Apache Sling e forneça as propriedades conforme especificado na captura de tela abaixo. O esquema de banco de dados é fornecido a você como parte deste tutorial de ativos.

![fonte de dados](assets/save-continue.PNG)

O banco de dados tem uma tabela chamada formdata com as 3 colunas conforme mostrado na captura de tela abaixo.

![data-base](assets/data-base-tables.PNG)

O arquivo sql para criar o esquema pode ser [baixado aqui](assets/form-data-db.sql). Você precisará importar esse arquivo usando o MySql workbench para criar o esquema e a tabela.

>[!NOTE]
>Nomeie sua fonte de dados **SaveAndContinue**. O código de amostra usa o nome para se conectar ao banco de dados.

| Nome da Propriedade | Valor |
| ------------------------|---------------------------------------|
| Nome da fonte de dados | SaveAndContinue |
| Classe de driver JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexão JDBC | jdbc:mysql://localhost:3306/aemformstutorial |
