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
source-git-commit: 6ae8110d4f4bc80682c35b0dab3fe7a62cad88f3
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Configurar fonte de dados

Há muitas maneiras de AEM a integração com o banco de dados externo. Uma das práticas mais comuns e padrão de integração de banco de dados é usar as propriedades de configuração da Fonte de Dados Pooling do Apache Sling Connection por meio do [configMgr](http://localhost:4502/system/console/configMgr).
A primeira etapa é baixar e implantar os drivers [apropriados do](https://mvnrepository.com/artifact/mysql/mysql-connector-java) My SQL no AEM.
Em seguida, defina as propriedades da Fonte de dados agrupada da conexão Sling. Essas propriedades são específicas ao seu banco de dados. A seguinte captura de tela mostra as configurações usadas para este tutorial. O schema do banco de dados é fornecido a você como parte dos ativos do tutorial.
![fonte de dados](assets/data-source.png)

O banco de dados tem uma tabela chamada formdata com as 3 colunas, conforme mostrado na captura de tela abaixo![da base de dados](assets/data-base-tables.PNG)
