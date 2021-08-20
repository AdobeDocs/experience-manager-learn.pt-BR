---
title: Criar tabelas de banco de dados
description: Criar banco de dados a ser usado pelo modelo de dados de formulário
feature: Formulários adaptáveis
version: 6.4,6.5
kt: 5811
thumbnail: kt-5811.jpg
topic: Desenvolvimento
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 2%

---


# Criar Tabelas de Banco de Dados

O modelo de dados de formulário pode ser baseado em fontes RDBMS, RESTfull, SOAP ou OData. O foco deste curso é no preenchimento prévio do formulário adaptável usando o modelo de dados de formulário apoiado pela fonte de dados RDBMS. Para a finalidade deste tutorial, foi usado o banco de dados MYSQL. Criamos as duas tabelas a seguir para demonstrar o caso de uso

* **** nwhiretable - Esta tabela armazena as informações completas

   ![newhire](assets/newhire-table.png)


* **** beneficiários - Isso armazena beneficiários novos

   ![beneficiários](assets/beneficiaries-table.png)

Você pode importar o [arquivo sql](assets/db-schema.sql) usando o Workbench MySQL para criar tabelas com alguns dados de amostra.