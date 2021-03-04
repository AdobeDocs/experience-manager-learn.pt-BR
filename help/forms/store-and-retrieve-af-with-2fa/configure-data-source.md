---
title: Configurar fonte de dados
description: Criar DataSource apontando para o banco de dados MySQL
feature: Formulários adaptáveis
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Desenvolvimento
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 2%

---


# Configurar fonte de dados

Há muitas maneiras com as quais o AEM habilita a integração com o banco de dados externo. Uma das práticas mais comuns e padrão da integração do banco de dados é usar as propriedades de configuração do Apache Sling Pool DataSource por meio do [configMgr](http://localhost:4502/system/console/configMgr).
A primeira etapa é baixar e implantar os drivers [MySQL adequados](https://mvnrepository.com/artifact/mysql/mysql-connector-java) no AEM.
Em seguida, defina as propriedades da fonte de dados agrupadas da conexão do Sling específicas ao seu banco de dados. A captura de tela a seguir mostra as configurações usadas para este tutorial. O schema do banco de dados é fornecido a você como parte desses ativos tutoriais.

![fonte de dados](assets/data-source.JPG)


* Classe de Driver JDBC: `com.mysql.cj.jdbc.Driver`
* URI da conexão JDBC: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>Certifique-se de nomear sua fonte de dados `StoreAndRetrieveAfData`, pois esse é o nome usado no serviço OSGi.


## Criar banco de dados


O banco de dados a seguir foi usado para o propósito deste caso de uso. O banco de dados tem uma tabela chamada `formdatawithattachments` com as 4 colunas, conforme mostrado na captura de tela abaixo.
![base de dados](assets/table-schema.JPG)

* A coluna **afdata** manterá os dados do formulário adaptável.
* A coluna **attachmentInfo** manterá as informações sobre os anexos do formulário.
* As colunas **phoneNumber** terão o número de celular da pessoa que preenche o formulário.

Crie o banco de dados importando o [schema do banco de dados](assets/data-base-schema.sql)
usando o Workbench MySQL.

## Criar modelo de dados do formulário

Crie o modelo de dados de formulário e o baseie na fonte de dados criada na etapa anterior.
Configure o serviço **get** desse modelo de dados de formulário, conforme mostrado na captura de tela abaixo.
Certifique-se de que você não está retornando a matriz no serviço **get**.

Este serviço **get** é usado para buscar o número de telefone associado à ID do aplicativo.

![get-service](assets/get-service.JPG)

Esse modelo de dados de formulário será usado no **MyAccountForm** para buscar o número de telefone associado à ID do aplicativo.
