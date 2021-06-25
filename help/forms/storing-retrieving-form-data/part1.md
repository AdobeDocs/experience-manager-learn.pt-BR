---
title: Armazenando e Recuperando Dados de Formulário do Banco de Dados MySQL
description: Tutorial de várias partes para orientá-lo pelas etapas envolvidas no armazenamento e recuperação de dados do formulário
version: 6.3,6.4,6.5
feature: Formulários adaptáveis
topic: Desenvolvimento
role: Developer
level: Experienced
source-git-commit: 3569d8b2a38d1cce02f6f4ff8b0c583f4dc118b6
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 3%

---

# Configurar fonte de dados

Há muitas maneiras de AEM a integração com o banco de dados externo. Uma das práticas mais comuns e padrão da integração do banco de dados é usar as propriedades de configuração do Apache Sling Pool DataSource por meio do [configMgr](http://localhost:4502/system/console/configMgr).
O primeiro passo é baixar e implantar os [drivers MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) apropriados em AEM.
Crie a fonte de dados agrupada da conexão Apache Sling e forneça as propriedades conforme especificado na captura de tela abaixo. O schema do banco de dados é fornecido a você como parte desses ativos tutoriais.

![fonte de dados](assets/save-continue.PNG)

O banco de dados tem uma tabela chamada formdata com as 3 colunas, conforme mostrado na captura de tela abaixo.

![base de dados](assets/data-base-tables.PNG)

O arquivo sql para criar o esquema pode ser [baixado aqui](assets/form-data-db.sql). Será necessário importar esse arquivo usando o MySql workbench para criar o schema e a tabela.

>[!NOTE]
>Certifique-se de nomear sua fonte de dados **SaveAndContinue**. O código de amostra usa o nome para se conectar ao banco de dados.

| Nome da Propriedade | Valor |
| ------------------------|---------------------------------------|
| Nome da origem de dados | SaveAndContinue |
| Classe de Driver JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexão JDBC | jdbc:mysql://localhost:3306/aemformstutorial |
