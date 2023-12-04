---
title: Configurar fonte de dados do AEM
description: Configurar a fonte de dados com suporte do MySQL para armazenar e recuperar dados de formulário
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: 6.4,6.5
jira: KT-6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
duration: 61
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 3%

---

# Configurar fonte de dados

Há muitas maneiras com as quais o AEM permite a integração com um banco de dados externo. Uma das maneiras mais comuns de integrar um banco de dados é usando as propriedades de configuração da fonte de dados agrupada da conexão Apache Sling por meio da [configMgr](http://localhost:4502/system/console/configMgr).
A primeira etapa é baixar e implantar a solução [Drivers MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) no AEM.
Crie a fonte de dados agrupada da conexão Apache Sling e forneça as propriedades conforme especificado na captura de tela abaixo. O esquema de banco de dados é fornecido a você como parte deste tutorial de ativos.

![fonte de dados](assets/data-source.PNG)

O banco de dados tem uma tabela chamada formdata com as 3 colunas conforme mostrado na captura de tela abaixo.

![data-base](assets/data-base.PNG)


>[!NOTE]
>Nomeie sua fonte de dados **aemformstutorial**. O código de amostra usa o nome para se conectar ao banco de dados.

| Nome da Propriedade | Valor |
| ------------------------|--------------------------------------- |
| Nome da fonte de dados | SaveAndContinue |
| Classe de driver JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexão JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

O arquivo SQL para criar o esquema pode ser [baixado aqui](assets/sign-multiple-forms.sql). Você precisará importar esse arquivo usando o MySql workbench para criar o esquema e a tabela.

## Próximas etapas

[Criar um serviço OSGi para armazenar e buscar dados no banco de dados](./create-osgi-service.md)
