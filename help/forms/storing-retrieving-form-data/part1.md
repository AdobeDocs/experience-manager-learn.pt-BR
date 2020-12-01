---
title: Armazenamento e Recuperação de Dados de Formulário do Banco de Dados MySQL
description: Tutorial de várias peças para orientá-lo pelas etapas envolvidas no armazenamento e recuperação de dados do formulário
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 787a79663472711b78d467977d633e3d410803e5
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 1%

---

# Configurar fonte de dados

Há muitas maneiras de AEM a integração com o banco de dados externo. Uma das práticas mais comuns e padrão de integração de banco de dados é usar as propriedades de configuração da DataSource de pool de conexão Apache Sling por meio do [configMgr](http://localhost:4502/system/console/configMgr).
A primeira etapa é baixar e implantar os [drivers MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) apropriados no AEM.
Crie a fonte de dados agrupados da conexão Apache Sling e forneça as propriedades conforme especificado na captura de tela abaixo. O schema do banco de dados é fornecido a você como parte dos ativos do tutorial.

![fonte de dados](assets/save-continue.PNG)

O banco de dados tem uma tabela chamada formdata com as 3 colunas, conforme mostrado na captura de tela abaixo.

![base de dados](assets/data-base-tables.PNG)

O arquivo sql para criar o schema pode ser [baixado daqui](assets/form-data-db.sql). Será necessário importar esse arquivo usando a bancada MySql para criar o schema e a tabela.

>[!NOTE]
>Certifique-se de nomear sua fonte de dados **SaveAndContinue**. O código de amostra usa o nome para conectar-se ao banco de dados.

| Nome da Propriedade | Valor |
------------------------|---------------------------------------
| Nome da fonte de dados | SaveAndContinue |
| Classe de driver JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexão JDBC | jdbc:mysql://localhost:3306/aemformstutorial |


