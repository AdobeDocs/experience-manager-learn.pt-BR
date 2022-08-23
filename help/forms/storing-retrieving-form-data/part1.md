---
title: Armazenamento e Recuperação de Dados de Formulário do Banco de Dados MySQL - Configurar Fonte de Dados
description: Tutorial de várias partes para orientá-lo pelas etapas envolvidas no armazenamento e recuperação de dados do formulário
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 4%

---

# Configurar fonte de dados

Há muitas maneiras de AEM a integração com o banco de dados externo. Uma das práticas mais comuns e padrão da integração de banco de dados é usar as propriedades de configuração do DataSource Pool do Apache Sling Connection por meio do [configMgr](http://localhost:4502/system/console/configMgr).
A primeira etapa é baixar e implantar o [Drivers MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) em AEM.
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
