---
title: Configurar fonte de dados do AEM
description: Configurar a fonte de dados com backup do MySQL para armazenar e recuperar dados de formulário
feature: Formulários adaptáveis
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
topic: Desenvolvimento
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 4%

---

# Configurar fonte de dados

Há muitas maneiras com as quais o AEM habilita a integração com um banco de dados externo. Uma das maneiras mais comuns de integrar um banco de dados é usando as propriedades de configuração do Apache Sling Pool DataSource por meio do [configMgr](http://localhost:4502/system/console/configMgr).
O primeiro passo é baixar e implantar os [drivers MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) apropriados no AEM.
Crie a fonte de dados agrupada da conexão Apache Sling e forneça as propriedades conforme especificado na captura de tela abaixo. O schema do banco de dados é fornecido a você como parte desses ativos tutoriais.

![fonte de dados](assets/data-source.PNG)

O banco de dados tem uma tabela chamada formdata com as 3 colunas, conforme mostrado na captura de tela abaixo.

![base de dados](assets/data-base.PNG)


>[!NOTE]
>Certifique-se de nomear a fonte de dados **aemformstutorial**. O código de amostra usa o nome para se conectar ao banco de dados.

| Nome da Propriedade | Valor |
------------------------|---------------------------------------
| Nome da origem de dados | SaveAndContinue |
| Classe de Driver JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexão JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

## Ativos

O arquivo sql para criar o esquema pode ser [baixado aqui](assets/sign-multiple-forms.sql). Será necessário importar esse arquivo usando o MySql workbench para criar o schema e a tabela.


