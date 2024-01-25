---
title: Criar tabelas de banco de dados
description: Criar banco de dados a ser usado pelo modelo de dados de formulário
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 1136244a-c3e6-45f6-8af8-eb3c100f838e
duration: 25
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 1%

---

# Criar tabelas de banco de dados

O modelo de dados de formulário pode ser baseado em fontes RDBMS, RESTfull, SOAP ou OData. O foco deste curso é preencher previamente o formulário adaptável usando o modelo de dados de formulário compatível com a fonte de dados RDBMS. Para o propósito deste tutorial, foi usado o banco de dados MYSQL. Criamos as duas tabelas a seguir para demonstrar o caso de uso

* **newhire** tabela - Esta tabela armazena as novas informações

  ![newhire](assets/newhire-table.png)


* **beneficiários** tabela - armazena novos beneficiários

  ![beneficiários](assets/beneficiaries-table.png)

Você pode importar o [arquivo sql](assets/db-schema.sql) usar o MySQL workbench para criar para tabelas com alguns dados de amostra.

## Próximas etapas

[Configurar modelo de dados do formulário](./configuring-form-data-model.md)
