---
title: Criar tabelas de banco de dados
description: Criar banco de dados a ser usado pelo modelo de dados de formulário
feature: Adaptive Forms
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 1136244a-c3e6-45f6-8af8-eb3c100f838e
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 1%

---

# Criar Tabelas de Banco de Dados

O modelo de dados de formulário pode ser baseado em fontes RDBMS, RESTfull, SOAP ou OData. O foco deste curso é no preenchimento prévio do formulário adaptável usando o modelo de dados de formulário apoiado pela fonte de dados RDBMS. Para a finalidade deste tutorial, foi usado o banco de dados MYSQL. Criamos as duas tabelas a seguir para demonstrar o caso de uso

* **newhire** tabela - Essa tabela armazena as informações completas

   ![newhire](assets/newhire-table.png)


* **beneficiários** tabela - Isso armazena novos beneficiários

   ![beneficiários](assets/beneficiaries-table.png)

É possível importar a variável [arquivo sql](assets/db-schema.sql) usando o MySQL workbench para criar em tabelas com alguns dados de amostra.

## Próximas etapas

[Configurar o modelo de dados de formulário](./configuring-form-data-model.md)
