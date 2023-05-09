---
title: Configurar fonte de dados AEM
description: Configurar a fonte de dados com backup do MySQL para armazenar e recuperar dados de formulário
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 5%

---

# Configurar fonte de dados

Há muitas maneiras de AEM a integração com um banco de dados externo. Uma das maneiras mais comuns de integrar um banco de dados é usando as propriedades de configuração do Apache Sling Connection Pool DataSource por meio do [configMgr](http://localhost:4502/system/console/configMgr).
A primeira etapa é baixar e implantar o [Drivers MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) em AEM.
Crie a fonte de dados agrupada da conexão Apache Sling e forneça as propriedades conforme especificado na captura de tela abaixo. O schema do banco de dados é fornecido a você como parte desses ativos tutoriais.

![fonte de dados](assets/data-source.PNG)

O banco de dados tem uma tabela chamada formdata com as 3 colunas, conforme mostrado na captura de tela abaixo.

![base de dados](assets/data-base.PNG)


>[!NOTE]
>Certifique-se de nomear sua fonte de dados **aemformstutorial**. O código de amostra usa o nome para se conectar ao banco de dados.

| Nome da Propriedade | Valor |
| ------------------------|--------------------------------------- |
| Nome da origem de dados | SaveAndContinue |
| Classe de Driver JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexão JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

O arquivo SQL para criar o schema pode ser [baixado aqui](assets/sign-multiple-forms.sql). Será necessário importar esse arquivo usando o MySql workbench para criar o schema e a tabela.

## Próximas etapas

[Criar um serviço OSGi para armazenar e buscar dados no banco de dados](./create-osgi-service.md)
