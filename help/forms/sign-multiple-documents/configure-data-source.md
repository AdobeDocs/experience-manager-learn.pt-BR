---
title: Configurar fonte de dados AEM
description: Configurar fonte de dados com backup do MySQL para armazenar e recuperar dados de formulário
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 2%

---

# Configurar fonte de dados

Há muitas maneiras de AEM a integração com um banco de dados externo. Uma das maneiras mais comuns de integrar um banco de dados é usando as propriedades de configuração do Apache Sling Connection Pooling DataSource por meio do [configMgr](http://localhost:4502/system/console/configMgr).
A primeira etapa é baixar e implantar os [drivers MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) apropriados no AEM.
Crie a fonte de dados agrupados da conexão Apache Sling e forneça as propriedades conforme especificado na captura de tela abaixo. O schema do banco de dados é fornecido a você como parte dos ativos do tutorial.

![fonte de dados](assets/data-source.PNG)

O banco de dados tem uma tabela chamada formdata com as 3 colunas, conforme mostrado na captura de tela abaixo.

![base de dados](assets/data-base.PNG)


>[!NOTE]
>Certifique-se de nomear sua fonte de dados **aemformstutorial**. O código de amostra usa o nome para conectar-se ao banco de dados.

| Nome da Propriedade | Valor |
------------------------|---------------------------------------
| Nome da fonte de dados | SaveAndContinue |
| Classe de driver JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexão JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

## Ativos

O arquivo sql para criar o schema pode ser [baixado daqui](assets/sign-multiple-forms.sql). Será necessário importar esse arquivo usando a bancada MySql para criar o schema e a tabela.


